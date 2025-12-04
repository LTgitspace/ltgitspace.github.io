# Module 10: Cleanup & Cost Optimization

## Mục tiêu Module

- Identify unused/underutilized resources
- Delete test resources
- Optimize costs
- Archive important data
- Backup before deletion
- Documentation & lessons learned

---

## Phần 1: Resource Inventory & Assessment

### Current AWS Resources

Run commands để list tất cả resources:

```bash
# List all Lambda functions
aws lambda list-functions --region us-east-1 --output json
aws lambda list-functions --region ap-southeast-1 --output json

# List all API Gateways
aws apigateway get-rest-apis --region ap-southeast-1 --output json

# List all RDS instances
aws rds describe-db-instances --region ap-southeast-1 --output json

# List all S3 buckets
aws s3api list-buckets --output json

# List all CloudFront distributions
aws cloudfront list-distributions --output json

# List all VPC resources
aws ec2 describe-vpcs --region ap-southeast-1 --output json
```

Create inventory spreadsheet:
- Resource name & type
- Region
- Creation date
- Current usage
- Estimated monthly cost

---

## Phần 2: Identify Unused Resources

### Check Resource Usage

#### Lambda Functions

```bash
# Check Lambda invocations last 7 days
aws cloudwatch get-metric-statistics \
  --namespace AWS/Lambda \
  --metric-name Invocations \
  --dimensions Name=FunctionName,Value=AdminManageCoachesFunction \
  --start-time $(date -d '7 days ago' -u +%Y-%m-%dT%H:%M:%S) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
  --period 86400 \
  --statistics Sum \
  --region ap-southeast-1
```

If no invocations in last 7 days = unused function (consider deletion)

#### API Gateways

```bash
# Check API request count
aws cloudwatch get-metric-statistics \
  --namespace AWS/ApiGateway \
  --metric-name Count \
  --dimensions Name=ApiName,Value=LeafLungs-UserInfo-API \
  --start-time $(date -d '30 days ago' -u +%Y-%m-%dT%H:%M:%S) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
  --period 2592000 \
  --statistics Sum \
  --region ap-southeast-1
```

#### S3 Buckets

Check bucket sizes:

```bash
# Get bucket size
aws s3api list-objects-v2 \
  --bucket leaflungs-frontend-new \
  --query 'Contents[*].Size' \
  --output json | jq 'add/1024/1024/1024'  # Size in GB
```

Check S3 storage class:
- Standard: $0.023/GB/month
- Infrequent Access: $0.0125/GB/month
- Glacier: $0.004/GB/month

Move old objects to cheaper storage classes

#### CloudFront

Check cache hit ratio:

```bash
# If cache hit ratio < 50%, may need optimization
aws cloudwatch get-metric-statistics \
  --namespace AWS/CloudFront \
  --metric-name CacheHitRate \
  --dimensions Name=DistributionId,Value=E1NREZDKTJH6Y9 \
  --start-time $(date -d '30 days ago' -u +%Y-%m-%dT%H:%M:%S) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
  --period 2592000 \
  --statistics Average \
  --region us-east-1
```

#### RDS

Check CPU & connection usage:

```bash
aws cloudwatch get-metric-statistics \
  --namespace AWS/RDS \
  --metric-name CPUUtilization \
  --dimensions Name=DBInstanceIdentifier,Value=smoking-cessation-db \
  --start-time $(date -d '30 days ago' -u +%Y-%m-%dT%H:%M:%S) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
  --period 2592000 \
  --statistics Average,Maximum \
  --region ap-southeast-1
```

If CPU < 10% consistently: May downsize instance type (saves money)

---

## Phần 3: Cost Optimization Strategies

### 1. Right-Sizing Instances

Current: db.t3.small RDS
Cost: ~$40/month

Options:
- db.t3.micro: ~$20/month (if usage low)
- db.t4g.micro: ~$15/month (Graviton processor, cheaper)

Check: RDS CPU < 10% on average → downsize

### 2. Reserve Capacity (if stable usage)

For production, consider Reserved Instances:
- 1-year: ~30% discount
- 3-year: ~50% discount

Example:
```bash
# Purchase 1-year reserved RDS instance
aws rds purchase-reserved-db-instances-offering \
  --reserved-db-instances-offering-id <OFFERING_ID> \
  --db-instance-count 1 \
  --region ap-southeast-1
```

### 3. Optimize Data Transfer

Costs:
- S3 to CloudFront: Free
- S3 to Internet: $0.09/GB
- S3 to Lambda (same region): Free

Recommendation: Keep CloudFront caching enabled

### 4. Lambda Optimization

Current: 256 MB average
Cost: ~$0.0000002 per invocation

If high invocation rate:
- Use Lambda provisioned concurrency: Higher cost but predictable
- Monitor: Check if memory increase helps reduce duration (saves cost)

### 5. Database Optimization

Recommendations:
- Enable automated backup (already done)
- Setup read replicas if scaling reads (cost: ~50% of main instance)
- Archive old logs (CloudWatch retention: 30 days by default)

### 6. CloudFront Optimization

If cache hit ratio < 50%:
1. Increase TTL for static assets
2. Add more cache behaviors
3. Use cache policies with parameters

---

## Phần 4: Backup & Archive

### Bước 1: Backup RDS

```bash
# Create manual snapshot before deletion/modification
aws rds create-db-snapshot \
  --db-instance-identifier smoking-cessation-db \
  --db-snapshot-identifier smoking-cessation-db-final-backup-$(date +%s) \
  --region ap-southeast-1
```

### Bước 2: Export RDS Data to S3

```bash
aws rds start-export-task \
  --export-task-identifier smoking-cessation-export-$(date +%s) \
  --source-arn arn:aws:rds:ap-southeast-1:140570829989:db:smoking-cessation-db \
  --s3-bucket-name smoking-cessation-backups \
  --s3-prefix rds-export/ \
  --iam-role-arn arn:aws:iam::140570829989:role/rds-export-role \
  --region ap-southeast-1
```

### Bước 3: Archive S3 Data

For old data (> 90 days):

```bash
# Transition S3 objects to Glacier
aws s3api put-bucket-lifecycle-configuration \
  --bucket leaflungs-images \
  --lifecycle-configuration '{
    "Rules": [{
      "ID": "archive-old-data",
      "Filter": {"Prefix": "chat/"},
      "Transitions": [{
        "Days": 90,
        "StorageClass": "GLACIER"
      }],
      "Expiration": {
        "Days": 365
      }
    }]
  }' \
  --region ap-southeast-1
```

---

## Phần 5: Test Resource Cleanup (Non-Production)

### Bước 1: Delete Test Lambda Functions

If any test functions exist:

```bash
aws lambda delete-function \
  --function-name test-function-name \
  --region ap-southeast-1
```

### Bước 2: Delete Unused API Gateway

If multiple APIs for testing:

```bash
aws apigateway delete-rest-api \
  --rest-api-id api-id-to-delete \
  --region ap-southeast-1
```

### Bước 3: Delete Empty S3 Buckets

```bash
# Empty bucket first
aws s3 rm s3://test-bucket-name --recursive

# Delete bucket
aws s3api delete-bucket \
  --bucket test-bucket-name \
  --region ap-southeast-1
```

### Bước 4: Delete Unused CloudFront Distributions

Before deletion, disable distribution:

```bash
aws cloudfront update-distribution \
  --id E1NREZDKTJH6Y9 \
  --distribution-config file://dist-config.json
```

Wait 15 minutes, then delete

---

## Phần 6: Delete Production Resources (if shutting down)

WARNING: This is destructive and cannot be undone!

### Backup Checklist Before Deletion

- [ ] RDS snapshot created
- [ ] Data exported to S3
- [ ] S3 data backed up externally
- [ ] CloudTrail logs reviewed & archived
- [ ] Code backed up to GitHub
- [ ] DNS records updated (if redirecting)
- [ ] Team notified

### Bước 1: Delete RDS Instance

```bash
aws rds delete-db-instance \
  --db-instance-identifier smoking-cessation-db \
  --skip-final-snapshot \
  --region ap-southeast-1
```

Or with final snapshot:

```bash
aws rds delete-db-instance \
  --db-instance-identifier smoking-cessation-db \
  --final-db-snapshot-identifier smoking-cessation-final-snapshot \
  --region ap-southeast-1
```

### Bước 2: Delete Lambda Functions

```bash
aws lambda delete-function \
  --function-name AdminManageCoachesFunction \
  --region ap-southeast-1

# Repeat for other functions
```

### Bước 3: Delete API Gateways

```bash
aws apigateway delete-rest-api \
  --rest-api-id v7agf76rrh \
  --region ap-southeast-1

aws apigateway delete-rest-api \
  --rest-api-id vuds39de1b \
  --region ap-southeast-1
```

### Bước 4: Delete Cognito User Pool

```bash
aws cognito-idp delete-user-pool \
  --user-pool-id us-east-1_dskUsnKt3 \
  --region us-east-1
```

### Bước 5: Delete S3 Buckets (and contents)

```bash
# Empty bucket
aws s3 rm s3://leaflungs-frontend-new --recursive
aws s3 rm s3://leaflungs-images --recursive
aws s3 rm s3://leaflungs-images-sg --recursive

# Delete buckets
aws s3api delete-bucket --bucket leaflungs-frontend-new
aws s3api delete-bucket --bucket leaflungs-images --region ap-southeast-1
aws s3api delete-bucket --bucket leaflungs-images-sg --region ap-southeast-1
```

### Bước 6: Delete CloudFront Distribution

```bash
aws cloudfront delete-distribution \
  --id E1NREZDKTJH6Y9
```

---

## Phần 7: Cost Analysis & Reporting

### Monthly Cost Breakdown

Typical costs for this architecture:

| Service | Usage | Cost |
|---------|-------|------|
| Lambda | 100K invocations/month | ~$2 |
| API Gateway | 10M requests/month | ~$50 |
| RDS | db.t3.small, 30 day backup | ~$40 |
| S3 | 100 GB storage | ~$2 |
| CloudFront | 1 TB/month transfer | ~$85 |
| Cognito | 1K users | ~$0 (free tier) |
| NAT Gateway | 10 GB transfer | ~$5 |
| **Total** | | ~**$184/month** |

Cost optimization opportunities:
1. Use db.t4g.micro: Saves ~$25/month
2. Cache more aggressively: Saves ~$30/month
3. Reserved RDS: Saves ~$12/month
4. Total savings: ~$67/month (36% reduction)

### AWS Cost Explorer

1. AWS Console → Billing → Cost Explorer
2. View costs by:
   - Service
   - Linked account
   - Region
   - Usage type
3. Set up cost anomaly detection
4. Review trends

---

## Phần 8: Documentation & Lessons Learned

### Create Post-Mortem Document

```markdown
# Workshop Completion Report

## Architecture Summary
- Services deployed: Lambda, API Gateway, RDS, S3, CloudFront, Cognito, NLB
- Regions: us-east-1 (Cognito), ap-southeast-1 (main)
- Total users: 1000+
- Monthly costs: $184

## Key Learnings

1. Regional considerations
   - Cognito must be in us-east-1 for CloudFront
   - Main services in ap-southeast-1 for latency

2. Security best practices implemented
   - VPC isolation
   - Security group restrictions
   - IAM least-privilege
   - Secrets Manager for credentials

3. Cost optimization
   - CloudFront caching important
   - RDS can be right-sized
   - Reserved instances save 30-50%

4. Monitoring critical
   - CloudWatch alarms prevented many issues
   - X-Ray helped debug performance

## Recommendations for Next Phase

1. Enable auto-scaling for RDS read replicas
2. Implement CI/CD for automated deployments
3. Add comprehensive test suite
4. Setup on-call rotation & runbooks
5. Quarterly cost reviews
```

---

## Checklist - Cleanup Decision

Before final cleanup, verify:

- [ ] All data backed up
- [ ] Snapshots created
- [ ] CloudTrail logs archived
- [ ] Code pushed to GitHub
- [ ] Cost analysis completed
- [ ] Team trained on infrastructure
- [ ] Documentation complete
- [ ] Stakeholders approved
- [ ] DNS cutover plan (if applicable)
- [ ] Rollback plan documented

---

## Final Recommendations

1. Keep infrastructure running (you've built it correctly)
2. Implement automated scaling if needed
3. Setup CI/CD pipeline for updates
4. Add comprehensive testing
5. Plan quarterly cost reviews
6. Train team on operational procedures
7. Consider disaster recovery plan

---

## Summary & Next Steps

Congratulations! You've successfully:

1. ✓ Verified/setup Cognito authentication
2. ✓ Verified/setup Lambda functions
3. ✓ Verified/setup API Gateways
4. ✓ Created RDS database
5. ✓ Verified S3 & CloudFront
6. ✓ Verified VPC & Security
7. ✓ Implemented monitoring & logging
8. ✓ Optimized costs

This infrastructure can support:
- 1000+ concurrent users
- 100K+ requests/day
- Highly available (multi-AZ capable)
- Scalable (auto-scale Lambda & RDS replicas)
- Secure (VPC isolation, encryption, IAM)
- Observable (comprehensive logging & monitoring)

---

## Kết Quả Đạt Được

Sau Module 10:

1. Resource inventory & usage analyzed
2. Cost optimization strategies identified
3. Unused resources identified for cleanup
4. Backup procedures implemented
5. Cost reduction plan created (up to 36% savings possible)
6. Post-mortem & lessons learned documented
7. Recommendations for next phase
8. Workshop complete & infrastructure production-ready
