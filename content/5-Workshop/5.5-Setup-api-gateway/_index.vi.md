# Module 5: Verify & Configure API Gateway

## Mục tiêu Module

- Verify 2 API Gateways hiện tại
- Kiểm tra REST API resources & methods
- Verify Lambda integration
- Test API endpoints
- Setup CORS & rate limiting
- Cập nhật cấu hình nếu cần

---

## Phần 1: Inventory API Gateways

### Status Hiện Tại

2 REST APIs ở ap-southeast-1:

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

## Phần 2: Verify LeafLungs-UserInfo-API

### Bước 1: Truy cập API Gateway

1. Login vào AWS Console
2. Tìm kiếm "API Gateway"
3. Click vào service
4. Chọn region: ap-southeast-1
5. Click vào "LeafLungs-UserInfo-API"

![API Gateway Home](../assets/05-api-gateway-dashboard.png)

### Bước 2: Review API Resources

1. Click vào "Resources" (left menu)
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

### Bước 3: Verify /admin/coaches Resource

1. Click vào "/admin/coaches"
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

### Bước 4: Verify Method Request

Click vào "Method Request" (for GET /admin/coaches):

Check:
- Authorization: AWS_IAM hoặc COGNITO_USER_POOLS
- API Key Required: false
- Request Validator: Validate body, parameters

![Method Request](../assets/05-api-gateway-method-request.png)

### Bước 5: Verify Integration Response

Click vào "Integration Response":

Check:
- 200: Success response
- 400: Bad request
- 401: Unauthorized
- 500: Server error

![Integration Response](../assets/05-api-gateway-integration-response.png)

---

## Phần 3: Verify /upload Resource

### Bước 1: Review Upload Endpoint

1. Click vào "/upload" resource
2. Verify POST method
3. Check:
   - Lambda Integration: image-upload-lambda
   - Request Type: Binary (image data)
   - Response: S3 URL

### Bước 2: Test Upload

1. Click "POST" method
2. Click "Test" button
3. Send test request:

```json
{
  "httpMethod": "POST",
  "path": "/upload",
  "headers": {
    "Content-Type": "multipart/form-data",
    "Authorization": "Bearer <TOKEN>"
  }
}
```

Expected response:
```json
{
  "statusCode": 200,
  "body": {
    "url": "https://leaflungs-images.s3.amazonaws.com/..."
  }
}
```

---

## Phần 4: Verify Chat API (vuds39de1b)

### Bước 1: Select Chat API

1. Click vào "leaflungs-chat-api"
2. Review resources

### Bước 2: Check Chat Resources

Expected resources:
- /chat/rooms
- /chat/rooms/{roomId}
- /chat/rooms/{roomId}/messages
- /ws (WebSocket)

### Bước 3: Verify WebSocket Integration

1. Click vào "/ws" resource
2. Check:
   - Route Selection Expression: $request.body.action
   - Authorizer: leaflungs-websocket-authorizer

![WebSocket Config](../assets/05-api-gateway-websocket-config.png)

---

## Phần 5: CORS Configuration

### Bước 1: Enable CORS

For each API, verify CORS is enabled:

1. Click vào resource (e.g., /admin/coaches)
2. Click "Enable CORS"
3. Headers:
   - Access-Control-Allow-Headers: Content-Type, Authorization
   - Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
   - Access-Control-Allow-Origin: * (for dev) or specific domain (for prod)

![CORS Settings](../assets/05-api-gateway-cors-settings.png)

### Bước 2: Deploy API

After CORS changes:

1. Click "Deploy API"
2. Deployment stage: prod (or dev)
3. Wait for deployment

---

## Phần 6: API Keys & Usage Plans

### Bước 1: Create API Key (Optional)

For rate limiting per client:

1. Left menu → "API Keys"
2. Click "Create"
3. Name: "mobile-app" hoặc "web-client"
4. Save API Key

### Bước 2: Create Usage Plan (Optional)

For rate limiting:

1. Left menu → "Usage Plans"
2. Click "Create"
3. Name: "basic-plan"
4. Throttling: 100 requests/second
5. Quota: 1,000,000 requests/month

---

## Phần 7: Test API Endpoints

### Test with AWS CLI

#### Test GET /api/user-info

```bash
curl https://v7agf76rrh.execute-api.ap-southeast-1.amazonaws.com/prod/api/user-info \
  -H "Authorization: Bearer <YOUR_ACCESS_TOKEN>" \
  -H "Content-Type: application/json"
```

Expected: 200 OK with user list

#### Test POST /admin/coaches

```bash
curl -X POST \
  https://v7agf76rrh.execute-api.ap-southeast-1.amazonaws.com/prod/admin/coaches \
  -H "Authorization: Bearer <YOUR_ACCESS_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Coach Name",
    "email": "coach@example.com",
    "specialization": "Smoking Cessation"
  }'
```

Expected: 201 Created with coach ID

#### Test File Upload

```bash
curl -X POST \
  https://v7agf76rrh.execute-api.ap-southeast-1.amazonaws.com/prod/upload \
  -H "Authorization: Bearer <YOUR_ACCESS_TOKEN>" \
  -F "file=@/path/to/image.jpg"
```

Expected: 200 OK with S3 URL

### Test WebSocket

```bash
# Connect to WebSocket
wscat -c wss://vuds39de1b.execute-api.ap-southeast-1.amazonaws.com/prod \
  --header "Authorization: <TOKEN>"

# Send message
{"action": "sendMessage", "content": "Hello"}

# Expected response
{"message": "Message sent successfully"}
```

---

## Phần 8: Monitoring & Logging

### Bước 1: Enable CloudWatch Logs

1. For each API, click "Logging"
2. Check "Log full requests/responses"
3. CloudWatch log group: API-Gateway-Logs
4. Log retention: 30 days

### Bước 2: View Logs

1. CloudWatch → Logs → Log groups
2. Open "API-Gateway-Logs"
3. View recent requests

---

## Phần 9: Setup CloudFront Distribution (if needed)

### Bước 1: Create CloudFront Distrbution

To cache API responses (optional, for performance):

1. CloudFront Console
2. Click "Create distribution"
3. Origin domain: API Gateway domain
4. Viewer protocol: HTTPS only
5. Caching behavior: Cache GET requests

---

## Phần 10: Troubleshooting

### Issue: CORS errors in browser console

Solution:
- Enable CORS for resource
- Verify Access-Control headers
- Check allowed origin

### Issue: 403 Unauthorized

Solution:
- Verify JWT token valid
- Check authorization type (COGNITO_USER_POOLS)
- Verify user in correct group

### Issue: 500 Internal Server Error

Solution:
- Check Lambda logs via CloudWatch
- Verify Lambda role has proper permissions
- Check function timeout

### Issue: Lambda timeout

Solution:
- Increase API Gateway timeout: 30 seconds
- Increase Lambda timeout: 60 seconds
- Optimize Lambda code

---

## Checklist

- [ ] LeafLungs-UserInfo-API verified (v7agf76rrh)
- [ ] leaflungs-chat-api verified (vuds39de1b)
- [ ] All resources & methods verified
- [ ] CORS enabled
- [ ] CloudWatch logging enabled
- [ ] API endpoints tested via curl
- [ ] WebSocket tested
- [ ] API Keys created (optional)
- [ ] Usage Plans created (optional)
- [ ] Ready for Module 6 (Setup RDS)

---

## Notes

- API Gateway auto-scales
- Per-request pricing (pay for actual usage)
- CloudFront caching reduces latency
- CORS must match frontend domain

---

## Kết Quả Đạt Được

Sau Module 5:

1. 2 API Gateways fully verified
2. All resources & methods reviewed
3. Integration with Lambda confirmed
4. CORS configured
5. Monitoring enabled
6. Ready to setup RDS (Module 6)
