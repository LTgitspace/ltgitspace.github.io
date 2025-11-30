---
title: "Week 8 Worklog"
date: "2025-10-26"
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 8 Objectives

- Set up RDS for managed relational database services.
- Implement basic backup and recovery strategies.
- Configure security groups for database access from ECS services.

### Tasks carried out this week
| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| Mon | - Create an RDS database instance (PostgreSQL/MySQL) <br> - Configure multi-AZ deployment for high availability | 16/10/2025 | 19/10/2025 | <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/">RDS Documentation</a> |
| Tue | - Configure database security group to allow access from ECS <br> - Test database connectivity from EC2 bastion host | 16/10/2025 | 19/10/2025 | <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.html">Getting Started with RDS</a> |
| Wed | - Weekly Scrum/Meeting: Progress report and team synchronization | 16/10/2025 | 19/10/2025 | — |
| Thu | - Enable Enhanced Monitoring for RDS instance <br> - Configure database parameter groups for optimization | 16/10/2025 | 19/10/2025 | <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/MonitoringOverview.html">Monitoring RDS</a> |
| Fri | - Update application connection strings in ECS tasks <br> - Test database operations from containerized application | 16/10/2025 | 19/10/2025 | <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ConnectToInstance.html">Connecting to RDS</a> |
| Sat | - Perform backup and restore test <br> - Document database configuration and credentials management | 16/10/2025 | 19/10/2025 | — |

### Week 8 Achievements

- Successfully provisioned a multi-AZ RDS instance with automatic failover.
- Configured proper security group ingress rules for application access.
- Implemented automated daily backups with 7-day retention.
- Verified application connectivity to the RDS database.
- Set up Enhanced Monitoring to track database performance metrics.
- Learned RDS parameter group customization and performance tuning basics.

### Challenges and lessons learned

- Database parameter changes may require instance reboots; plan maintenance windows carefully.
- Multi-AZ deployments double the cost; evaluate needs before enabling.
- Application connection pooling is important to avoid exhausting available connections.
