# Module 3: Verify & Configure Cognito Authentication

## Mục tiêu Module

- Verify Cognito User Pool hiện tại
- Kiểm tra App Client configuration
- Review User Groups & Roles
- Test authentication flow
- Cập nhật cấu hình nếu cần thiết

---

## Phần 1: Verify Cognito User Pool Hiện Tại

### Status Hiện Tại

Cognito User Pool:
- Pool ID: us-east-1_dskUsnKt3
- Name: leaflungs-user-pool
- Region: us-east-1
- Created: 2025-11-24
- App Client ID: 4175kqc33olfjinhkll4jme379

### Bước 1: Truy cập Cognito Console

1. Login vào AWS Console
2. Tìm kiếm "Cognito"
3. Click vào Cognito
4. Click "User pools"
5. Chọn "leaflungs-user-pool"

![Cognito Console](../assets/03-cognito-console.png)

### Bước 2: Review User Pool Settings

1. Từ dashboard, click vào "leaflungs-user-pool"
2. Review các settings:
   - Sign-up experience
   - Security requirements (Password policy)
   - MFA configuration
   - Account recovery options

![User Pool Settings](../assets/03-cognito-user-pool-settings.png)

---

## Phần 2: Verify App Client Configuration

### Bước 1: Truy cập App Clients

1. Từ User Pool "leaflungs-user-pool"
2. Click "App integration" (left menu)
3. Click "App clients and analytics"
4. Chọn "smoking-cessation-client" hoặc existing client

![App Clients](../assets/03-cognito-app-clients.png)

### Bước 2: Review App Client Settings

Verify các settings sau:

1. Authentication flows:
   - ALLOW_USER_PASSWORD_AUTH - Should be enabled
   - ALLOW_REFRESH_TOKEN_AUTH - Should be enabled
   - ALLOW_CUSTOM_AUTH - Check if needed

2. Callback URLs:
   - http://localhost:5173
   - http://localhost:3000
   - https://yourdomain.com (production)

3. Sign-out URLs:
   - http://localhost:5173
   - http://localhost:3000
   - https://yourdomain.com

4. Allowed OAuth Scopes:
   - openid
   - email
   - profile
   - phone

![App Client Config](../assets/03-cognito-app-client-settings.png)

### Bước 3: Update Callback URLs (if needed)

Nếu callback URLs chưa chính xác:

1. Click "Edit" (App client settings)
2. Update callback URLs:
   ```
   http://localhost:5173
   http://localhost:3000
   https://smoking-cessation.com
   ```
3. Update sign-out URLs tương tự
4. Click "Save"

![Edit Callback URLs](../assets/03-cognito-edit-callback-urls.png)

---

## Phần 3: Review User Groups

### Bước 1: Truy cập User Groups

1. Từ "leaflungs-user-pool"
2. Click "User groups" (left menu)
3. Verify 3 groups tồn tại:
   - users (regular users/VIPs)
   - coaches (support staff)
   - admins (system administrators)

![User Groups](../assets/03-cognito-user-groups.png)

### Bước 2: Review Group Permissions

Cho mỗi group, review:
- Group name
- Description
- IAM role (if configured)
- Policies

### Bước 3: Create Groups (if missing)

Nếu groups chưa tồn tại:

1. Click "Create group"
2. Nhập:
   - Group name: "users"
   - Description: "Regular users and VIPs"
3. Click "Create group"

Repeat cho "coaches" và "admins"

---

## Phần 4: Verify User Management

### Bước 1: Check Existing Users

1. Từ "leaflungs-user-pool"
2. Click "Users" (left menu)
3. View danh sách users
4. Verify các test users tồn tại

![Users List](../assets/03-cognito-users-list.png)

### Bước 2: Create Test Users (if needed)

Tạo 3 test users nếu chưa có:

#### Test User (Regular User)

1. Click "Create user"
2. Username: "testuser@example.com"
3. Email: "testuser@example.com"
4. Temporary password: "TempPass123!"
5. Check "Mark email as verified"
6. Click "Create user"
7. Add user vào "users" group

![Create Test User](../assets/03-cognito-create-test-user.png)

#### Test Coach

1. Username: "testcoach@example.com"
2. Email: "testcoach@example.com"
3. Add to "coaches" group

#### Test Admin

1. Username: "testadmin@example.com"
2. Email: "testadmin@example.com"
3. Add to "admins" group

---

## Phần 5: Test Authentication Flow

### Bước 1: Test User Login

Sử dụng AWS CLI:

```bash
aws cognito-idp initiate-auth \
  --client-id 4175kqc33olfjinhkll4jme379 \
  --auth-flow USER_PASSWORD_AUTH \
  --auth-parameters USERNAME=testuser@example.com,PASSWORD=TempPass123! \
  --region us-east-1
```

Expected output:
```json
{
  "AuthenticationResult": {
    "AccessToken": "...",
    "IdToken": "...",
    "RefreshToken": "...",
    "ExpiresIn": 3600,
    "TokenType": "Bearer"
  }
}
```

**Note**: If user chưa change temporary password, sẽ nhận `NEW_PASSWORD_REQUIRED` response

### Bước 2: Change Temporary Password

```bash
aws cognito-idp admin-set-user-password \
  --user-pool-id us-east-1_dskUsnKt3 \
  --username testuser@example.com \
  --password "NewSecurePass123!" \
  --permanent \
  --region us-east-1
```

### Bước 3: Retry Login

```bash
aws cognito-idp initiate-auth \
  --client-id 4175kqc33olfjinhkll4jme379 \
  --auth-flow USER_PASSWORD_AUTH \
  --auth-parameters USERNAME=testuser@example.com,PASSWORD=NewSecurePass123! \
  --region us-east-1
```

### Bước 4: Verify Token

```bash
aws cognito-idp get-user \
  --access-token <ACCESS_TOKEN_FROM_STEP_3> \
  --region us-east-1
```

Should return user attributes & groups

---

## Phần 6: Configure Cognito Domain (Optional)

### Bước 1: Create Cognito Domain

Nếu chưa có domain:

1. Từ "leaflungs-user-pool"
2. Click "App integration" → "Domain name"
3. Click "Create domain"
4. Domain prefix: "smoking-cessation-dev"
5. Click "Check availability"
6. Click "Create domain"

![Domain Configuration](../assets/03-cognito-domain-setup.png)

### Bước 2: Verify Domain

Domain URL sẽ là:
```
https://smoking-cessation-dev.auth.us-east-1.amazoncognito.com
```

---

## Phần 7: Frontend Amplify Configuration

### Bước 1: Verify .env Variables

Check file `.env`:

```
VITE_COGNITO_USER_POOL_ID=us-east-1_dskUsnKt3
VITE_COGNITO_CLIENT_ID=4175kqc33olfjinhkll4jme379
VITE_COGNITO_REGION=us-east-1
```

All must be verified ✓

### Bước 2: Verify Amplify Config (src/config/amplify.config.js)

```javascript
import { Amplify } from 'aws-amplify';

const poolData = {
  UserPoolId: import.meta.env.VITE_COGNITO_USER_POOL_ID,
  ClientId: import.meta.env.VITE_COGNITO_CLIENT_ID,
};

Amplify.configure({
  Auth: {
    region: import.meta.env.VITE_COGNITO_REGION,
    userPoolId: poolData.UserPoolId,
    userPoolWebClientId: poolData.ClientId,
  }
});
```

### Bước 3: Test Frontend Login

1. Start dev server: `npm run dev`
2. Navigate to login page
3. Test login with testuser@example.com / NewSecurePass123!
4. Verify token được lưu vào localStorage
5. Check console logs cho authentication flow

![Login Success](../assets/03-frontend-login-success.png)

---

## Phần 8: Security Best Practices

### 1. Enable MFA

1. Từ User Pool settings
2. Click "Sign-in experience"
3. Multi-factor authentication: "Optional"
4. Check "Authenticator apps"
5. Save

### 2. Password Policy

Verify password requirements:
- Length: minimum 8 characters
- Uppercase: required
- Lowercase: required
- Numbers: required
- Special characters: required

### 3. Account Recovery

Enable recovery methods:
- Email
- Phone (if phone number collected)

### 4. CloudTrail Logging

Enable CloudTrail để audit Cognito operations:

```bash
aws cloudtrail create-trail \
  --name cognito-audit-trail \
  --s3-bucket-name your-audit-bucket \
  --is-multi-region-trail \
  --region us-east-1
```

---

## Phần 9: Troubleshooting

### Issue: "Client does not support custom auth flow"

Solution:
- Verify "ALLOW_USER_PASSWORD_AUTH" enabled ở App Client
- Verify "ALLOW_REFRESH_TOKEN_AUTH" enabled

### Issue: "Callback URL mismatch"

Solution:
- Verify callback URLs ở App Client settings
- Match frontend domain exactly
- Include protocol (http:// or https://)

### Issue: "Invalid client id"

Solution:
- Verify Client ID ở .env matches App Client
- Check region matches (us-east-1)

### Issue: "User does not exist in Cognito user pool"

Solution:
- Create user first via create-user command
- Or allow self-sign-up in User Pool settings

---

## Checklist

- [ ] Cognito User Pool "leaflungs-user-pool" verified
- [ ] App Client "smoking-cessation-client" verified
- [ ] 3 User Groups (users, coaches, admins) exist
- [ ] 3 Test users created (testuser, testcoach, testadmin)
- [ ] Authentication flow tested via AWS CLI
- [ ] Frontend .env variables updated
- [ ] Amplify config verified
- [ ] Frontend login tested
- [ ] MFA configured
- [ ] CloudTrail logging enabled
- [ ] Ready for Module 4 (Verify Lambda Functions)

---

## Notes

- Cognito handles authentication - Frontend just stores tokens
- Tokens need refresh every 1 hour (AccessToken)
- User groups used for RBAC in Lambda functions
- All sensitive data (passwords) encrypted in transit & at rest

---

## Kết Quả Đạt Được

Sau Module 3:

1. Cognito User Pool fully verified
2. Authentication flow working end-to-end
3. Test users created & tested
4. Frontend integrated with Cognito
5. Security measures implemented
6. Ready to verify Lambda functions (Module 4)
