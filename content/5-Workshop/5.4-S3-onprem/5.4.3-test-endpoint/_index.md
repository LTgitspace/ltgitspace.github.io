---
title: "Test Interface Endpoint"
date: "2025-01-01"
weight: 3
chapter: false
pre: " <b> 5.4.3 </b> "
---

## Get Regional DNS Name of S3 Interface Endpoint

1. In Amazon VPC menu, select Endpoints.

2. Click the name of the endpoint we created in section 4.2: s3-interface-endpoint. Click details and save the regional DNS name of the endpoint (the first one) to your text-editor for use in following steps.

![DNS Name](/images/5-Workshop/5.4-S3-onprem/dns.png)

## Connect to EC2 Instance in "VPC On-prem" (Simulated On-premises Environment)

1. Go to **Session manager** by typing "session manager" in the search box

2. Click **Start Session**, select EC2 instance named **Test-Interface-Endpoint**. This EC2 instance is running on "VPC On-prem" and will be used to test connectivity to Amazon S3 through Interface endpoint. Session Manager will open a new browser tab with shell prompt: **sh-4.2 $**

![Start Session](/images/5-Workshop/5.4-S3-onprem/start-session.png)

3. Go to ssm-user's home directory with command "cd ~"

4. Create a file named testfile2.xyz
```bash
fallocate -l 1G testfile2.xyz
```

![User](/images/5-Workshop/5.4-S3-onprem/cli1.png)

5. Copy file to S3 bucket you created in section 4.2
```bash
aws s3 cp --endpoint-url https://bucket.<Regional-DNS-Name> testfile2.xyz s3://<your-bucket-name>
``` 
+ This command requires the --endpoint-url parameter, because you need to use the DNS name specific to the endpoint to access S3 through Interface endpoint.
+ Do not include the '*' when copy/pasting the regional DNS name.
+ Provide your S3 bucket name

![Copy File](/images/5-Workshop/5.4-S3-onprem/cli2.png)

The file has now been added to your S3 container. Let's verify the container in the next step.

## Verify Object in S3 Bucket

1. Go to S3 console
2. Click Buckets
3. Click your bucket name and you will see testfile2.xyz has been added to your s3 bucket

![Check Bucket](/images/5-Workshop/5.4-S3-onprem/check-bucket.png)

Success! You have verified that the Interface Endpoint is working correctly by uploading a file from the simulated on-premises environment to S3 through the PrivateLink connection.
---
title: "Prepare Resources"
date: "2025-01-01"
weight: 1
chapter: false
pre: " <b> 5.4.1 </b> "
---

To prepare for this section of the workshop, you will need to:
+ Deploy CloudFormation stack
+ Modify VPC route table

These components work together to simulate DNS forwarding and name resolution.

## Deploy CloudFormation Stack

The CloudFormation template will create additional services to support the on-premises environment simulation:
+ A Route 53 Private Hosted Zone hosting Alias records for S3 PrivateLink endpoints
+ A Route 53 Inbound Resolver endpoint allowing "VPC Cloud" to resolve DNS requests sent to the Private Hosted Zone
+ A Route 53 Outbound Resolver endpoint allowing "VPC On-prem" to forward DNS requests for S3 to "VPC Cloud"

![Route 53 diagram](/images/5-Workshop/5.4-S3-onprem/route53.png)

1. Click the following link to open [AWS CloudFormation console](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https://s3.amazonaws.com/reinvent-endpoints-builders-session/R53CF.yaml&stackName=PLOnpremSetup). The template will be pre-loaded into the menu. Accept all defaults and click Create stack.

![Create stack](/images/5-Workshop/5.4-S3-onprem/create-stack.png)

![Button](/images/5-Workshop/5.4-S3-onprem/create-stack-button.png)

Stack deployment may take a few minutes to complete. You can proceed with the next step without waiting for the deployment to finish.

## Update Private On-premises Route Table

This workshop uses StrongSwan VPN running on an EC2 instance to simulate connectivity between on-premises data center and AWS cloud environment. Most required components are provided before you start. To complete VPN configuration, you will modify the "VPC on-prem" route table to direct traffic to cloud through StrongSwan VPN instance.

1. Open Amazon EC2 console 

2. Select instance named infra-vpngw-test. From Details tab, copy Instance ID and paste into your text editor for use in following steps

![EC2 ID](/images/5-Workshop/5.4-S3-onprem/ec2-onprem-id.png)

3. Go to VPC menu by typing "VPC" in Search box

4. Click on Route Tables, select RT Private On-prem route table, select Routes tab, and click Edit Routes.

![Route Table](/images/5-Workshop/5.4-S3-onprem/rt.png)

5. Click Add route.
+ Destination: CIDR block of Cloud VPC
+ Target: ID of infra-vpngw-test instance (you saved this in previous step)

Complete the route configuration and save changes.

