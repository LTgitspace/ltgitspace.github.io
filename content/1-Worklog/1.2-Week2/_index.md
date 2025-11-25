---
title: "Week 2 Worklog"
date: "2025-09-21"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---


### Week 2 Objectives

- Learn commonly used AWS services in more depth.
- Join a major AWS event and capture key takeaways.

### Tasks carried out this week
| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| Mon | - Team scrum for project proposal <br> - Set up Linux environment | 11/09/2025 | 14/09/2025 | — |
| Tue | — | 11/09/2025 | 14/09/2025 | — |
| Wed | - Help teammates set up Linux and Windows environments | 11/09/2025 | 14/09/2025 | <a href="https://ubuntu.com/tutorials">Ubuntu tutorials</a> · <a href="https://learn.microsoft.com/windows/wsl/install">WSL install</a> |
| Thu | - Attend AWS Cloud Day event | 11/09/2025 | 14/09/2025 | <a href="https://aws.amazon.com/events/">AWS events</a> |
| Fri | - Acquire domain on Route 53 and set up SSL via Cloudflare <br> - Read docs for EC2, ECS, ECR, EKS, VPC, Lambda | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html">Route 53: Register domain</a> · <a href="https://developers.cloudflare.com/ssl/">Cloudflare SSL/TLS</a> · <a href="https://docs.aws.amazon.com/ec2/">EC2</a> · <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html">ECS</a> · <a href="https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html">ECR</a> · <a href="https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html">EKS</a> · <a href="https://docs.aws.amazon.com/vpc/">VPC</a> · <a href="https://docs.aws.amazon.com/lambda/latest/dg/welcome.html">Lambda</a> |
| Sat | - Learn EC2 Auto Scaling and deploy an existing backend on EC2 with Docker | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html">EC2 Auto Scaling</a> · <a href="https://docs.docker.com/engine/install/">Install Docker</a> |

### Week 2 Achievements

- Deployed a web application with a custom domain and HTTPS termination via Cloudflare.
- Helped teammates standardize development environments (Linux/Windows) for smoother collaboration.
- Strengthened knowledge across key AWS services: EC2, ECS, ECR, EKS, VPC, and Lambda.
- Practiced scaling concepts with EC2 Auto Scaling and containerized deployment with Docker on EC2.

### Challenges and lessons learned

- DNS and certificate changes can take time to propagate; plan for TTLs and validate records before cutover.
- Mixing providers (Route 53 + Cloudflare) requires careful name server and SSL/TLS configuration.
- Environment setup differs across OSes—document a minimal, repeatable setup path for teammates.
- When troubleshooting deployments on EC2, start with security groups, NACLs, and instance logs.
