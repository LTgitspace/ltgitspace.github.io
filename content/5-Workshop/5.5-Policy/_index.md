---
title: "Module 5: VPC Endpoint Policies"
date: "2025-01-01"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

## Overview

When you create an Interface Endpoint or gateway endpoint, you can attach an endpoint policy to control access to the service you are connecting to. A VPC Endpoint policy is an IAM resource policy that you attach to the endpoint. If you do not attach a policy when creating the endpoint, AWS will attach a default policy for you to allow full access to the service through the endpoint.

You can create a policy that restricts access to only specific S3 buckets. This is useful if you only want certain S3 buckets to be accessible through the endpoint.

In this section, you will create a VPC Endpoint policy that restricts access to S3 buckets specified in the VPC Endpoint policy.

![Endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy.png)

---

## Connect to EC2 and Verify S3 Connectivity

1. Start a new AWS Session Manager session on the server named Test-Gateway-Endpoint. From this session, verify that you can list the contents of the bucket you created in Part 1: Access S3 from VPC.

```bash
aws s3 ls s3://<your-bucket-name>
```

![Test](/images/5-Workshop/5.5-Policy/test1.png)

The bucket contents include two files of 1GB size that were previously uploaded.

2. Create a new S3 bucket; follow the naming pattern you used in Part 1, but add '-2' to the name. Leave other fields as default and click **Create**.

![Create bucket](/images/5-Workshop/5.5-Policy/create-bucket.png)

3. Bucket created successfully.

![Success](/images/5-Workshop/5.5-Policy/create-bucket-success.png)

The default policy allows access to all S3 buckets through the VPC endpoint.

4. In the **Edit Policy** interface, copy and paste the following policy, replacing yourbucketname-2 with your second bucket name. This policy will allow access to the new bucket through the VPC endpoint, but not allow access to other buckets. Select **Save** to activate the policy.

```json
{
  "Id": "Policy1631305502445",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1631305501021",
      "Action": "s3:*",
      "Effect": "Allow",
      "Resource": [
        "arn:aws:s3:::yourbucketname-2",
        "arn:aws:s3:::yourbucketname-2/*"
      ],
      "Principal": "*"
    }
  ]
}
```

![Custom policy](/images/5-Workshop/5.5-Policy/policy2.png)

Policy configuration successful.

![Success](/images/5-Workshop/5.5-Policy/success.png)

5. From your session on the Test-Gateway-Endpoint instance, check access to the S3 bucket you created in the first step

```bash
aws s3 ls s3://<yourbucketname>
```

The command returns an error because access to the S3 bucket is not allowed by the VPC endpoint policy.

![Error](/images/5-Workshop/5.5-Policy/error.png)

6. Return to home directory on your EC2 instance ```cd~```

+ Create a file ```fallocate -l 1G test-bucket2.xyz ```
+ Copy file to the second bucket ```aws s3 cp test-bucket2.xyz s3://<your-2nd-bucket-name>```

![Success](/images/5-Workshop/5.5-Policy/test2.png)

This operation is allowed by the VPC endpoint policy.

![Success](/images/5-Workshop/5.5-Policy/test2-success.png)

Then we check access to the first S3 bucket

```bash
aws s3 cp test-bucket2.xyz s3://<your-1st-bucket-name>
```

![Fail](/images/5-Workshop/5.5-Policy/test2-fail.png)

The command fails because the bucket does not have access permissions from the VPC endpoint policy.

---

## Summary

In this section, you created a VPC Endpoint policy for Amazon S3 and used AWS CLI to test the policy. AWS CLI operations related to your original S3 bucket failed because you applied a policy that only allows access to the second bucket you created. AWS CLI operations targeting your second bucket succeeded because the policy allows them. These policies can be useful in situations where you need to control access to resources through VPC Endpoints.

