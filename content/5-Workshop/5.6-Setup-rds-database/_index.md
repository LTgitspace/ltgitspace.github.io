---
title: "Module 6: Verify EC2 Servers & Databases"
date: "2025-01-01"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# Module 6: Verify EC2 Servers & Databases

## Module Objectives

- Verify 4 running EC2 instances
- Check database servers (PostgreSQL + MongoDB)
- Verify application servers (user-cessation + social-media)
- Test connectivity between servers
- Configure monitoring & backups
- Troubleshoot connection issues

---

## Part 1: Inventory EC2 Instances

### Current Status

4 EC2 instances running at ap-southeast-1 (t4g.small):

| Instance ID | Name | IP Address | Type | SG | Created |
|-------------|------|------------|------|----|---------|
| i-0d82a626b99a2fecd | DB-PG | 172.0.8.55 | t4g.small | leaflungs-db-sg | 2025-11-30 |
| i-0374ff6972fd306fe | DB-Mongo | 172.0.8.124 | t4g.small | leaflungs-db-sg | 2025-12-02 |
| i-01dd1a4b2b8b4a41f | user-cessation | 172.0.3.240 | t4g.small | leaflungs-backend-sg | 2025-11-30 |
| i-059fae7766eb52ae3 | social-media | 172.0.3.236 | t4g.small | leaflungs-backend-sg | 2025-12-02 |

---

## Part 2: Verify Database Servers

### 2.1: PostgreSQL Server (DB-PG)

#### Step 1: Access EC2 Dashboard

1. EC2 Console
2. Click "Instances"
3. Click instance "DB-PG" (i-0d82a626b99a2fecd)

![EC2 Instance Details](../assets/06-ec2-instance-details.png)

#### Step 2: Check Instance Status

Verify:
- State: running
- Status checks: passed (2/2)
- IP Address: 172.0.8.55 (private)
- Security Group: leaflungs-db-sg
- Instance Type: t4g.small
- CPU Credits: > 80 (healthy)

![Instance Status](../assets/06-ec2-instance-status.png)

#### Step 3: Test PostgreSQL Connectivity

Connect via SSH (if you have keypair):

```bash
ssh -i your-key.pem ec2-user@172.0.8.55

# Once logged in
psql -U postgres -d smokingcessation
\dt  # List tables
\q   # Quit
```

Or check logs:

```bash
# View PostgreSQL status
systemctl status postgresql

# Check logs
tail -f /var/log/postgresql/postgresql.log
```

#### Step 4: Monitor PostgreSQL

1. CloudWatch → EC2 Instances
2. Select DB-PG instance
3. View metrics:
   - CPU Utilization (should be < 20%)
   - Network In/Out
   - Disk operations

If CPU too high: May need to scale up instance type

### 2.2: MongoDB Server (DB-Mongo)

#### Step 1: Access Instance

1. EC2 Console
2. Click instance "DB-Mongo" (i-0374ff6972fd306fe)

#### Step 2: Verify Status

Same checks as PostgreSQL:
- State: running
- Status checks: 2/2 passed
- IP: 172.0.8.124
- SG: leaflungs-db-sg

#### Step 3: Test MongoDB Connectivity

```bash
ssh -i your-key.pem ec2-user@172.0.8.124

# Once logged in
mongo
# Check connection
db.adminCommand('ping')
```

---

## Part 3: Verify Application Servers

### 3.1: User-Cessation Service

#### Step 1: Access Instance

1. EC2 Console
2. Click instance "user-cessation" (i-01dd1a4b2b8b4a41f)

#### Step 2: Verify Service Status

```bash
ssh -i your-key.pem ec2-user@172.0.3.240

# Check if service is running
systemctl status user-cessation-service

# Check logs
journalctl -u user-cessation-service -f

# Check port 8000
netstat -tlnp | grep 8000
```

### 3.2: Social-Media Service

#### Step 1: Access Instance

1. EC2 Console
2. Click instance "social-media" (i-059fae7766eb52ae3)

#### Step 2: Verify Service Status

```bash
ssh -i your-key.pem ec2-user@172.0.3.236

# Check if service is running
systemctl status social-media-service

# Check logs
journalctl -u social-media-service -f

# Check port 8000
netstat -tlnp | grep 8000
```

---

## Part 4: Test Inter-Server Connectivity

### Test Database Connectivity from Application Servers

```bash
# From user-cessation EC2
ssh -i your-key.pem ec2-user@172.0.3.240

# Test PostgreSQL connection
psql -h 172.0.8.55 -U cessation_user -d smokingcessation -c "SELECT version();"

# Test MongoDB connection
mongo --host 172.0.8.124 --eval "db.adminCommand('ping')"
```

If connections fail, check:
1. Security Groups - ensure ingress rules allow connections
2. Network ACLs - allow traffic between subnets
3. Database credentials - correct username/password

---

## Part 5: Configure Monitoring

### Setup CloudWatch Alarms

```bash
# CPU High alarm
aws cloudwatch put-metric-alarm \
  --alarm-name db-pg-cpu-high \
  --alarm-description "Alert when DB-PG CPU exceeds 80%" \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --region ap-southeast-1
```

### Enable Enhanced Monitoring

1. For each EC2 instance, click "Monitoring" tab
2. Enable detailed monitoring (additional cost)
3. View metrics in CloudWatch

---

## Part 6: Configure Backups

### RDS Backup (if using RDS instead of EC2)

For production:
- Enable automated backups (7-35 days retention)
- Setup multi-AZ deployment for high availability
- Enable encryption at rest

### EBS Snapshots

Create snapshots of database volumes:

```bash
aws ec2 create-snapshot \
  --volume-id vol-xxxxx \
  --description "DB-PG backup $(date +%Y-%m-%d)" \
  --region ap-southeast-1
```

Schedule regular snapshots for disaster recovery.
