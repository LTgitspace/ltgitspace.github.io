---
title: "DNS Simulation & Route 53 Configuration"
date: "2025-01-01"
weight: 4
chapter: false
pre: " <b> 5.4.4 </b> "
---

## Overview

In this section, you will configure Route 53 to simulate DNS resolution for the S3 interface endpoint from the on-premises environment. This demonstrates how DNS forwarding works when accessing AWS services through private endpoints.

---

## Part 1: Create Private Hosted Zone

1. Route 53 Console
2. Click "Hosted zones"
3. Click "Create hosted zone"

Configuration:
- Domain name: `s3.us-east-1.amazonaws.com`
- Type: Private hosted zone
- VPC: Select "VPC Cloud"
- Tags: Environment=workshop

![Create Hosted Zone](/images/5-Workshop/5.4-S3-onprem/create-hosted-zone.png)

---

## Part 2: Create Alias Record

1. From the hosted zone, click "Create record"
2. Record configuration:
   - Name: `bucket` (to create `bucket.s3.us-east-1.amazonaws.com`)
   - Type: A
   - Alias: Yes
   - Alias target: Your S3 interface endpoint DNS name

![Create Record](/images/5-Workshop/5.4-S3-onprem/create-record.png)

3. Click "Create records"

---

## Part 3: Setup Route 53 Resolver

### Create Inbound Endpoint (Cloud VPC)

This endpoint receives DNS requests from on-premises environment.

1. Route 53 → Resolver
2. Click "Inbound endpoints"
3. Click "Create inbound endpoint"

Configuration:
- Name: `inbound-endpoint-cloud`
- VPC: VPC Cloud
- Security group: Allow UDP/TCP port 53 from on-premises CIDR
- IP addresses: Select 2 subnets in different AZs

![Inbound Endpoint](/images/5-Workshop/5.4-S3-onprem/inbound-endpoint.png)

### Create Outbound Endpoint (On-prem VPC)

This endpoint forwards DNS requests from on-premises to cloud.

1. Click "Outbound endpoints"
2. Click "Create outbound endpoint"

Configuration:
- Name: `outbound-endpoint-onprem`
- VPC: VPC On-prem
- Security group: Allow all outbound to inbound endpoint
- IP addresses: Select 2 subnets

![Outbound Endpoint](/images/5-Workshop/5.4-S3-onprem/outbound-endpoint.png)

---

## Part 4: Create Resolver Rules

### Create Forwarding Rule

1. Route 53 → Resolver → Rules
2. Click "Create rule"

Configuration:
- Name: `forward-s3-to-cloud`
- Type: Forward
- Domain name: `s3.us-east-1.amazonaws.com`
- Outbound endpoint: `outbound-endpoint-onprem`
- Target IP addresses: 
  - Inbound endpoint IP 1
  - Inbound endpoint IP 2

![Forwarding Rule](/images/5-Workshop/5.4-S3-onprem/forwarding-rule.png)

3. Associate rule with on-premises VPC

---

## Part 5: Test DNS Resolution

### From Cloud VPC (test-gateway-endpoint)

```bash
# SSH to instance or use Session Manager
nslookup bucket.s3.us-east-1.amazonaws.com

# Should return the private IP of the interface endpoint
# Example output:
# Name: bucket.s3.us-east-1.amazonaws.com
# Address: 172.0.5.123
```

### From On-prem VPC (test-interface-endpoint)

```bash
# Using Session Manager
nslookup bucket.s3.us-east-1.amazonaws.com

# Should resolve through Route 53 resolver
# and return the interface endpoint private IP
```

---

## Part 6: Verify End-to-End Connectivity

### Upload File Using Resolved DNS Name

```bash
# From on-premises environment
# Create test file
fallocate -l 100M testfile.xyz

# Upload using DNS name (instead of explicit endpoint URL)
aws s3 cp testfile.xyz s3://<your-bucket-name> \
  --region us-east-1
```

The system should:
1. Resolve `s3.us-east-1.amazonaws.com` through Route 53 DNS
2. Forward DNS request through Outbound Resolver
3. Receive response from Inbound Resolver in cloud
4. Connect to S3 interface endpoint via private IP
5. Upload file successfully

---

## Troubleshooting DNS Issues

### Issue: DNS resolution fails

Check:
- Security groups allow UDP/TCP port 53
- Resolver rules are properly associated with VPCs
- Forwarding targets are correct

```bash
# Debug DNS resolution
nslookup -type=A bucket.s3.us-east-1.amazonaws.com
nslookup -debug bucket.s3.us-east-1.amazonaws.com
```

### Issue: Connection fails after DNS resolution

Check:
- Network ACLs allow traffic on port 443 (HTTPS)
- Security groups on interface endpoint allow traffic
- Route tables have proper routes to interface endpoint

```bash
# Test connection to endpoint
curl -v https://bucket.s3.us-east-1.amazonaws.com/
```

---

## Summary

This section demonstrates:
- How to use Route 53 Private Hosted Zones for internal DNS
- How to setup Route 53 Resolver for DNS forwarding
- How DNS resolution flows from on-premises through AWS to resolve private endpoints
- Complete end-to-end connectivity for accessing S3 through PrivateLink from on-premises

This pattern can be extended to other AWS services that support PrivateLink endpoints.

