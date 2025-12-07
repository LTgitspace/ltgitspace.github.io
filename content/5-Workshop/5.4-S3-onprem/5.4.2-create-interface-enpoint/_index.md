---
title: "Create an S3 Interface Endpoint"
date: "2025-01-01"
weight: 2
chapter: false
pre: " <b> 5.4.2 </b> "
---

In this section, you will create and test S3 Interface Endpoint using the simulated on-premises environment.

1. Go back to Amazon VPC menu. In the left navigation bar, select Endpoints, then click Create Endpoint.

2. In the Create endpoint console:
+ Set name for interface endpoint
+ In Service category, select **aws services** 

![Name](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint1.png)

3. In Search box, type S3 and press Enter. Select endpoint named com.amazonaws.us-east-1.s3. Ensure that the Type column has value Interface.

![Service](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint2.png)

4. For VPC, select VPC Cloud from the drop-down.
{{% notice warning %}}
Ensure that you select "VPC Cloud" and not "VPC On-prem"
{{% /notice %}}
+ Expand **Additional settings** and ensure that Enable DNS name is *not* selected (you will use this in the next part of the workshop)

![VPC](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint3.png)

5. Select 2 subnets in the following AZs: us-east-1a and us-east-1b

![Subnets](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint4.png)

6. For Security group, select SGforS3Endpoint:

![Security Group](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint5.png)

7. Keep default policy - full access and click Create endpoint

![Success](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint-success.png)

Congratulations! You have successfully created S3 interface endpoint. In the next step, we will test the interface endpoint.

