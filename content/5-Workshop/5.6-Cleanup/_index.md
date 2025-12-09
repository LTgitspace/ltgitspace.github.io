---
title : "Resource Cleanup"
date: "2025-01-01"
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Resource Cleanup

Congratulations on completing this lab!
In this lab, you learned about architectural patterns to access Amazon S3 without using the Public Internet.

+ By creating a Gateway endpoint, you enabled direct communication between EC2 resources and Amazon S3, without going through the Internet Gateway.
By creating an Interface endpoint, you extended S3 connectivity to resources running on your on-premises data center through AWS Site-to-Site VPN or Direct Connect.

#### Cleanup
1. Navigate to Hosted Zones on the left side of the Route 53 console. Click on the name of the s3.us-east-1.amazonaws.com zone. Click Delete and confirm the deletion by entering the keyword "delete".

![hosted zone](/images/5-Workshop/5.6-Cleanup/delete-zone.png)

2. Disassociate Route 53 Resolver Rule - myS3Rule from "VPC Onprem" and Delete it. 

![hosted zone](/images/5-Workshop/5.6-Cleanup/vpc.png)

3. Open the CloudFormation console and delete the two CloudFormation stacks you created for this lab:
+ PLOnpremSetup
+ PLCloudSetup

![delete stack](/images/5-Workshop/5.6-Cleanup/delete-stack.png)

4. Delete the S3 buckets

+ Open the S3 console
+ Select the bucket we created for the lab, click and confirm it is empty. Click Delete and confirm the deletion.

![delete s3](/images/5-Workshop/5.6-Cleanup/delete-s3.png)

