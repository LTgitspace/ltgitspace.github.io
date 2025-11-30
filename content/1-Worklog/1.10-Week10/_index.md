---
title: "Week 10 Worklog"
date: "2025-11-09"
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 10 Objectives

- Optimize S3 storage and implement lifecycle policies.
- Configure S3 for static website hosting with CloudFront.
- Implement data encryption and access controls for S3 buckets.

### Tasks carried out this week
| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| Mon | - Review S3 bucket structure and storage classes <br> - Plan lifecycle policies for cost optimization | 30/10/2025 | 02/11/2025 | <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/">S3 Documentation</a> |
| Tue | - Create S3 lifecycle rules for transitioning objects <br> - Configure versioning and MFA delete protection | 30/10/2025 | 02/11/2025 | <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html">Lifecycle Management</a> |
| Wed | - Weekly Scrum/Meeting: Progress report and team synchronization | 30/10/2025 | 02/11/2025 | — |
| Thu | - Enable S3 access logging and configure CloudTrail for audit <br> - Set up bucket inventory for analysis | 30/10/2025 | 02/11/2025 | <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html">Logging Guide</a> |
| Fri | - Implement presigned URLs for temporary object access <br> - Test access control and permissions | 30/10/2025 | 02/11/2025 | <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html">Presigned URLs</a> |
| Sat | - Estimate cost savings from lifecycle policies <br> - Document S3 bucket configuration and access patterns | 30/10/2025 | 02/11/2025 | — |

### Week 10 Achievements

- Implemented S3 lifecycle policies to transition old objects to Glacier storage.
- Configured bucket encryption and access logging for compliance.
- Set up presigned URLs for secure, time-limited object access.
- Enabled versioning and bucket inventory for data management.
- Calculated potential 40% cost reduction through storage class optimization.
- Learned S3 bucket policies, IAM roles, and access control strategies.

### Challenges and lessons learned

- Lifecycle transitions have minimum object age requirements; plan accordingly.
- Presigned URL generation requires proper IAM permissions for the generating role.
- Enabling versioning increases storage costs due to retained object versions.
