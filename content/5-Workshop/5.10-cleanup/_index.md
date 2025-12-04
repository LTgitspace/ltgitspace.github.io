# Module 10: Cleanup & Cost Optimization

## Module Objectives

- Identify unused/underutilized resources
- Delete test resources
- Optimize costs
- Archive important data
- Backup before deletion
- Documentation & lessons learned

---

## Part 1: Resource Inventory & Assessment

### Current AWS Resources

Run commands to list all resources:

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

# List all EC2 instances
aws ec2 describe-instances --region ap-southeast-1 --output json
```

Create inventory spreadsheet:
- Resource name & type
- Region
- Creation date
- Current usage
- Estimated monthly cost

---

## Part 2: Identify Unused Resources

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

#### EC2 Instances

Check CPU utilization:

```bash
# Check average CPU last 7 days
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name CPUUtilization \
  --dimensions Name=InstanceId,Value=i-0d82a626b99a2fecd \
  --start-time $(date -d '7 days ago' -u +%Y-%m-%dT%H:%M:%S) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
  --period 86400 \
  --statistics Average \
  --region ap-southeast-1
```

If average CPU < 5% = underutilized instance (consider smaller type or termination)

---

## Part 3: Cost Optimization Strategies

### 1. Reserved Instances

For production EC2:

```bash
# Buy 1-year reserved instance for 30-40% savings
aws ec2 purchase-reserved-instances-offering \
  --reserved-instances-offering-id <offering-id> \
  --instance-count 1
```

### 2. S3 Intelligent-Tiering

Automatically moves objects to cheaper storage classes:

```bash
# Enable Intelligent-Tiering
aws s3api put-bucket-intelligent-tiering-configuration \
  --bucket leaflungs-frontend-new \
  --id AutoTier \
  --intelligent-tiering-configuration file://intelligent-tiering.json
```

### 3. Lambda Optimization

```bash
# Reduce memory = reduce cost
# Analyze CloudWatch metrics to find optimal memory setting

# Memory cost calculation:
# Cost per GB-second = $0.0000166667
# 512 MB × 1 second = 0.5 GB-second × $0.0000166667 = $0.0000083335
```

### 4. RDS Cost Optimization

Options:
- Use Aurora Serverless (pay per second)
- Use AWS Database Migration Service for cheaper databases
- Enable auto-scaling

### 5. NAT Gateway Optimization

NAT Gateway costs $0.045/hour + data transfer:
- Option 1: Use NAT Instance instead (cheaper but less reliable)
- Option 2: Use VPC Endpoints to avoid NAT Gateway for AWS services

### 6. Data Transfer Costs

```bash
# Minimize data transfer out:
# - Use S3 Transfer Acceleration
# - Use CloudFront for static content
# - Use VPC Endpoints to avoid internet gateway charges
# - Use EC2 placement groups for lower inter-AZ data transfer
```

---

## Part 4: Backup Before Deletion

### EBS Snapshots

```bash
# Create snapshots of all EBS volumes
aws ec2 describe-volumes \
  --filters "Name=availability-zone,Values=ap-southeast-1a" \
  --query 'Volumes[*].{ID:VolumeId,Size:Size}' \
  --region ap-southeast-1

# Create snapshot
aws ec2 create-snapshot \
  --volume-id vol-xxxxx \
  --description "Backup before cleanup $(date +%Y-%m-%d)" \
  --region ap-southeast-1
```

### RDS Snapshots

```bash
# Create RDS snapshot before deletion
aws rds create-db-snapshot \
  --db-instance-identifier smoking-cessation-db \
  --db-snapshot-identifier smoking-cessation-db-backup-$(date +%Y%m%d) \
  --region ap-southeast-1
```

### S3 Bucket Backups

```bash
# Copy important data to archive bucket
aws s3 sync \
  s3://leaflungs-frontend-new \
  s3://leaflungs-archive-2024 \
  --region us-east-1
```

---

## Part 5: Cleanup Process

### 1. Stop EC2 Instances (if not deleting yet)

```bash
aws ec2 stop-instances \
  --instance-ids i-0d82a626b99a2fecd i-0374ff6972fd306fe \
  --region ap-southeast-1
```

### 2. Delete Non-Critical Resources

```bash
# Delete test Lambda functions
aws lambda delete-function \
  --function-name TestFunction \
  --region ap-southeast-1

# Delete unused API endpoints
aws apigateway delete-rest-api \
  --rest-api-id unused-api-id \
  --region ap-southeast-1
```

### 3. Delete CloudFormation Stacks

```bash
aws cloudformation delete-stack \
  --stack-name PLOnpremSetup \
  --region us-east-1
```

### 4. Empty and Delete S3 Buckets

```bash
# Empty bucket
aws s3 rm s3://leaflungs-temp-bucket --recursive

# Delete bucket
aws s3api delete-bucket \
  --bucket leaflungs-temp-bucket \
  --region us-east-1
```

---

## Part 6: Documentation & Lessons Learned

### Document Your Setup

Create a runbook including:

1. **Architecture Overview**
   - Diagram of all components
   - Service dependencies

2. **Deployment Instructions**
   - Step-by-step deployment guide
   - Required AWS services
   - Cost estimates

3. **Operational Procedures**
   - How to scale up/down
   - How to troubleshoot common issues
   - How to perform backups

4. **Security Checklist**
   - IAM policies
   - VPC security groups
   - Data encryption requirements

5. **Cost Optimization Tips**
   - Identified cost drivers
   - Recommendations for savings
   - Monitoring and alerts

### Lessons Learned

Document:
- What worked well
- What could be improved
- Performance bottlenecks identified
- Cost surprises
- Security issues discovered
- Team feedback

This documentation will be invaluable for future projects and knowledge sharing.
# Module 8: Verify VPC & Security Configuration

## Module Objectives

- Verify VPC setup
- Review Security Groups configuration
- Verify Network ACLs
- Configure NLB for WebSocket
- Setup VPN/Bastion (optional)
- Review IAM policies

---

## Part 1: Verify VPC

### Current Status

VPC:
- ID: vpc-046dc916dde2fb93f
- Name: project-bundau-milo
- CIDR: 172.0.0.0/18
- Region: ap-southeast-1

### Step 1: Access VPC Console

1. VPC service
2. Click "VPCs"
3. Click on "project-bundau-milo"

![VPC Dashboard](../assets/08-vpc-dashboard.png)

### Step 2: Review Subnets

1. Click "Subnets" (left menu)
2. Verify:
   - Public subnets (for ALB/NLB)
   - Private subnets (for Lambda, RDS)
   - Availability Zones (2-3 for HA)

Expected structure:
```
Public Subnets (route to Internet Gateway)
├── ap-southeast-1a: 172.0.0.0/24
└── ap-southeast-1b: 172.0.1.0/24

Private Subnets (route via NAT Gateway)
├── ap-southeast-1a: 172.0.10.0/24
├── ap-southeast-1b: 172.0.11.0/24
└── ap-southeast-1c: 172.0.12.0/24
```

![Subnets](../assets/08-vpc-subnets.png)

### Step 3: Review Internet Gateway

1. Click "Internet Gateways" (left menu)
2. Verify IGW attached to VPC
3. Should see route in public subnet:
   - Destination: 0.0.0.0/0
   - Target: Internet Gateway

### Step 4: Review NAT Gateway (for private subnets)

1. Click "NAT Gateways" (left menu)
2. Verify NAT Gateway in public subnet
3. Check Elastic IP assigned
4. Verify route in private subnets:
   - Destination: 0.0.0.0/0
   - Target: NAT Gateway

---

## Part 2: Verify Security Groups

### Current Security Groups

| Group ID | Name | VPC | Purpose |
|----------|------|-----|---------|
| sg-0cf34158cb7f5440b | leaflungs-backend-sg | project-bundau-milo | EC2 Application servers + Lambda |
| sg-0adb5d7ea1fe0f6bb | leaflungs-nlb-sg | project-bundau-milo | NLB (WebSocket) |
| sg-027ef04aa3c769ecf | leaflungs-db-sg | project-bundau-milo | EC2 Database servers (PostgreSQL + MongoDB) |

### Step 1: Review leaflungs-backend-sg

1. VPC → Security Groups
2. Click "leaflungs-backend-sg"

Inbound rules (should have):
```
HTTPS (443) from leaflungs-nlb-sg
MySQL/PostgreSQL (3306/5432) to leaflungs-db-sg
```

Outbound rules:
```
All traffic (0.0.0.0/0)
```

### Step 2: Review leaflungs-nlb-sg

Inbound rules (should have):
```
HTTPS (443) from 0.0.0.0/0 (any source)
HTTP (80) from 0.0.0.0/0 (redirects to HTTPS)
```

Outbound rules:
```
All traffic to leaflungs-backend-sg
```

### Step 3: Review leaflungs-db-sg

Inbound rules:
```
PostgreSQL (5432) from leaflungs-backend-sg
MongoDB (27017) from leaflungs-backend-sg
```

Outbound rules:
```
All traffic (0.0.0.0/0)
```

---

## Part 3: Configure Network ACLs (NACLs)

### Step 1: Access Network ACLs

1. VPC → Network ACLs
2. Review rules for each subnet

### Step 2: Verify NACL Rules

Expected inbound rules:
```
Rule #	Protocol	Port Range	Source		Reason
100	TCP		443		0.0.0.0/0	HTTPS (frontend)
110	TCP		80		0.0.0.0/0	HTTP (redirect)
120	TCP		1024-65535	0.0.0.0/0	Ephemeral ports
```

Expected outbound rules:
```
Rule #	Protocol	Port Range	Destination	Reason
100	TCP		All		0.0.0.0/0	All outbound
```

---

## Part 4: Configure NLB for WebSocket

### Step 1: Access Network Load Balancer

1. EC2 → Load Balancers
2. Click on "leaflungs-nlb" (if exists)

### Step 2: Verify Listener Configuration

Target Group:
- Health Check Path: /health
- Protocol: HTTPS
- Port: 443
- Target Instances: user-cessation, social-media

### Step 3: Enable WebSocket Support

Ensure:
- Connection draining: 300 seconds
- Deregistration delay: 300 seconds
- Preserve client IP: Enabled

---

## Part 5: Review IAM Policies

### EC2 Instance Role

EC2 instances should have IAM role with permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::leaflungs-*/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:*:*:*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:PutMetricData"
      ],
      "Resource": "*"
    }
  ]
}
```

---

## Part 6: Security Best Practices

1. **Enable Flow Logs**
   ```bash
   aws ec2 create-flow-logs \
     --resource-type VPC \
     --resource-ids vpc-046dc916dde2fb93f \
     --traffic-type ALL \
     --log-destination-type cloud-watch-logs
   ```

2. **Enable VPC Endpoint for S3**
   - Avoids data transfer costs
   - Keeps traffic private

3. **Use VPC Peering** (if needed)
   - Connect to on-premises environment
   - Use VPN or Direct Connect

4. **Monitor with VPC Flow Logs**
   - Detect anomalous traffic patterns
   - Troubleshoot connectivity issues

