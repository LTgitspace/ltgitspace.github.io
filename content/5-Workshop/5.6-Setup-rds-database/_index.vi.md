# Module 6: Verify EC2 Servers & Databases

## Mục tiêu Module

- Verify 4 EC2 instances đang chạy
- Kiểm tra database servers (PostgreSQL + MongoDB)
- Verify application servers (user-cessation + social-media)
- Test connectivity giữa các servers
- Configure monitoring & backups
- Troubleshoot connection issues

---

## Phần 1: Inventory EC2 Instances

### Status Hiện Tại

4 EC2 instances running ở ap-southeast-1 (t4g.small):

| Instance ID | Name | IP Address | Type | SG | Created |
|-------------|------|------------|------|----|---------|
| i-0d82a626b99a2fecd | DB-PG | 172.0.8.55 | t4g.small | leaflungs-db-sg | 2025-11-30 |
| i-0374ff6972fd306fe | DB-Mongo | 172.0.8.124 | t4g.small | leaflungs-db-sg | 2025-12-02 |
| i-01dd1a4b2b8b4a41f | user-cessation | 172.0.3.240 | t4g.small | leaflungs-backend-sg | 2025-11-30 |
| i-059fae7766eb52ae3 | social-media | 172.0.3.236 | t4g.small | leaflungs-backend-sg | 2025-12-02 |

---

## Phần 2: Verify Database Servers

### 2.1: PostgreSQL Server (DB-PG)

#### Bước 1: Truy cập EC2 Dashboard

1. EC2 Console
2. Click "Instances"
3. Click instance "DB-PG" (i-0d82a626b99a2fecd)

![EC2 Instance Details](../assets/06-ec2-instance-details.png)

#### Bước 2: Check Instance Status

Verify:
- State: running
- Status checks: passed (2/2)
- IP Address: 172.0.8.55 (private)
- Security Group: leaflungs-db-sg
- Instance Type: t4g.small
- CPU Credits: > 80 (healthy)

![Instance Status](../assets/06-ec2-instance-status.png)

#### Bước 3: Test PostgreSQL Connectivity

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

#### Bước 4: Monitor PostgreSQL

1. CloudWatch → EC2 Instances
2. Select DB-PG instance
3. View metrics:
   - CPU Utilization (should be < 20%)
   - Network In/Out
   - Disk operations

If CPU too high: May need to scale up instance type

### 2.2: MongoDB Server (DB-Mongo)

#### Bước 1: Truy cập Instance

1. EC2 Console
2. Click instance "DB-Mongo" (i-0374ff6972fd306fe)

#### Bước 2: Verify Status

Same checks as PostgreSQL:
- State: running
- Status checks: 2/2 passed
- IP: 172.0.8.124
- SG: leaflungs-db-sg

#### Bước 3: Test MongoDB Connectivity

```bash
ssh -i your-key.pem ec2-user@172.0.8.124

# Once logged in
mongo --version
systemctl status mongod

# Check if listening on port 27017
netstat -tlnp | grep 27017
```

#### Bước 4: Monitor MongoDB

1. CloudWatch → EC2 Instances
2. Select DB-Mongo
3. View metrics (same as PostgreSQL)

---

## Phần 3: Verify Application Servers

### 3.1: user-cessation Server

#### Bước 1: Truy cập Instance

1. EC2 Console
2. Click instance "user-cessation" (i-01dd1a4b2b8b4a41f)

#### Bước 2: Verify Running

Check:
- State: running
- Status: 2/2 passed
- IP: 172.0.3.240
- SG: leaflungs-backend-sg
- Port: 8000 (application)

#### Bước 3: Test Application

Connect via SSH:

```bash
ssh -i your-key.pem ec2-user@172.0.3.240

# Check application status
systemctl status user-cessation  # or your service name

# Check if listening on port 8000
netstat -tlnp | grep 8000

# View logs
tail -f /var/log/user-cessation/app.log
```

Or test via API Gateway:

```bash
curl -H "Authorization: Bearer <TOKEN>" \
  https://v7agf76rrh.execute-api.ap-southeast-1.amazonaws.com/prod/api/user-info
```

#### Bước 4: Monitor Application

1. CloudWatch → EC2 Instances
2. Select user-cessation
3. View:
   - CPU Utilization
   - Network traffic
   - Disk usage
   - Memory (if CloudWatch agent installed)

### 3.2: social-media Server

Same steps as user-cessation:

1. Instance: i-059fae7766eb52ae3
2. IP: 172.0.3.236
3. Port: 8000
4. SG: leaflungs-backend-sg

---

## Phần 4: Test Inter-Server Connectivity

### Test PostgreSQL Access from App Server

From user-cessation instance:

```bash
# Test PostgreSQL connectivity
psql -h 172.0.8.55 -U postgres -d smokingcessation -c "SELECT 1"

# Should return: 1 (success)
```

### Test MongoDB Access from App Server

```bash
# Test MongoDB connectivity
mongo --host 172.0.8.124 --eval "db.adminCommand('ping')"

# Should return: { ok: 1 }
```

### Test EC2 to EC2 Communication

```bash
# From app server to database server
ping -c 3 172.0.8.55  # PostgreSQL
ping -c 3 172.0.8.124 # MongoDB

# Check response times (should be < 5ms in same VPC)
```

---

## Phần 5: Verify Security Groups

### Security Group Configuration

Verify rules allow communication:

#### leaflungs-backend-sg (Application servers)
Inbound rules:
- SSH (22): from admin IP or bastion
- HTTP (80): from NLB (optional, if using HTTP)
- HTTPS (443): from NLB (optional, if TLS at app level)
- Custom TCP 8000: from API Gateway / NLB

Outbound rules:
- All to leaflungs-db-sg (database access)

#### leaflungs-db-sg (Database servers)
Inbound rules:
- PostgreSQL (5432): from leaflungs-backend-sg
- MongoDB (27017): from leaflungs-backend-sg

Outbound rules:
- All traffic (default)

### Update Security Groups if Needed

1. VPC → Security Groups
2. Select "leaflungs-backend-sg"
3. Click "Edit inbound rules"
4. Verify or add:
   - Port 8000 from 0.0.0.0/0 (public)
   - Or from NLB security group only (more secure)

---

## Phần 6: Configure Monitoring & CloudWatch Agent

### 6.1: Install CloudWatch Agent (Optional but Recommended)

On each EC2 instance:

```bash
# Download agent
wget https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm

# Install
rpm -U ./amazon-cloudwatch-agent.rpm

# Configure
/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard

# Start agent
systemctl start amazon-cloudwatch-agent
systemctl enable amazon-cloudwatch-agent
```

### 6.2: Setup EC2 Detailed Monitoring

1. EC2 Console
2. Select each instance
3. Right-click → Monitor and troubleshoot
4. Click "Enable detailed monitoring"

This provides 1-minute metrics instead of 5-minute

### 6.3: Create Custom CloudWatch Alarms

For EC2 instances:

```bash
# High CPU alarm
aws cloudwatch put-metric-alarm \
  --alarm-name ec2-high-cpu-user-cessation \
  --alarm-description "CPU > 80%" \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --dimensions Name=InstanceId,Value=i-01dd1a4b2b8b4a41f \
  --alarm-actions arn:aws:sns:ap-southeast-1:140570829989:email-notifications \
  --region ap-southeast-1
```

---

## Phần 7: Database Backup Strategy

### 7.1: PostgreSQL Backups

On DB-PG instance:

```bash
# Create backup
pg_dump -U postgres smokingcessation > backup_$(date +%Y%m%d).sql

# Or automated daily backup
crontab -e
# Add: 0 3 * * * /usr/bin/pg_dump -U postgres smokingcessation > /backups/backup_$(date +\%Y\%m\%d).sql
```

### 7.2: MongoDB Backups

On DB-Mongo instance:

```bash
# Create backup
mongodump --db smokingcessation --out /backups/backup_$(date +%Y%m%d)

# Or automated backup
crontab -e
# Add: 0 4 * * * /usr/bin/mongodump --db smokingcessation --out /backups/backup_$(date +\%Y\%m\%d)
```

### 7.3: Upload Backups to S3

```bash
# Upload to S3 (run from EC2)
aws s3 sync /backups s3://smoking-cessation-backups/databases/

# Set S3 lifecycle policy to archive after 30 days
aws s3api put-bucket-lifecycle-configuration \
  --bucket smoking-cessation-backups \
  --lifecycle-configuration file://lifecycle.json
```

---

## Phần 8: Troubleshooting

### Issue: Cannot Connect to PostgreSQL

```bash
# Check if PostgreSQL is listening
netstat -tlnp | grep 5432

# Check PostgreSQL logs
tail -100 /var/log/postgresql/postgresql.log

# Check security group allows port 5432
```

### Issue: High CPU on EC2 Instance

```bash
# Check top processes
top -b -n 1 | head -20

# Check what's consuming CPU
ps aux | sort -rnk 3 | head

# Possible solutions:
# - Optimize application code
# - Scale up instance type (t4g.medium)
# - Add more instances & use load balancing
```

### Issue: Database Disk Full

```bash
# Check disk usage
df -h /var

# If full:
# - Archive old data
# - Delete old logs
# - Expand EBS volume
```

### Issue: Slow Database Queries

PostgreSQL:

```bash
# Enable slow query log
nano /etc/postgresql/13/main/postgresql.conf

# Find & uncomment:
# log_min_duration_statement = 1000  # Log queries > 1 second

# Restart PostgreSQL
systemctl restart postgresql
```

MongoDB:

```bash
# Enable profiling
mongo --eval "db.setProfilingLevel(1, { slowms: 1000 })"

# Query slow logs
mongo --eval "db.system.profile.find({ millis: { \$gt: 1000 } }).pretty()"
```

---

## Phần 9: Cost Optimization

### Current Costs (4 × t4g.small)

- EC2: ~$30/month (4 instances × $0.0252/hour × 730 hours)
- Data transfer: ~$10/month (between instances & to internet)
- **Total: ~$40/month**

### Optimization Strategies

1. **Consolidate databases** (if usage low)
   - Run both PostgreSQL & MongoDB on same instance
   - Saves ~$8/month

2. **Use Reserved Instances**
   - 1-year: 30% discount
   - Saves ~$12/month

3. **Right-size instances** (if usage < 20% CPU average)
   - Downgrade to t4g.micro
   - Saves ~$20/month

---

## Checklist

- [ ] All 4 EC2 instances verified running
- [ ] DB-PG PostgreSQL verified accessible
- [ ] DB-Mongo MongoDB verified accessible
- [ ] user-cessation application verified running
- [ ] social-media application verified running
- [ ] Inter-server connectivity tested
- [ ] Security groups properly configured
- [ ] CloudWatch monitoring enabled
- [ ] Database backups configured
- [ ] Disaster recovery plan documented
- [ ] Cost optimization reviewed
- [ ] Ready for Module 7 (Verify S3 & CloudFront)

---

## Notes

- All instances in private subnets (no public IP)
- Access via Session Manager or Bastion host
- Auto-scaling not configured (static 4 instances)
- No snapshots currently (add for disaster recovery)
- Database replication not configured (single instance per DB)

---

## Kết Quả Đạt Được

Sau Module 6:

1. All 4 EC2 instances verified & operational
2. Database servers (PostgreSQL + MongoDB) tested
3. Application servers (user-cessation + social-media) verified
4. Inter-server connectivity confirmed
5. Monitoring configured via CloudWatch
6. Backup strategy implemented
7. Security groups validated
8. Cost optimization opportunities identified
9. Ready for S3 & CloudFront verification (Module 7)
