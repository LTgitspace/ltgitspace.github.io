---
title: "Week 4 Worklog"
date: "2025-09-28"
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 4 Objectives

- Deep dive into containerization with Docker and ECS fundamentals.
- Set up a basic VPC with proper networking configuration.
- Explore Elastic Load Balancer (ELB) for traffic distribution.

### Tasks carried out this week
| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| Mon | - Review Docker concepts and container images <br> - Learn ECS task definitions and clusters | 18/09/2025 | 21/09/2025 | <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/">ECS Documentation</a> |
| Tue | - Create a VPC with public and private subnets <br> - Configure security groups and NACLs | 18/09/2025 | 21/09/2025 | <a href="https://docs.aws.amazon.com/vpc/">VPC Documentation</a> |
| Wed | - Set up an Application Load Balancer (ALB) <br> - Configure target groups for EC2 instances | 18/09/2025 | 21/09/2025 | <a href="https://docs.aws.amazon.com/elasticloadbalancing/">ELB Documentation</a> |
| Thu | - Launch multiple EC2 instances in the VPC <br> - Register instances with load balancer | 18/09/2025 | 21/09/2025 | <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/">EC2 User Guide</a> |
| Fri | - Test load balancing across instances <br> - Monitor health checks and traffic distribution | 18/09/2025 | 21/09/2025 | <a href="https://docs.aws.amazon.com/elasticloadbalancing/latest/application/">ALB User Guide</a> |
| Sat | - Document VPC architecture <br> - Optimize security group rules | 18/09/2025 | 21/09/2025 | — |

### Week 4 Achievements

- Successfully created a VPC with proper subnet segmentation and CIDR planning.
- Configured security groups with least-privilege inbound and outbound rules.
- Set up an Application Load Balancer and registered EC2 instances as targets.
- Verified health checks and load balancing functionality across multiple instances.
- Learned key differences between security groups and NACLs and their use cases.
- Tested failover behavior when instances were unhealthy.

### Challenges and lessons learned

- CIDR block conflicts require careful planning; overlapping subnets can cause routing issues.
- Health check configuration directly impacts instance availability in the load balancer.
- Security group rules must allow ALB traffic on the correct ports and protocols.
