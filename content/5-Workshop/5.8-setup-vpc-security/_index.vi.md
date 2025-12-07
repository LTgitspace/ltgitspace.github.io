# Module 8: Verify VPC & Security Configuration

## Mục tiêu Module

- Verify VPC setup
- Review Security Groups configuration
- Verify Network ACLs
- Configure NLB for WebSocket
- Setup VPN/Bastion (optional)
- Review IAM policies

---

## Phần 1: Verify VPC

### Status Hiện Tại

VPC:
- ID: vpc-046dc916dde2fb93f
- Name: project-bundau-milo
- CIDR: 172.0.0.0/18
- Region: ap-southeast-1

### Bước 1: Truy cập VPC Console

1. VPC service
2. Click "VPCs"
3. Click vào "project-bundau-milo"

![VPC Dashboard](../assets/08-vpc-dashboard.png)

### Bước 2: Review Subnets

1. Click "Subnets" (left menu)
2. Verify:
   - Public subnets (for ALB/NLB)
   - Private subnets (for Lambda, RDS)
   - Availability Zones (2-3 for HA)

Expected structure:
```
Public Subnets (route to Internet Gateway)
├── ap-southeast-1a: 172.0.0.0/24
└── ap-southeast-1b: 172.0.1.0/24

Private Subnets (route via NAT Gateway)
├── ap-southeast-1a: 172.0.10.0/24
├── ap-southeast-1b: 172.0.11.0/24
└── ap-southeast-1c: 172.0.12.0/24
```

![Subnets](../assets/08-vpc-subnets.png)

### Bước 3: Review Internet Gateway

1. Click "Internet Gateways" (left menu)
2. Verify IGW attached to VPC
3. Should see route in public subnet:
   - Destination: 0.0.0.0/0
   - Target: Internet Gateway

### Bước 4: Review NAT Gateway (for private subnets)

1. Click "NAT Gateways" (left menu)
2. Verify NAT Gateway in public subnet
3. Check Elastic IP assigned
4. Verify route in private subnets:
   - Destination: 0.0.0.0/0
   - Target: NAT Gateway

---

## Phần 2: Verify Security Groups

### Security Groups Hiện Tại

| Group ID | Name | VPC | Purpose |
|----------|------|-----|---------|
| sg-0cf34158cb7f5440b | leaflungs-backend-sg | project-bundau-milo | EC2 Application servers + Lambda |
| sg-0adb5d7ea1fe0f6bb | leaflungs-nlb-sg | project-bundau-milo | NLB (WebSocket) |
| sg-027ef04aa3c769ecf | leaflungs-db-sg | project-bundau-milo | EC2 Database servers (PostgreSQL + MongoDB) |

### Bước 1: Review leaflungs-backend-sg

1. VPC → Security Groups
2. Click "leaflungs-backend-sg"

Inbound rules (should have):
```
HTTPS (443) from leaflungs-nlb-sg
MySQL/PostgreSQL (3306/5432) to leaflungs-db-sg
```

Outbound rules:
```
All traffic (0.0.0.0/0)
```

### Bước 2: Review leaflungs-nlb-sg

Inbound rules (should have):
```
HTTPS (443) from 0.0.0.0/0 (allow all)
```

Outbound rules:
```
All traffic to leaflungs-backend-sg
```

### Bước 3: Review leaflungs-db-sg

Inbound rules (should have):
```
PostgreSQL/MySQL (5432/3306) from leaflungs-backend-sg
PostgreSQL/MySQL (5432/3306) from leaflungs-nlb-sg (if needed)
```

Outbound rules:
```
All traffic (default)
```

---

## Phần 3: Verify NLB Configuration

### NLB Status

Name: leaflungs-userinfo-nlb
Type: Network Load Balancer
Status: Active
Purpose: WebSocket endpoint for chat

### Bước 1: Verify NLB Details

1. EC2 → Load Balancers
2. Click "leaflungs-userinfo-nlb"

Check:
- Scheme: Internal hoặc Internet-facing
- IP address type: IPv4
- VPC: project-bundau-milo
- Subnets: 2-3 public subnets

![NLB Configuration](../assets/06-nlb-configuration.png)

### Bước 2: Verify Target Groups

1. Click "Target groups" (left menu)
2. Check targets:
   - Type: IP hoặc Instance
   - Port: 80 hoặc 443
   - Health check: Enabled

### Bước 3: Configure SSL/TLS (if HTTPS)

1. Listeners tab
2. For HTTPS (443):
   - Protocol: TLS
   - Port: 443
   - Certificate: ACM certificate
   - SSL policy: ELBSecurityPolicy-TLS-1-2-2017-01

![NLB Listeners](../assets/06-nlb-listeners.png)

---

## Phần 4: Network ACLs (Optional Advanced)

### Bước 1: Review NACLs

1. VPC → Network ACLs
2. Select NACL for each subnet
3. Check inbound/outbound rules

Default should allow all traffic (you can restrict if needed)

---

## Phần 5: IAM Policies Review

### Bước 1: Review Lambda Execution Roles

For each role, check:
- CloudWatch logs access
- RDS database access
- S3 access (if needed)
- VPC access (for RDS)
- Secrets Manager access

Example policy for Lambda with RDS:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:ap-southeast-1:140570829989:*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:CreateNetworkInterface",
        "ec2:DescribeNetworkInterfaces",
        "ec2:DeleteNetworkInterface"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "rds-db:connect",
      "Resource": "arn:aws:rds:ap-southeast-1:140570829989:db:smoking-cessation-db"
    },
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue"
      ],
      "Resource": "arn:aws:secretsmanager:ap-southeast-1:140570829989:secret:smoking-cessation/*"
    }
  ]
}
```

### Bước 2: Review Cognito IAM Roles

Verify Cognito has permission to invoke Lambda triggers

---

## Phần 6: Security Best Practices

### 1. Disable Public RDS Access

Verify:
1. RDS → DB instances
2. "smoking-cessation-db"
3. Publicly accessible: No

### 2. Enable VPC Flow Logs (Recommended)

To monitor network traffic:

```bash
aws ec2 create-flow-logs \
  --resource-type VPC \
  --resource-ids vpc-046dc916dde2fb93f \
  --traffic-type ALL \
  --log-destination-type cloud-watch-logs \
  --log-group-name vpc-flow-logs \
  --deliver-logs-permission-role-arn arn:aws:iam::140570829989:role/flowlogsRole \
  --region ap-southeast-1
```

### 3. Enable GuardDuty (Threat Detection)

```bash
aws guardduty create-detector \
  --enable \
  --region ap-southeast-1
```

### 4. Enable Security Hub (Optional)

1. Security Hub Console
2. Click "Enable Security Hub"
3. Select standards:
   - AWS Foundational Security Best Practices
   - CIS AWS Foundations Benchmark

---

## Phần 7: VPN Setup (Optional)

If need remote access to private resources:

### Bước 1: Create VPN Connection

1. VPC → Virtual Private Network (VPN)
2. Click "Create VPN connection"
3. Type: Site-to-Site VPN
4. Target Gateway: VPC
5. Customer Gateway: Your network gateway

### Bước 2: Download VPN Configuration

Download configuration file and configure your VPN client

---

## Checklist

- [ ] VPC "project-bundau-milo" verified
- [ ] Subnets configured (public & private)
- [ ] Internet Gateway attached
- [ ] NAT Gateway configured
- [ ] All 3 Security Groups reviewed & configured
- [ ] NLB verified & operational
- [ ] SSL/TLS certificate for NLB configured
- [ ] IAM roles have proper permissions
- [ ] RDS not publicly accessible
- [ ] VPC Flow Logs enabled (optional)
- [ ] GuardDuty enabled (optional)
- [ ] Security Hub enabled (optional)
- [ ] Ready for Module 9 (Monitoring & Logging)

---

## Notes

- VPC isolation critical for security
- Security Groups are stateful (return traffic allowed automatically)
- Network ACLs are stateless (need explicit return rules)
- NLB better than ALB for WebSocket (lower latency)
- IAM follows least-privilege principle

---

## Kết Quả Đạt Được

Sau Module 8:

1. VPC fully verified & optimized
2. Security Groups properly configured
3. NLB operational for WebSocket
4. IAM roles with proper permissions
5. Network traffic isolated
6. Advanced security features enabled
7. Ready for Monitoring & Logging (Module 9)
