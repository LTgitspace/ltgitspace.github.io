# Module 9: Monitoring, Logging & Alerts

## Module Objectives

- Setup CloudWatch Logs aggregation
- Configure CloudWatch Metrics & Dashboards
- Setup Alarms & Notifications
- Configure CloudTrail audit logging
- Enable X-Ray distributed tracing
- Implement cost monitoring

---

## Part 1: CloudWatch Logs

### Step 1: Create Log Groups

1. CloudWatch → Logs → Log groups
2. Create log groups:

```bash
# API Gateway logs
aws logs create-log-group \
  --log-group-name /aws/apigateway/smoking-cessation \
  --region ap-southeast-1

# Lambda logs (auto-created per function)
# Already created for each Lambda function

# RDS logs
aws logs create-log-group \
  --log-group-name /aws/rds/smoking-cessation-db \
  --region ap-southeast-1

# Application logs
aws logs create-log-group \
  --log-group-name /app/smoking-cessation \
  --region ap-southeast-1
```

### Step 2: Set Log Retention

For each log group, set retention policy:

```bash
aws logs put-retention-policy \
  --log-group-name /aws/apigateway/smoking-cessation \
  --retention-in-days 30 \
  --region ap-southeast-1
```

Suggested retention:
- Development: 7 days
- Production: 30-90 days
- Compliance: 1-3 years (use S3 for archival)

### Step 3: View Logs

1. CloudWatch → Logs → Log groups
2. Click log group
3. View log streams
4. Search logs: Search for error keywords

![CloudWatch Logs](../assets/09-cloudwatch-logs.png)

### Step 4: Create Log Insights Queries (Advanced)

Query examples:

```
# Find errors
fields @timestamp, @message
| filter @message like /error/i
| stats count() by @message

# API response times
fields @duration
| stats avg(@duration), max(@duration), pct(@duration, 99)

# Lambda invocations per minute
fields @timestamp
| stats count() as invocations by bin(5m)

# Database errors
fields @timestamp, @message, @logStream
| filter @message like /exception/i or @message like /error/i
| stats count() by @logStream
```

---

## Part 2: CloudWatch Metrics & Dashboard

### Step 1: Create Custom Dashboard

1. CloudWatch → Dashboards
2. Click "Create dashboard"
3. Name: "smoking-cessation-monitoring"
4. Add widgets:
   - API Gateway requests
   - Lambda errors
   - RDS CPU/connections
   - CloudFront requests
   - NLB connections

![Dashboard Creation](../assets/09-cloudwatch-dashboard.png)

### Step 2: Add Metrics Widgets

**API Gateway Metrics:**
- Count (total requests)
- 4XXError (client errors)
- 5XXError (server errors)
- Latency (p99, p95)

**Lambda Metrics:**
- Invocations
- Errors
- Duration
- Throttles

**RDS/EC2 Metrics:**
- CPU Utilization
- Database Connections
- Storage Space
- Network I/O

**CloudFront Metrics:**
- Requests
- BytesDownloaded
- BytesUploaded
- 4XXErrorRate
- 5XXErrorRate

---

## Part 3: Setup Alarms & Notifications

### Step 1: Create SNS Topic

```bash
aws sns create-topic \
  --name smoking-cessation-alerts \
  --region ap-southeast-1
```

### Step 2: Subscribe to Topic

```bash
aws sns subscribe \
  --topic-arn arn:aws:sns:ap-southeast-1:ACCOUNT_ID:smoking-cessation-alerts \
  --protocol email \
  --notification-endpoint your-email@example.com
```

### Step 3: Create Alarms

```bash
# API Gateway 5XX errors
aws cloudwatch put-metric-alarm \
  --alarm-name api-gateway-5xx-errors \
  --alarm-description "Alert on API 5XX errors" \
  --metric-name 5XXError \
  --namespace AWS/ApiGateway \
  --statistic Sum \
  --period 300 \
  --threshold 5 \
  --comparison-operator GreaterThanThreshold \
  --alarm-actions arn:aws:sns:ap-southeast-1:ACCOUNT_ID:smoking-cessation-alerts

# Lambda errors
aws cloudwatch put-metric-alarm \
  --alarm-name lambda-errors \
  --alarm-description "Alert on Lambda errors" \
  --metric-name Errors \
  --namespace AWS/Lambda \
  --statistic Sum \
  --period 300 \
  --threshold 1 \
  --comparison-operator GreaterThanOrEqualToThreshold \
  --alarm-actions arn:aws:sns:ap-southeast-1:ACCOUNT_ID:smoking-cessation-alerts

# EC2 CPU high
aws cloudwatch put-metric-alarm \
  --alarm-name ec2-cpu-high \
  --alarm-description "Alert when EC2 CPU exceeds 80%" \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --alarm-actions arn:aws:sns:ap-southeast-1:ACCOUNT_ID:smoking-cessation-alerts
```

---

## Part 4: CloudTrail Audit Logging

### Step 1: Enable CloudTrail

```bash
aws cloudtrail create-trail \
  --name smoking-cessation-trail \
  --s3-bucket-name smoking-cessation-audit-logs \
  --is-multi-region-trail \
  --region us-east-1

aws cloudtrail start-logging \
  --trail-name smoking-cessation-trail
```

### Step 2: Enable CloudTrail Insights

Detects unusual account activity:
- Unusual API calls
- Rapid resource creation
- Sudden spike in errors

---

## Part 5: X-Ray Distributed Tracing

### Step 1: Enable X-Ray for Lambda

```bash
# Update Lambda execution role to include X-Ray write access
aws iam attach-role-policy \
  --role-name LambdaExecutionRole \
  --policy-arn arn:aws:iam::aws:policy/AWSXRayDaemonWriteAccess
```

### Step 2: Enable X-Ray for API Gateway

1. API Gateway Console
2. Click Stages
3. Select stage
4. Enable X-Ray tracing

### Step 3: View Traces

1. X-Ray Console
2. View service map
3. Analyze request latency
4. Identify bottlenecks

---

## Part 6: Cost Monitoring

### Step 1: Enable Cost Allocation Tags

Tag all resources:
```bash
Environment: production/staging/development
Project: smoking-cessation
Team: backend/frontend
```

### Step 2: Create Cost Budget

```bash
aws budgets create-budget \
  --account-id ACCOUNT_ID \
  --budget file://budget.json
```

### Step 3: Analyze Costs

1. Cost Explorer Console
2. Group by: Service, Region, Tag
3. View trends
4. Identify cost drivers
5. Optimize expensive services

**Common cost optimization:**
- Reserved Instances for EC2 (20-40% savings)
- S3 Intelligent-Tiering (automatic cost optimization)
- Lambda cost optimization (memory tuning)
- RDS reserved instances or switch to Aurora serverless

