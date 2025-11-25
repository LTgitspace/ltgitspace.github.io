---
title: "Week 6 Worklog"
date: "2025-10-12"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 6 Objectives

- Set up continuous integration and deployment (CI/CD) pipeline using AWS CodePipeline.
- Integrate GitHub repository with CodePipeline for automated deployments.
- Implement automated testing in the pipeline.

### Tasks carried out this week
| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| Mon | - Create a CodePipeline project <br> - Configure GitHub source connection and credentials | 02/10/2025 | 05/10/2025 | <a href="https://docs.aws.amazon.com/codepipeline/latest/userguide/">CodePipeline Documentation</a> |
| Tue | - Set up CodeBuild project for automated Docker image building <br> - Create buildspec.yml for build process | 02/10/2025 | 05/10/2025 | <a href="https://docs.aws.amazon.com/codebuild/latest/userguide/">CodeBuild Documentation</a> |
| Wed | - Configure ECR as deployment target <br> - Create deployment stage in CodePipeline | 02/10/2025 | 05/10/2025 | <a href="https://docs.aws.amazon.com/codepipeline/latest/userguide/integrations.html">CodePipeline Integrations</a> |
| Thu | - Add manual approval step before production deployment <br> - Test pipeline execution with code commits | 02/10/2025 | 05/10/2025 | <a href="https://docs.aws.amazon.com/codepipeline/latest/userguide/concepts.html">CodePipeline Concepts</a> |
| Fri | - Add CloudWatch notifications for pipeline events <br> - Document the pipeline stages and workflow | 02/10/2025 | 05/10/2025 | <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/">CloudWatch Events Guide</a> |
| Sat | - Test end-to-end deployment flow <br> - Verify ECR image updates and ECS service deployment | 02/10/2025 | 05/10/2025 | — |

### Week 6 Achievements

- Successfully created a working CI/CD pipeline from GitHub to ECS.
- Automated Docker image building and ECR push via CodeBuild.
- Implemented manual approval gates for production deployments.
- Set up CloudWatch notifications for pipeline state changes.
- Reduced manual deployment steps from 8+ steps to 1 git push.
- Learned pipeline best practices for failure handling and rollback scenarios.

### Challenges and lessons learned

- GitHub token authentication requires proper IAM policies for CodePipeline.
- CodeBuild environment variables must include AWS credentials for ECR push operations.
- Pipeline failures can be cryptic; CloudWatch logs are essential for debugging.
