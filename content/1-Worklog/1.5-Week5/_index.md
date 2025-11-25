---
title: "Week 5 Worklog"
date: "2025-10-05"
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 5 Objectives

- Build and push Docker images to Amazon ECR.
- Deploy containerized applications using ECS Fargate.
- Integrate ECR with CodePipeline for automated deployments.

### Tasks carried out this week
| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| Mon | - Create ECR repositories for application images <br> - Configure IAM permissions for ECR access | 25/09/2025 | 28/09/2025 | <a href="https://docs.aws.amazon.com/AmazonECR/latest/userguide/">ECR Documentation</a> |
| Tue | - Build Docker images locally <br> - Push images to ECR using AWS CLI | 25/09/2025 | 28/09/2025 | <a href="https://docs.docker.com/engine/reference/commandline/">Docker CLI Reference</a> |
| Wed | - Create ECS Fargate task definitions <br> - Configure container environment variables and resource limits | 25/09/2025 | 28/09/2025 | <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html">Task Definitions Guide</a> |
| Thu | - Launch ECS services using Fargate launch type <br> - Attach services to the load balancer created in Week 4 | 25/09/2025 | 28/09/2025 | <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html">ECS Services Guide</a> |
| Fri | - Test automatic task scaling based on CPU and memory <br> - Verify service availability through the load balancer | 25/09/2025 | 28/09/2025 | <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-auto-scaling.html">Auto Scaling Guide</a> |
| Sat | - Update image version in ECR <br> - Test rolling deployment to verify blue-green pattern | 25/09/2025 | 28/09/2025 | — |

### Week 5 Achievements

- Successfully built and pushed Docker images to ECR repositories.
- Created reusable ECS Fargate task definitions with proper resource allocation.
- Deployed containerized services and integrated them with the Application Load Balancer.
- Configured service auto-scaling policies for CPU and memory metrics.
- Tested rolling deployments without service downtime.
- Learned image tagging best practices for version control and deployment.

### Challenges and lessons learned

- ECR repository permissions must be correctly configured for push/pull operations.
- Task definition compatibility between local Docker and Fargate runtime requires attention to base images.
- Service updates can be time-consuming; consider using service deployment configuration parameters.
