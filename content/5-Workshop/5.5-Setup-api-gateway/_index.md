# Module 5: Verify & Configure API Gateway

## Module Objectives

- Verify 2 existing API Gateways
- Check REST API resources & methods
- Verify Lambda integration
- Test API endpoints
- Setup CORS & rate limiting
- Update configuration if needed

---

## Part 1: Inventory API Gateways

### Current Status

2 REST APIs at ap-southeast-1:

| API Name | ID | Created | Purpose |
|----------|-----|---------|---------|
| LeafLungs-UserInfo-API | v7agf76rrh | 2025-11-24 | User management, Admin operations |
| leaflungs-chat-api | vuds39de1b | 2025-12-02 | Chat & WebSocket |

### Resources Created

**LeafLungs-UserInfo-API** (v7agf76rrh):
- /admin
- /admin/coaches
- /api/user-info
- /api/user-info/{id}
- /api/user-info/by-user-id
- /api/user-info/empty
- /upload

**leaflungs-chat-api** (vuds39de1b):
- Chat endpoints (to be verified)
- WebSocket endpoint

---

## Part 2: Verify LeafLungs-UserInfo-API

### Step 1: Access API Gateway

1. Login to AWS Console
2. Search for "API Gateway"
3. Click on service
4. Select region: ap-southeast-1
5. Click on "LeafLungs-UserInfo-API"

![API Gateway Home](../assets/05-api-gateway-dashboard.png)

### Step 2: Review API Resources

1. Click "Resources" (left menu)
2. View tree structure:
   ```
   /
   ├── /admin
   │   └── /admin/coaches
   ├── /api
   │   └── /api/user-info
   │       ├── /{id}
   │       ├── /by-user-id
   │       └── /empty
   └── /upload
   ```

![API Resources Tree](../assets/05-api-gateway-resources.png)

### Step 3: Verify /admin/coaches Resource

1. Click on "/admin/coaches"
2. Verify methods:
   - GET: List coaches
   - POST: Create coach
   - PUT: Update coach
   - DELETE: Delete coach

3. For each method, check:
   - Integration type: Lambda Function
   - Lambda Function: AdminManageCoachesFunction
   - Timeout: 30 seconds
   - Proxy integration: Checked

![Resource Method](../assets/05-api-gateway-method-config.png)

### Step 4: Verify Method Request

Click on "Method Request" (for GET /admin/coaches):

Check:
- Authorization: AWS_IAM or COGNITO_USER_POOLS
- API Key Required: false
- Request Validator: Validate body, parameters

![Method Request](../assets/05-api-gateway-method-request.png)

### Step 5: Verify Integration Response

Click on "Integration Response":

Check:
- Status Code: 200
- Response Models: application/json mapped correctly
- Response Templates: Transform responses if needed

---

## Part 3: Configure CORS (Cross-Origin Resource Sharing)

### Step 1: Enable CORS for All Resources

1. Click on root "/" resource
2. Right-click and select "Enable CORS"
3. For each resource, configure:
   - Access-Control-Allow-Headers: Content-Type, Authorization
   - Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
   - Access-Control-Allow-Origin: * (or specific domain)

### Step 2: Deploy API

1. Click "Deploy" button
2. Select deployment stage: "prod" or "dev"
3. Review and confirm

---

## Part 4: Test API Endpoints

### Using AWS CLI

```bash
# Test /admin/coaches GET
aws apigateway test-invoke-method \
  --rest-api-id v7agf76rrh \
  --resource-id /admin/coaches \
  --http-method GET \
  --region ap-southeast-1
```

### Using Postman or curl

```bash
# Test with curl
curl -X GET https://v7agf76rrh.execute-api.ap-southeast-1.amazonaws.com/prod/admin/coaches \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

---

## Part 5: Setup Rate Limiting

### Step 1: Create Usage Plan

1. Click "Usage Plans" (left menu)
2. Click "Create Usage Plan"
3. Set:
   - Name: "smoking-cessation-plan"
   - Description: "Rate limiting for Smoking Cessation Platform"
   - Throttle settings:
     - Request rate limit: 10,000 requests/second
     - Burst limit: 5,000 requests

### Step 2: Create API Key

1. Click "API Keys" (left menu)
2. Click "Create API Key"
3. Assign to Usage Plan
4. Share API key with clients (securely)

---

## Part 6: Monitor API Performance

1. Click "Stages"
2. Select deployment stage
3. Enable CloudWatch Logs
4. View metrics:
   - Count (total requests)
   - 4XXError (client errors)
   - 5XXError (server errors)
   - Latency (response time)
# Module 4: Verify & Update Lambda Functions

## Module Objectives

- Verify current Lambda functions
- Check IAM roles & permissions
- Review environment variables & configuration
- Check logs & troubleshoot errors
- Verify integration with EC2 servers
- Update functions if necessary

## Important: Lambda & EC2 Integration

Lambda functions work **complementary** with EC2 servers:
- EC2 servers (user-cessation, social-media): Handle continuous requests
- Lambda functions: Handle specific events (file upload, payments, webhooks)
- Integration via API Gateway: API Gateway routes requests to EC2 or Lambda

---

## Part 1: Inventory Current Lambda Functions

### Current Status

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

## Part 2: Verify Cognito Post-Confirmation Trigger

### Step 1: Access Lambda Function

1. Login to AWS Console
2. Search for "Lambda"
3. Click Lambda service
4. Select region: **us-east-1**
5. Click on function "CognitoPostConfirmationTrigger"

![Lambda Function Page](../assets/04-lambda-function-page.png)

### Step 2: Review Configuration

From "General configuration" section:

- Runtime: nodejs20.x (OK)
- Memory: 256 MB (Adequate)
- Timeout: 30 seconds (OK)
- Role: CognitoPostConfirmationLambdaRole (OK)

![Function Configuration](../assets/04-lambda-general-configuration.png)

### Step 3: Verify Cognito Integration

1. Click "Configuration" tab
2. Click "Triggers" (left menu)
3. Verify trigger:
   - Source: Cognito User Pool
   - User Pool: leaflungs-user-pool
   - Trigger: Post confirmation

![Cognito Trigger](../assets/04-lambda-cognito-trigger.png)

### Step 4: Review Function Code

1. Click "Code" tab
2. Review code content:
   - Check if it creates user profile
   - Check if it initializes coaching sessions
   - Verify error handling

Expected code should:
- Accept Cognito trigger event
- Create user profile in database
- Initialize coaching data
- Return response

---

## Part 3: Verify Other Lambda Functions

### AdminManageCoachesFunction

1. Switch region to ap-southeast-1
2. Click function "AdminManageCoachesFunction"
3. Verify:
   - Integration: API Gateway (/admin/coaches)
   - Role has permissions for database operations
   - Memory adequate (512 MB is good)
   - Timeout sufficient (30s OK)

### ImageUploadFunction

1. Click "image-upload-lambda"
2. Verify:
   - Integration: API Gateway (/upload)
   - Role has S3 PutObject permissions
   - Memory adequate (256 MB minimum)
   - Timeout for file operations (10s may be tight for large files)

### Payment Function

1. Click "PaymentFunction"
2. Verify:
   - Environment variables configured (payment provider credentials)
   - Role has permissions for payment service
   - Error handling for failed transactions
   - Logging enabled

---

## Part 4: Monitor Lambda Performance

### Check CloudWatch Logs

1. For each function, click "Monitor" tab
2. View:
   - Invocations (number of calls)
   - Errors (failed executions)
   - Duration (execution time)
   - Throttles (if any)

### Check Cost

1. Check Lambda pricing (pay per invocation)
2. If invocations are low, costs are minimal
3. Monitor memory usage - adjust if needed

