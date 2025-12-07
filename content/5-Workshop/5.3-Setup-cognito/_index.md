# Module 3: Verify & Configure Cognito Authentication

## Module Objectives

- Verify existing Cognito User Pool
- Check App Client configuration
- Review User Groups & Roles
- Test authentication flow
- Update configuration if necessary

---

## Part 1: Verify Current Cognito User Pool

### Current Status

Cognito User Pool:
- Pool ID: us-east-1_dskUsnKt3
- Name: leaflungs-user-pool
- Region: us-east-1
- Created: 2025-11-24
- App Client ID: 4175kqc33olfjinhkll4jme379

### Step 1: Access Cognito Console

1. Login to AWS Console
2. Search for "Cognito"
3. Click on Cognito
4. Click "User pools"
5. Select "leaflungs-user-pool"

![Cognito Console](../assets/03-cognito-console.png)

### Step 2: Review User Pool Settings

1. From dashboard, click on "leaflungs-user-pool"
2. Review settings:
   - Sign-up experience
   - Security requirements (Password policy)
   - MFA configuration
   - Account recovery options

![User Pool Settings](../assets/03-cognito-user-pool-settings.png)

---

## Part 2: Verify App Client Configuration

### Step 1: Access App Clients

1. From User Pool "leaflungs-user-pool"
2. Click "App integration" (left menu)
3. Click "App clients and analytics"
4. Select "smoking-cessation-client" or existing client

![App Clients](../assets/03-cognito-app-clients.png)

### Step 2: Review App Client Settings

Verify the following settings:

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

### Step 3: Update Callback URLs (if needed)

If callback URLs are not correct:

1. Click "Edit" (App client settings)
2. Update callback URLs:
   ```
   http://localhost:5173
   http://localhost:3000
   https://smoking-cessation.com
   ```
3. Update sign-out URLs similarly
4. Click "Save"

![Edit Callback URLs](../assets/03-cognito-edit-callback-urls.png)

---

## Part 3: Setup User Groups

### Step 1: Create User Groups

1. From User Pool settings, click "User groups"
2. Create groups:
   - **users**: Regular members using the platform
   - **coaches**: Professional coaches providing guidance
   - **admins**: Platform administrators

### Step 2: Assign IAM Roles

Each group should have associated IAM roles defining permissions:
- Users: Access to personal data & features
- Coaches: Access to client data & coaching tools
- Admins: Full access to platform management

---

## Part 4: Test Authentication Flow

### Step 1: Test User Registration

1. Go to your application frontend
2. Click "Sign up"
3. Enter valid email and password
4. Verify email confirmation

### Step 2: Test Login

1. Click "Login"
2. Enter credentials
3. Verify successful authentication
4. Check if JWT token is returned

### Step 3: Test Token Refresh

Implement refresh token flow to extend session without re-login.

