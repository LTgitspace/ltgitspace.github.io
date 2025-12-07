# Module 7: Verify S3 & CloudFront Frontend Hosting

## Mục tiêu Module

- Verify S3 bucket configuration
- Verify CloudFront distribution
- Configure Origin Access Control (OAC)
- Setup HTTPS & SSL/TLS
- Test frontend access
- Setup cache invalidation

---

## Phần 1: Verify S3 Frontend Bucket

### Status Hiện Tại

S3 Bucket:
- Name: leaflungs-frontend-new
- Region: us-east-1
- Created: 2025-11-24

### Bước 1: Truy cập S3 Bucket

1. S3 Console
2. Click vào bucket "leaflungs-frontend-new"

![S3 Bucket](../assets/07-s3-bucket-contents.png)

### Bước 2: Verify Bucket Properties

1. Click "Properties" tab
2. Check:
   - Versioning: Enabled (for rollback)
   - Server access logging: Enabled
   - Block Public Access: All enabled (important!)

### Bước 3: Verify Bucket Policy

1. Click "Permissions" tab
2. Click "Bucket policy"
3. Check policy allows CloudFront access:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowCloudFrontAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity <OAI_ID>"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::leaflungs-frontend-new/*"
    }
  ]
}
```

### Bước 4: Enable Static Website Hosting

1. Properties tab
2. Scroll to "Static website hosting"
3. Click "Edit"
4. Enable static website hosting:
   - Index document: index.html
   - Error document: index.html (for SPA routing)
5. Save

![Static Website Config](../assets/07-s3-static-website-hosting.png)

---

## Phần 2: Verify CloudFront Distribution

### Status Hiện Tại

Distribution:
- ID: E1NREZDKTJH6Y9
- Domain: d2yo2hr161ib8h.cloudfront.net
- Status: Deployed

### Bước 1: Truy cập Distribution

1. CloudFront Console
2. Click vào distribution "E1NREZDKTJH6Y9"

![CloudFront Distribution](../assets/07-cloudfront-distribution.png)

### Bước 2: Review General Settings

1. General tab
2. Check:
   - Enabled: Yes
   - Domain name: d2yo2hr161ib8h.cloudfront.net
   - SSL/TLS certificate: AWS Certificate Manager (ACM)
   - Default root object: index.html

### Bước 3: Review Origin Access Control (OAC)

1. Origins tab
2. Click origin "leaflungs-frontend-new"
3. Verify:
   - Origin Access Control: Enabled
   - OAI ID: E7DE5EADPNE96

![OAC Configuration](../assets/07-cloudfront-oac-settings.png)

### Bước 4: Review Cache Behavior

1. Behaviors tab
2. Path Pattern: Default (*)
3. Check:
   - Viewer protocol: Redirect HTTP to HTTPS
   - Allowed methods: GET, HEAD, OPTIONS
   - Caching: Enabled
   - TTL: 86400 seconds (1 day) for HTML

---

## Phần 3: Setup Custom Domain (Optional)

### Bước 1: Register Domain

If not already registered:
- Route 53 hoặc external registrar (GoDaddy, etc.)
- Domain: yourdomain.com

### Bước 2: Request ACM Certificate

1. ACM Console (us-east-1 only for CloudFront!)
2. Click "Request certificate"
3. Domain names:
   - yourdomain.com
   - *.yourdomain.com
4. Validation: DNS
5. Create certificate

![ACM Certificate](../assets/09-acm-certificate-request.png)

### Bước 3: Add Custom Domain to CloudFront

1. CloudFront distribution
2. General tab → Edit
3. Alternate domain names (CNAME): yourdomain.com, www.yourdomain.com
4. Custom SSL certificate: Select ACM certificate
5. Save

### Bước 4: Update DNS

1. Route 53 hoặc DNS provider
2. Create CNAME record:
   - Name: yourdomain.com
   - Value: d2yo2hr161ib8h.cloudfront.net
3. Propagate (up to 24 hours)

---

## Phần 4: Build & Deploy Frontend

### Bước 1: Build React App

```bash
npm run build
```

Output:
- dist/ folder with optimized HTML, CSS, JS

### Bước 2: Upload to S3

Option A: AWS Console

1. S3 bucket
2. Click "Upload"
3. Select dist/ folder
4. Click "Upload"

Option B: AWS CLI

```bash
aws s3 sync dist/ s3://leaflungs-frontend-new --delete
```

### Bước 3: Verify Upload

1. S3 bucket
2. Check files uploaded:
   - index.html
   - assets/ folder
   - *.js, *.css files

![Upload Complete](../assets/07-s3-upload-complete.png)

---

## Phần 5: Test Frontend Access

### Bước 1: Test CloudFront URL

```bash
curl https://d2yo2hr161ib8h.cloudfront.net
```

Should return index.html content

### Bước 2: Test Browser Access

1. Open browser
2. Navigate to https://d2yo2hr161ib8h.cloudfront.net
3. Check:
   - Page loads
   - Assets loaded
   - No 404 errors
   - HTTPS secure

![Frontend Loaded](../assets/07-browser-smoking-cessation-app.png)

### Bước 3: Test Custom Domain (if configured)

Navigate to https://yourdomain.com
Should load frontend

---

## Phần 6: Cache Invalidation

### Issue: After Deploy, Old Content Shown

Solution: Invalidate CloudFront cache

### Bước 1: Invalidate via Console

1. CloudFront distribution
2. "Invalidations" tab
3. Click "Create invalidation"
4. Object paths: /* (invalidate everything)
5. Click "Invalidate"

Wait 2-3 minutes for propagation

### Bước 2: Automatic Invalidation (Recommended)

Setup script to auto-invalidate:

```bash
#!/bin/bash

# Build
npm run build

# Upload to S3
aws s3 sync dist/ s3://leaflungs-frontend-new --delete

# Invalidate CloudFront
aws cloudfront create-invalidation \
  --distribution-id E1NREZDKTJH6Y9 \
  --paths "/*" \
  --region us-east-1

echo "Deployment complete!"
```

---

## Phần 7: Security Headers

### Bước 1: Add Security Headers via CloudFront

1. Behaviors tab
2. Cache policy
3. Custom headers:
   - X-Frame-Options: DENY (prevent clickjacking)
   - X-Content-Type-Options: nosniff
   - X-XSS-Protection: 1; mode=block
   - Strict-Transport-Security: max-age=31536000; includeSubDomains
   - Content-Security-Policy: default-src 'self'

### Bước 2: Configure CORS Headers

Update S3 CORS:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET"],
    "AllowedOrigins": ["https://yourdomain.com"],
    "ExposeHeaders": ["ETag"],
    "MaxAgeSeconds": 3000
  }
]
```

---

## Phần 8: Monitoring & Analytics

### Bước 1: Enable CloudFront Metrics

1. CloudFront distribution
2. Monitoring tab
3. View metrics:
   - Requests
   - Data transferred
   - Cache hit ratio
   - HTTP status codes

### Bước 2: Setup CloudWatch Alarms

```bash
aws cloudwatch put-metric-alarm \
  --alarm-name cloudfront-errors \
  --alarm-description "CloudFront 4xx/5xx errors" \
  --metric-name 4xxErrorRate \
  --namespace AWS/CloudFront \
  --statistic Average \
  --period 300 \
  --threshold 5 \
  --comparison-operator GreaterThanThreshold
```

---

## Checklist

- [ ] S3 bucket "leaflungs-frontend-new" verified
- [ ] Static website hosting enabled
- [ ] CloudFront distribution verified
- [ ] Origin Access Control configured
- [ ] Custom domain configured (optional)
- [ ] SSL/TLS certificate valid
- [ ] Frontend built & uploaded to S3
- [ ] CloudFront URL accessible
- [ ] Custom domain accessible (if configured)
- [ ] Cache invalidation working
- [ ] Security headers configured
- [ ] Monitoring enabled
- [ ] Ready for Module 8 (Verify VPC & Security)

---

## Notes

- CloudFront caches content at edge locations
- Cache invalidation cost: $0.005 per path
- OAC prevents direct S3 access (more secure than OAI)
- SPA routing requires index.html as error document

---

## Kết Quả Đạt Được

Sau Module 7:

1. S3 bucket fully configured
2. CloudFront distribution optimized
3. Frontend deployed & accessible
4. HTTPS/SSL configured
5. Caching & invalidation working
6. Security headers configured
7. Ready for VPC & Security verification (Module 8)
