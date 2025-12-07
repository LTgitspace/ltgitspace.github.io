---
title: "Module 4: Access S3 from On-premises Environment"
date: "2025-01-01"
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

## Overview

In this section, you will create an Interface Endpoint to access Amazon S3 from a simulated on-premises environment. The Interface Endpoint will allow you to route traffic to Amazon S3 through a VPN connection from your simulated on-premises environment.

Why use **Interface Endpoint**:
- Gateway endpoints only work with resources running in the VPC where they are created. Interface endpoints work with resources running in VPCs and resources running in on-premises environments. Connectivity from your on-premises environment to AWS cloud can be provided by AWS Site-to-Site VPN or AWS Direct Connect.
- Interface endpoints allow you to connect to services provided by AWS PrivateLink. These services include AWS services, services hosted by AWS partners and customers in their own VPCs (called PrivateLink endpoints), and AWS Marketplace Partner services. For this workshop, we will focus on connecting to Amazon S3.

![Interface endpoint architecture](/images/5-Workshop/5.4-S3-onprem/diagram3.png)

## Content

- [Prepare resources](5.4.1-prepare/)
- [Create Interface endpoint](5.4.2-create-interface-enpoint/)
- [Test endpoint](5.4.3-test-endpoint/)
- [DNS simulation](5.4.4-dns-simulation/)
---
title: "Module 3: Access S3 from VPC"
date: "2025-01-01"
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

## Using Gateway Endpoint

In this section, you will create a Gateway endpoint to access Amazon S3 from an EC2 instance. The Gateway endpoint will allow you to upload an object to an S3 bucket without using the Public Internet. To create an endpoint, you must specify the VPC where you want to create the endpoint and the service (in this case S3) that you want to establish connectivity to.

![Overview](../images/5-Workshop/5.3-S3-vpc/diagram2.png)

## Content

- [Create gateway endpoint](3.1-create-gwe/)
- [Test gateway endpoint](3.2-test-gwe/)

