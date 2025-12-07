# Module 9: Monitoring, Logging & Alerts

## Mục tiêu Module

- Setup CloudWatch Logs aggregation
- Configure CloudWatch Metrics & Dashboards
- Setup Alarms & Notifications
- Configure CloudTrail audit logging
- Enable X-Ray distributed tracing
- Implement cost monitoring

---

## Phần 1: CloudWatch Logs

### Bước 1: Create Log Groups

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
```

### Bước 2: Set Log Retention

For each log group, set retention policy:

```bash
aws logs put-retention-policy \
  --log-group-name /aws/apigateway/smoking-cessation \
  --retention-in-days 30 \
  --region ap-southeast-1
```

### Bước 3: View Logs

1. CloudWatch → Logs → Log groups
2. Click log group
3. View log streams
4. Search logs: Search for error keywords

![CloudWatch Logs](../assets/09-cloudwatch-logs.png)

### Bước 4: Create Log Insights Queries (Advanced)

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
```

---

## Phần 2: CloudWatch Metrics & Dashboard

### Bước 1: Create Custom Dashboard

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

### Bước 2: Add Metrics Widgets

For each service, add relevant metrics:

API Gateway:
- Count (total requests)
- 4XXError (client errors)
- 5XXError (server errors)
- Latency (p99, p95)

Lambda:
- Invocations (total calls)
- Duration (execution time)
- Errors (failed invocations)
- Throttles (rate limit hits)
- ConcurrentExecutions (parallel runs)

RDS:
- CPUUtilization
- DatabaseConnections
- ReadLatency, WriteLatency
- ReadThroughput, WriteThroughput
- NetworkTransmitThroughput

CloudFront:
- Requests (total)
- BytesDownloaded
- 4xxErrorRate, 5xxErrorRate
- CacheHitRate

### Bước 3: Configure Auto-Refresh

1. Dashboard
2. Settings
3. Auto-refresh: 1 minute

---

## Phần 3: CloudWatch Alarms

### Bước 1: Create Lambda Error Alarm

```bash
aws cloudwatch put-metric-alarm \
  --alarm-name lambda-errors-smoking-cessation \
  --alarm-description "Lambda function errors" \
  --metric-name Errors \
  --namespace AWS/Lambda \
  --statistic Sum \
  --period 300 \
  --threshold 5 \
  --comparison-operator GreaterThanOrEqualToThreshold \
  --evaluation-periods 1 \
  --dimensions Name=FunctionName,Value=AdminManageCoachesFunction \
  --alarm-actions arn:aws:sns:ap-southeast-1:140570829989:email-notifications \
  --region ap-southeast-1
```

### Bước 2: Create API Gateway Errors Alarm

```bash
aws cloudwatch put-metric-alarm \
  --alarm-name api-gateway-errors-smoking-cessation \
  --alarm-description "API Gateway 5xx errors" \
  --metric-name 5XXError \
  --namespace AWS/ApiGateway \
  --statistic Sum \
  --period 300 \
  --threshold 10 \
  --comparison-operator GreaterThanOrEqualToThreshold \
  --evaluation-periods 1 \
  --dimensions Name=ApiName,Value=LeafLungs-UserInfo-API \
  --alarm-actions arn:aws:sns:ap-southeast-1:140570829989:email-notifications \
  --region ap-southeast-1
```

### Bước 3: Create RDS Performance Alarm

```bash
aws cloudwatch put-metric-alarm \
  --alarm-name rds-cpu-smoking-cessation \
  --alarm-description "RDS high CPU usage" \
  --metric-name CPUUtilization \
  --namespace AWS/RDS \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanOrEqualToThreshold \
  --evaluation-periods 2 \
  --dimensions Name=DBInstanceIdentifier,Value=smoking-cessation-db \
  --alarm-actions arn:aws:sns:ap-southeast-1:140570829989:email-notifications \
  --region ap-southeast-1
```

### Bước 4: Setup SNS Notifications

```bash
# Create SNS topic
aws sns create-topic \
  --name email-notifications \
  --region ap-southeast-1

# Subscribe email
aws sns subscribe \
  --topic-arn arn:aws:sns:ap-southeast-1:140570829989:email-notifications \
  --protocol email \
  --notification-endpoint your-email@example.com \
  --region ap-southeast-1
```

Verify email subscription (check inbox)

---

## Phần 4: CloudTrail Audit Logging

### Bước 1: Enable CloudTrail

```bash
# Create S3 bucket for CloudTrail logs
aws s3api create-bucket \
  --bucket smoking-cessation-cloudtrail-logs-$(date +%s) \
  --region ap-southeast-1 \
  --create-bucket-configuration LocationConstraint=ap-southeast-1

# Create CloudTrail
aws cloudtrail create-trail \
  --name smoking-cessation-audit \
  --s3-bucket-name smoking-cessation-cloudtrail-logs-xxxxx \
  --is-multi-region-trail \
  --region ap-southeast-1

# Enable logging
aws cloudtrail start-logging \
  --trail-name smoking-cessation-audit \
  --region ap-southeast-1
```

### Bước 2: View CloudTrail Events

1. CloudTrail → Event history
2. Filter by:
   - Event name (e.g., PutFunction for Lambda updates)
   - Resource name
   - Time range
3. View event details

---

## Phần 5: X-Ray Distributed Tracing

### Bước 1: Enable X-Ray for Lambda

For each Lambda function:

```bash
aws lambda update-function-configuration \
  --function-name AdminManageCoachesFunction \
  --tracing-config Mode=Active \
  --region ap-southeast-1
```

### Bước 2: Enable X-Ray for API Gateway

1. API Gateway console
2. Logging → Settings
3. Check "X-Ray request tracing enabled"
4. Save

### Bước 3: View Service Map

1. X-Ray Console
2. Service map
3. View dependencies between services:
   - API Gateway → Lambda → RDS
   - API Gateway → Lambda → S3

### Bước 4: Analyze Traces

1. Click on service
2. View latency distribution
3. Identify slow operations
4. Drill down into error traces

![X-Ray Service Map](../assets/09-xray-service-map.png)

---

## Phần 6: Cost Monitoring

### Bước 1: Setup Cost Anomaly Detection

```bash
aws ce create-anomaly-monitor \
  --anomaly-monitor '{
    "MonitorName": "smoking-cessation-costs",
    "MonitorType": "DIMENSIONAL",
    "MonitorDimension": "SERVICE",
    "MonitorSpecification": "SERVICE"
  }' \
  --region us-east-1
```

### Bước 2: Create Budget

1. Billing Console → Budgets
2. Click "Create budget"
3. Budget type: Cost budget
4. Budget amount: $500/month
5. Alert threshold: 80%

### Bước 3: Cost Explorer

1. Cost Explorer
2. View costs by:
   - Service (Lambda, RDS, S3, etc.)
   - Time period
   - Usage type

Optimize top cost drivers

---

## Phần 7: Health Checks & Synthetics

### Bước 1: Create CloudWatch Synthetic Canary

```bash
aws synthetics create-canary \
  --name smoking-cessation-api-health \
  --artifact-s3-location s3://smoking-cessation-artifacts/canaries/ \
  --execution-role-arn arn:aws:iam::140570829989:role/canary-role \
  --code 'Handler=index.handler, Script="..." \
  --schedule-expression 'rate(5 minutes)' \
  --region ap-southeast-1
```

This creates synthetic transactions to test API availability

### Bước 2: Monitor Canary Results

1. Synthetics → Canaries
2. Click "smoking-cessation-api-health"
3. View success rates & latency
4. Setup alarm if failures detected

---

## Phần 8: Incident Management

### Bước 1: Setup Incident Response

When alarm triggers:

1. Notification sent via SNS
2. Team investigates via CloudWatch Logs
3. Check CloudTrail for API changes
4. Use X-Ray to debug performance issues
5. Escalate if needed

### Bước 2: Create Runbooks

Document incident response procedures:

Example runbook for "High Lambda Errors":

```
1. Check CloudWatch alarm details
2. View Lambda logs: /aws/lambda/AdminManageCoachesFunction
3. Search for error keywords
4. Check recent code deployments via CloudTrail
5. If database issue: Check RDS performance metrics
6. If rate limit: Check concurrent execution limits
7. Rollback code or scale resources
8. Verify error rate decreases
9. Post-incident review
```

---

## Checklist

- [ ] CloudWatch Log groups created for all services
- [ ] Log retention policies set (30 days)
- [ ] Dashboard created with key metrics
- [ ] Alarms configured:
  - Lambda errors
  - API Gateway errors
  - RDS performance
  - Billing/costs
- [ ] SNS topic & email notifications working
- [ ] CloudTrail enabled & logging
- [ ] X-Ray enabled for Lambda & API Gateway
- [ ] Cost anomaly detection active
- [ ] Budget alerts configured
- [ ] Health checks/Synthetics running
- [ ] Incident runbooks documented
- [ ] Ready for Module 10 (Cleanup)

---

## Notes

- CloudWatch retention: Default 0 (never expire) → set explicit values
- Metrics granularity: 1 minute (standard) or 10 seconds (high-res, higher cost)
- Alarms evaluate metrics every period (usually 5 minutes)
- X-Ray samples traces (sample rate configurable)
- Cost Explorer can identify cost optimization opportunities

---

## Kết Quả Đạt Được

Sau Module 9:

1. Comprehensive logging system setup
2. Real-time metrics dashboards
3. Automated alerts & notifications
4. Audit trail via CloudTrail
5. Distributed tracing via X-Ray
6. Cost monitoring & budget alerts
7. Health checks automated
8. Incident response processes documented
9. Ready for Cleanup & Optimization (Module 10)
