---
title: "Week 12 Worklog"
date: "2025-11-23"
weight: 2
chapter: false
pre: " <b> 1.12. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 12 Objectives

- Conduct end-to-end testing of the complete infrastructure.
- Perform disaster recovery and failover testing.
- Document the entire deployment and finalize the project.

### Tasks carried out this week
| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| Mon | - Review all infrastructure components and dependencies <br> - Create comprehensive architecture documentation | 13/11/2025 | 16/11/2025 | — |
| Tue | - Perform end-to-end application workflow testing <br> - Test all user journeys from login to data operations | 13/11/2025 | 16/11/2025 | — |
| Wed | - Weekly Scrum/Meeting: Progress report and team synchronization | 13/11/2025 | 16/11/2025 | — |
| Thu | - Simulate RDS failover and verify application recovery <br> - Test ECS task termination and restart behavior | 13/11/2025 | 16/11/2025 | <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.Maintenance.html">RDS Failover</a> |
| Fri | - Document infrastructure costs and optimization recommendations <br> - Create runbooks for common operational tasks | 13/11/2025 | 16/11/2025 | — |
| Sat | - Final review and cleanup <br> - Present project completion and lessons learned | 13/11/2025 | 16/11/2025 | — |

### Week 12 Achievements

- Successfully completed end-to-end infrastructure deployment covering compute, networking, database, and CI/CD.
- Verified application resilience through multi-point failure testing.
- Documented all components including architecture diagrams, runbooks, and cost analysis.
- Confirmed auto-scaling policies respond correctly under load.
- Tested backup and recovery procedures for RDS and S3 data.
- Delivered a production-ready infrastructure with monitoring and alerting.

### Challenges and lessons learned

- Load testing revealed connection pooling issues at scale; optimized application settings.
- Failover testing exposed security group rules that needed updating for backup scenarios.
- Infrastructure-as-Code would have simplified deployment and documentation significantly.
- Regular cost reviews should be scheduled to optimize resource utilization.

### Overall Internship Outcomes

Over the 12-week internship, successfully built a complete cloud infrastructure from scratch:
- **Infrastructure**: VPC with ELB, multi-AZ RDS, DynamoDB, and S3 storage
- **Containerization**: Docker, ECR, and ECS Fargate for application deployment
- **CI/CD**: Automated pipeline from GitHub to production via CodePipeline and CodeBuild
- **Monitoring**: Comprehensive CloudWatch dashboards and alerting
- **DNS & Security**: Route53 domain management with HTTPS via ACM

Key learnings include infrastructure design patterns, cost optimization, security best practices, and operational excellence. The internship provided practical experience with enterprise-scale AWS deployments.
