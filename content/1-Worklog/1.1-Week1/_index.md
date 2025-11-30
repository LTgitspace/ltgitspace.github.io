---
title: "Week 1 Worklog"
date: "2025-09-14"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---


### Week 1 Objectives

- Connect and get acquainted with First Cloud Journey (FCJ) members.
- Understand core AWS service categories and learn to use both the Console and the AWS CLI.

### Tasks carried out this week
| Day | Task | Start Date | Completion Date | Reference Material |
|-----|------|------------|-----------------|--------------------|
| Mon | - Team onboarding with FCJ members <br> - Read and internalize internship rules and regulations | 11/09/2025 | 14/09/2025 | — |
| Tue | - Overview of AWS and core service groups <br>&emsp; • Compute <br>&emsp; • Storage <br>&emsp; • Networking <br>&emsp; • Database | 11/09/2025 | 14/09/2025 | <a href="https://aws.amazon.com/what-is-aws/">What is AWS?</a> · <a href="https://aws.amazon.com/products/">AWS products</a> |
| Wed | - Weekly Scrum/Meeting: Progress report and team synchronization | 11/09/2025 | 14/09/2025 | — |
| Thu | - Learn EC2 fundamentals: <br>&emsp; • Instance types <br>&emsp; • AMIs <br>&emsp; • EBS <br>&emsp; • Key pairs & security groups <br> - SSH methods for EC2 <br> - Start coding a personal FCJ report site while learning Angular | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/ec2/">EC2 docs</a> · <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html">Connect to EC2</a> · <a href="https://angular.dev/overview">Angular overview</a> |
| Fri | - Practice: <br>&emsp; • Launch an EC2 instance <br>&emsp; • Connect via SSH <br>&emsp; • Attach an EBS volume | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html">Launch EC2</a> · <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-to-linux-instance.html">SSH to EC2</a> · <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html">Attach EBS</a> |
| Sat | - Create an S3 bucket and configure static website hosting <br> - Deploy the personal FCJ static site | 11/09/2025 | 14/09/2025 | <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html">S3 static website</a> |

### Week 1 Achievements

- Gained a solid understanding of AWS fundamentals and the main service groups: Compute, Storage, Networking, and Database.
- Successfully created and configured an AWS Free Tier account.
- Became familiar with the AWS Management Console and how to find, access, and use services via the web UI.
- Installed and configured the AWS CLI, including Access Key, Secret Key, and default Region settings.
- Launched a test EC2 instance, connected via SSH, and attached an EBS volume.
- Deployed a simple static website to Amazon S3 for FCJ reporting.

### Challenges and lessons learned

- IAM permissions and key management require careful handling; least-privilege is essential.
- Security groups and networking settings (inbound/outbound rules) directly impact connectivity; validate them first when SSH fails.
- Region selection affects service availability and pricing—standardize on a primary region early.
