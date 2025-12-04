# Module 7: Verify S3 & CloudFront Frontend Hosting

## Module Objectives

- Verify S3 bucket configuration
- Verify CloudFront distribution
- Configure Origin Access Control (OAC)
- Setup HTTPS & SSL/TLS
- Test frontend access
- Setup cache invalidation

---

## Part 1: Verify S3 Frontend Bucket

### Current Status

S3 Bucket:
- Name: leaflungs-frontend-new
- Region: us-east-1
- Created: 2025-11-24

### Step 1: Access S3 Bucket

1. S3 Console
2. Click on bucket "leaflungs-frontend-new"

![S3 Bucket](../assets/07-s3-bucket-contents.png)

### Step 2: Verify Bucket Properties

1. Click "Properties" tab
2. Check:
   - Versioning: Enabled (for rollback)
   - Server access logging: Enabled
   - Block Public Access: All enabled (important!)

### Step 3: Verify Bucket Policy

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

### Step 4: Enable Static Website Hosting

1. Properties tab
2. Scroll to "Static website hosting"
3. Click "Edit"
4. Enable static website hosting:
   - Index document: index.html
   - Error document: index.html (for SPA routing)
5. Save

![Static Website Config](../assets/07-s3-static-website-hosting.png)

---

## Part 2: Verify CloudFront Distribution

### Current Status

Distribution:
- ID: E1NREZDKTJH6Y9
- Domain: d2yo2hr161ib8h.cloudfront.net
- Status: Deployed

### Step 1: Access Distribution

1. CloudFront Console
2. Click on distribution "E1NREZDKTJH6Y9"

![CloudFront Distribution](../assets/07-cloudfront-distribution.png)

### Step 2: Review General Settings

1. General tab
2. Check:
   - Enabled: Yes
   - Domain name: d2yo2hr161ib8h.cloudfront.net
   - SSL/TLS certificate: AWS Certificate Manager (ACM)
   - Default root object: index.html

### Step 3: Review Origin Access Control (OAC)

1. Click "Origins" tab
2. Verify:
   - Origin Domain: leaflungs-frontend-new.s3.us-east-1.amazonaws.com
   - Origin Access Identity: Configured (restricts direct S3 access)
   - Protocol: HTTPS only

### Step 4: Configure Cache Behavior

1. Click "Behaviors" tab
2. Verify default behavior:
   - Path Pattern: Default (*)
   - Origin: leaflungs-frontend-new
   - Viewer Protocol Policy: Redirect HTTP to HTTPS
   - Cache Policy: Managed-CachingOptimized or custom

### Step 5: Setup Custom Domain (Optional)

1. Click "Settings"
2. For production, add custom domain:
   - Domain name: smoking-cessation.com
   - SSL/TLS certificate: Create or import in ACM

---

## Part 3: Setup Cache Invalidation

### Invalidate Cache After Updates

```bash
# Invalidate all files
aws cloudfront create-invalidation \
  --distribution-id E1NREZDKTJH6Y9 \
  --paths "/*" \
  --region us-east-1

# Invalidate specific files
aws cloudfront create-invalidation \
  --distribution-id E1NREZDKTJH6Y9 \
  --paths "/index.html" "/css/*" \
  --region us-east-1
```

### Monitor Invalidation Status

```bash
aws cloudfront list-invalidations \
  --distribution-id E1NREZDKTJH6Y9 \
  --region us-east-1
```

---

## Part 4: Test Frontend Access

### Test CloudFront Domain

```bash
curl -I https://d2yo2hr161ib8h.cloudfront.net/

# Should return 200 OK with cache headers
```

### Test Custom Domain (if configured)

```bash
curl -I https://smoking-cessation.com/
```

### Performance Monitoring

1. CloudFront Console → Monitoring
2. View metrics:
   - Requests (total requests)
   - Data transferred (bytes)
   - Error rate (4xx, 5xx)
   - Cache hit ratio (should be > 80%)

---

## Part 5: Security & Optimization

### Enable Security Headers

Add custom headers in CloudFront:

```
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

### Compression

Enable automatic compression:
- Enable "Compress objects automatically"
- Improves performance for text-based content (HTML, CSS, JSON)

### Geo-Restriction (Optional)

Restrict access by country:
1. Click "Restrictions" tab
2. Enable geo-restriction if needed
3. Whitelist or blacklist countries
---
title: "Module 6: Cleanup Resources"
date: "2025-01-01"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

## Cleanup Resources

Congratulations on completing this lab!

In this lab, you have learned about architecture patterns to access Amazon S3 without using the Public Internet.

+ By creating a Gateway endpoint, you have enabled direct communication between EC2 resources and Amazon S3, without going through the Internet Gateway.
+ By creating an Interface endpoint, you have extended S3 connectivity to resources running in your on-premises data center through AWS Site-to-Site VPN or Direct Connect.

---

## Cleanup Steps

1. Navigate to Hosted Zones on the left side of the Route 53 dashboard. Click on the name of the s3.us-east-1.amazonaws.com zone. Click Delete and confirm deletion by entering the keyword "delete".

![Hosted Zone](/images/5-Workshop/5.6-Cleanup/delete-zone.png)

2. Disassociate Route 53 Resolver Rule - myS3Rule from "VPC Onprem" and Delete it.

![VPC](/images/5-Workshop/5.6-Cleanup/vpc.png)

3. Open the CloudFormation console and delete the two CloudFormation stacks you created for this lab:
   + PLOnpremSetup
   + PLCloudSetup

![Delete Stack](/images/5-Workshop/5.6-Cleanup/delete-stack.png)

4. Delete the S3 buckets

   + Open the S3 console
   + Select the bucket we created for the lab, click and confirm it is empty. Click Delete and confirm deletion.

![Delete S3](/images/5-Workshop/5.6-Cleanup/delete-s3.png)

---

## Cost Summary

Resources you have cleaned up:
- 2 CloudFormation stacks
- Route 53 hosted zones
- S3 buckets and objects
- VPC endpoints (free, but removed)
- Security groups and network configurations

You should not incur further charges for these resources.

