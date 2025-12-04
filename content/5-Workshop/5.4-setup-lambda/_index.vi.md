# Module 4: Verify & Update Lambda Functions

## Mục tiêu Module

- Verify Lambda functions hiện tại
- Kiểm tra IAM roles & permissions
- Review environment variables & configuration
- Kiểm tra logs & troubleshoot errors
- Verify integration với EC2 servers
- Update functions nếu cần thiết

## Important: Lambda & EC2 Integration

Lambda functions hoạt động **complementary** với EC2 servers:
- EC2 servers (user-cessation, social-media): Xử lý requests liên tục
- Lambda functions: Xử lý specific events (file upload, payments, webhooks)
- Integration qua API Gateway: API Gateway routes requests tới EC2 hoặc Lambda

---

## Phần 1: Inventory Lambda Functions Hiện Tại

### Status Hiện Tại

**us-east-1 (1 function)**

| Function | Runtime | Role | Memory | Timeout | Last Modified |
|----------|---------|------|--------|---------|---|
| CognitoPostConfirmationTrigger | nodejs20.x | CognitoPostConfirmationLambdaRole | 256 MB | 30s | 2025-11-24 |

**ap-southeast-1 (4 functions)**

| Function | Runtime | Role | Memory | Timeout | Last Modified |
|----------|---------|------|--------|---------|---|
| AdminManageCoachesFunction | nodejs20.x | AdminOperationsLambdaRole | 512 MB | 30s | 2025-12-01 |
| PaymentFunction | nodejs24.x | ? | ? | ? | 2025-11-30 |
| leaflungs-websocket-authorizer | nodejs20.x | ? | ? | ? | 2025-12-02 |
| image-upload-lambda | nodejs20.x | ImageUploadLambdaRole | 256 MB | 10s | 2025-11-30 |

**Total: 5 Lambda functions**

### API Gateway Resources Integrated

```
/admin
/admin/coaches
/api/user-info
/api/user-info/{id}
/api/user-info/by-user-id
/api/user-info/empty
/upload
```

---

## Phần 2: Verify Cognito Post-Confirmation Trigger

### Bước 1: Truy cập Lambda Function

1. Login vào AWS Console
2. Tìm kiếm "Lambda"
3. Click Lambda service
4. Chọn region: **us-east-1**
5. Click vào function "CognitoPostConfirmationTrigger"

![Lambda Function Page](../assets/04-lambda-function-page.png)

### Bước 2: Review Configuration

Từ "General configuration" section:

- Runtime: nodejs20.x (OK)
- Memory: 256 MB (Adequate)
- Timeout: 30 seconds (OK)
- Role: CognitoPostConfirmationLambdaRole (OK)

![Function Configuration](../assets/04-lambda-general-configuration.png)

### Bước 3: Verify Cognito Integration

1. Click "Configuration" tab
2. Click "Triggers" (left menu)
3. Verify trigger:
   - Source: Cognito User Pool
   - User Pool: leaflungs-user-pool
   - Trigger: Post confirmation

![Cognito Trigger](../assets/04-lambda-cognito-trigger.png)

### Bước 4: Review Function Code

1. Click "Code" tab
2. Review code content:
   - Check if it creates user profile
   - Check if it initializes coaching sessions
   - Verify error handling

Example expected code structure:

```javascript
exports.handler = async (event) => {
  console.log('Cognito post-confirmation event:', event);

  try {
    const userId = event.request.userAttributes.sub;
    const email = event.request.userAttributes.email;

    // TODO: Create user profile in RDS
    // INSERT INTO users (cognito_id, email, ...) VALUES (...)

    return event;
  } catch (error) {
    console.error('Error:', error);
    throw error;
  }
};
```

### Bước 5: Check CloudWatch Logs

1. Click "Monitor" tab
2. Click "View logs in CloudWatch"
3. Select log stream (most recent)
4. Review logs cho:
   - Success: "Cognito post-confirmation event"
   - Errors: Check error messages
   - Database operations: Verify user creation

![CloudWatch Logs](../assets/09-cloudwatch-logs.png)

---

## Phần 3: Verify Admin Coaches Function

### Bước 1: Truy cập Function

1. Chọn region: **ap-southeast-1**
2. Click vào function "AdminManageCoachesFunction"

### Bước 2: Review Configuration

- Runtime: nodejs20.x (OK)
- Memory: 512 MB (Good for database operations)
- Timeout: 30 seconds (May need increase)
- Role: AdminOperationsLambdaRole (OK)

### Bước 3: Verify API Gateway Integration

1. Click "Configuration" tab
2. Click "Triggers" (left menu)
3. Verify:
   - API Gateway: LeafLungs-UserInfo-API
   - Resources: /admin/coaches
   - Methods: GET, POST, PUT, DELETE

![API Gateway Trigger](../assets/04-lambda-api-gateway-trigger.png)

### Bước 4: Review Code

Expected functionality:
- List coaches
- Create new coach
- Update coach info
- Delete coach
- RBAC check (admin only)

### Bước 5: Test Function

Via AWS Console:

1. Click "Test" tab
2. Create test event:

```json
{
  "httpMethod": "GET",
  "path": "/admin/coaches",
  "headers": {
    "Authorization": "Bearer <YOUR_ACCESS_TOKEN>"
  }
}
```

3. Click "Test"
4. Verify response:
   - Status: 200
   - Body: List of coaches

![Test Result](../assets/04-lambda-test-execution.png)

---

## Phần 4: Verify Image Upload Lambda

### Bước 1: Truy cập Function

1. Stay in ap-southeast-1 region
2. Click vào "image-upload-lambda"

### Bước 2: Review Configuration

- Runtime: nodejs20.x (OK)
- Memory: 256 MB (Adequate for image upload)
- Timeout: 10 seconds (May need increase to 30s)
- Role: ImageUploadLambdaRole (OK)

**Issue**: Timeout có thể quá ngắn cho file upload

### Bước 3: Update Timeout (if needed)

1. Click "Configuration" tab
2. Click "General configuration" → Edit
3. Change Timeout: 30 seconds (or 60s cho large files)
4. Click "Save"

![Edit Configuration](../assets/04-lambda-edit-timeout.png)

### Bước 4: Verify S3 Integration

1. Click "Configuration" tab
2. Click "Environment variables"
3. Verify variables:
   - S3_BUCKET: leaflungs-images hoặc leaflungs-images-sg
   - S3_REGION: ap-southeast-1

![Environment Variables](../assets/04-lambda-environment-variables.png)

### Bước 5: Test Image Upload

1. Click "Test" tab
2. Create test event:

```json
{
  "httpMethod": "POST",
  "path": "/upload",
  "headers": {
    "Content-Type": "multipart/form-data",
    "Authorization": "Bearer <YOUR_ACCESS_TOKEN>"
  },
  "body": "base64-encoded-image-data"
}
```

3. Click "Test"
4. Verify response:
   - Status: 200
   - Body: Contains S3 URL

---

## Phần 5: Verify WebSocket Authorizer Lambda

### Bước 1: Truy cập Function

1. Stay in ap-southeast-1
2. Click vào "leaflungs-websocket-authorizer"

### Bước 2: Review Configuration

- Runtime: nodejs20.x (OK)
- Purpose: Authorize WebSocket connections
- Should validate JWT tokens

### Bước 3: Check CloudWatch Logs

1. Click "Monitor" tab
2. View recent logs
3. Check for:
   - Token validation errors
   - Connection rejections
   - Authorization failures

### Bước 4: Verify Code

Expected functionality:
```javascript
exports.handler = async (event) => {
  const token = event.authorizationToken;

  try {
    // Validate JWT token
    const decoded = verifyToken(token);

    // Return auth policy
    return {
      principalId: decoded.sub,
      policyDocument: {
        Version: '2012-10-17',
        Statement: [
          {
            Action: 'execute-api:Invoke',
            Effect: 'Allow',
            Resource: event.methodArn
          }
        ]
      }
    };
  } catch (error) {
    throw new Error('Unauthorized');
  }
};
```

---

## Phần 6: Verify Payment Function

### Bước 1: Check Function Details

1. Click vào "PaymentFunction"
2. Note: Runtime is nodejs24.x (newer version)

### Bước 2: Find IAM Role

```bash
aws lambda get-function-configuration --function-name PaymentFunction --region ap-southeast-1 --output table --query 'Role'
```

This will show the IAM role ARN

### Bước 3: Verify Role Permissions

```bash
aws iam get-role-policy --role-name <RoleName> --policy-name <PolicyName>
```

Check if role has:
- DynamoDB permissions (if using DynamoDB for payment records)
- SNS permissions (if sending notifications)
- S3 permissions (if storing receipts)

---

## Phần 7: Verify IAM Roles & Permissions

### Bước 1: Check All Lambda Roles

Review each IAM role:

```bash
aws iam get-role --role-name CognitoPostConfirmationLambdaRole --output table --query 'Role.[RoleName,Arn,CreateDate]'
```

### Bước 2: List Role Policies

```bash
aws iam list-role-policies --role-name AdminOperationsLambdaRole --output table
```

### Bước 3: Review Policy Details

For each policy:

```bash
aws iam get-role-policy --role-name AdminOperationsLambdaRole --policy-name <PolicyName> --output json
```

Check permissions cover:
- DynamoDB/RDS operations
- S3 access (if needed)
- CloudWatch logs
- VPC access (if RDS in VPC)

---

## Phần 8: Monitor Lambda Functions

### Bước 1: View Metrics

For each function:

1. Click function
2. Click "Monitor" tab
3. View metrics:
   - Invocations: Total calls
   - Duration: Execution time
   - Errors: Failed invocations
   - Throttles: Rate limit hits
   - Concurrent Executions: Parallel runs

![Lambda Metrics](../assets/04-lambda-cloudwatch-metrics.png)

### Bước 2: Setup Alarms (Recommended)

Create alarms cho:
- Errors > 5 per minute
- Duration > 25 seconds
- Throttles > 0

Navigate to CloudWatch:

1. CloudWatch → Alarms → Create alarm
2. Select metric: Lambda Errors
3. Function: AdminManageCoachesFunction
4. Threshold: > 5 errors per minute
5. Action: SNS notification (email)

---

## Phần 9: Optimize Lambda Functions

### Recommendation 1: Increase Memory (if needed)

Functions that access database:
- AdminManageCoachesFunction: 512 MB (OK)
- image-upload-lambda: 512 MB (increase from 256)

More memory = More CPU = Faster execution = Lower cost

### Recommendation 2: Add Concurrency Limits

Reserve concurrency để prevent cost spikes:

```bash
aws lambda put-function-concurrency \
  --function-name AdminManageCoachesFunction \
  --reserved-concurrent-executions 10 \
  --region ap-southeast-1
```

### Recommendation 3: Enable X-Ray Tracing

For better debugging:

1. Function → Configuration
2. Click "General configuration"
3. Execution role: Edit
4. Add "AWSXRayWriteAccessPolicy"
5. Function → Configuration
6. Click "Monitoring tools"
7. Check "X-Ray"

---

## Phần 10: Environment Variables Review

### Bước 1: Check Variables ở Mỗi Function

For each Lambda function:

```bash
aws lambda get-function-configuration \
  --function-name <FunctionName> \
  --region ap-southeast-1 \
  --query 'Environment.Variables' \
  --output table
```

### Bước 2: Verify Required Variables

Common variables needed:
- RDS_ENDPOINT: Database host
- RDS_USERNAME: Database user
- RDS_PASSWORD: Database password (use Secrets Manager!)
- S3_BUCKET: S3 bucket name
- COGNITO_USER_POOL_ID: User pool ID
- JWT_SECRET: For token validation

### Bước 3: Use AWS Secrets Manager (Best Practice)

Instead of environment variables for secrets:

```bash
aws secretsmanager create-secret \
  --name smoking-cessation/db-password \
  --secret-string <PASSWORD> \
  --region ap-southeast-1
```

Update Lambda code:

```javascript
const secretsManager = new AWS.SecretsManager();

const secret = await secretsManager.getSecretValue({
  SecretId: 'smoking-cessation/db-password'
}).promise();

const dbPassword = JSON.parse(secret.SecretString).password;
```

---

## Checklist

- [ ] All 5 Lambda functions verified
- [ ] CognitoPostConfirmationTrigger trigger verified
- [ ] API Gateway integrations verified
- [ ] CloudWatch logs reviewed
- [ ] IAM roles & permissions checked
- [ ] Environment variables configured
- [ ] Functions tested successfully
- [ ] Metrics & alarms configured
- [ ] Secrets stored in Secrets Manager
- [ ] X-Ray tracing enabled (optional)
- [ ] Memory & timeout optimized
- [ ] Ready for Module 5 (Verify API Gateway)

---

## Troubleshooting

### Function Timeout

Issue: "Task timed out after 30 seconds"

Solution:
- Increase timeout: 60 seconds
- Check database connection (VPC issue?)
- Add connection pooling

### Permission Denied

Issue: "User is not authorized to perform: dynamodb:Query"

Solution:
- Add IAM policy to role
- Use Secrets Manager for credentials

### Cold Start

Issue: Long duration on first invocation

Solution:
- Increase memory (also increases CPU)
- Use Lambda provisioned concurrency
- Use Lambda warmup events

### Out of Memory

Issue: "Process exited before completing request"

Solution:
- Increase memory allocation
- Optimize code (leak memory?)
- Split into smaller functions

---

## Notes

- All functions using Node.js 20.x (current LTS)
- Timeout varies: 10s (image upload) to 30s (default)
- Roles have principle of least privilege
- CloudWatch logs retention: Check & set appropriately

---

## Kết Quả Đạt Được

Sau Module 4:

1. Tất cả 5 Lambda functions verified
2. Configuration & roles reviewed
3. API Gateway integration confirmed
4. CloudWatch monitoring setup
5. Performance optimized
6. Ready to verify API Gateway (Module 5)
