---
title: "Proposal"
date: "2025-01-01"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Smoking Cessation Support System Proposal

## 1. Executive Summary

This proposal outlines the design and implementation of a cloud-based **Smoking Cessation Support Platform** aimed at helping users quit smoking through data tracking, behavioral insights, AI coaching, and community engagement.  
The system integrates a modern, scalable backend infrastructure deployed on **AWS Cloud**, ensuring high availability, security, and seamless user experience.

The goal is to provide an intelligent, personalized journey for users to monitor, plan, and achieve their smoking cessation goals—while giving administrators and health coaches the tools to support and guide them.

---

## 2. System Objectives

- Help users build and follow personalized quit-smoking plans.
- Track smoking behavior and health progress in real-time.
- Offer AI-driven coaching, reminders, and motivational feedback.
- Enable social interaction and encouragement among members.
- Provide a secure, cloud-native infrastructure for scalability and reliability.

---

## 3. Key Features

### User-Oriented Features

- **Homepage & Knowledge Hub:** Public homepage that introduces the platform, shares success stories, FAQs, and a blog for experience sharing and educational content.
- **Registration, Membership Packages & Payments:** Users register, select membership tiers (free/premium), and complete payments via a secure third-party payment provider; subscription status drives feature access.
- **Smoking Status Tracking:** Log daily cigarette consumption, frequency, brand/cost, and triggers.
- **Quit Plan Creator & Journey Branching:** Create a cessation plan (reasons, stages, start date, target quit date). The system proposes a plan that users can customize. If relapse or abandonment occurs, the journey branches with tailored recovery steps and adjusted milestones.
- **Progress Tracking & Insights:** Show smoke-free days, money saved, health improvements, streaks, and trend charts.
- **Periodic Motivational Notifications:** Daily/weekly reminders and motivational messages via in-app, email, or SMS.
- **Achievements & Badges (Sharable):** Earn milestones (e.g., “1 Day Smoke-Free”, “₫100K Saved”) and share badges to the community feed.
- **Community Interaction:** Encourage and exchange advice with other members (comments, reactions), with basic moderation.
- **AI Coaching Agent & Roadmaps:** Personalized guidance and consultable roadmaps/knowledge articles powered by AI; includes safety disclaimers and handoff to human coaches when needed.
- **Coach Interaction:** Users can chat or schedule video sessions with platform coaches for counseling.
- **User Profile Management:** Edit profile, preferences, goals, notification settings, and privacy/consent options.
- **Ratings & Feedback:** Rate coaching sessions and content; submit feedback for continuous improvement.

### Admin & Operator Features

- **Dashboard & Reports:** Monitor user metrics, engagement, and health impact analytics.
- **Coach Portal & Case Management:** Assign caseloads, triage users at risk, and conduct chat/video counseling.
- **Membership Packages & Pricing Declaration:** Define and publish fee packages, benefits, and promotions; manage terms and conditions.
- **Payment & Subscription Management:** Manage subscriptions, invoices, refunds, and failed payment retries.
- **Content Management:** Manage homepage/blog posts, motivational messages, badges, and educational content.
- **Feedback & Rating Management:** Track and respond to user satisfaction metrics and content quality.

---

## 4. System Architecture (AWS Cloud)

The system leverages AWS-managed services for scalability and security, as visualized in the architecture diagram.

![Platform Architecture Diagram](/images/2-Proposal/arch.drawio.png)

### Frontend Layer

- **Amazon S3** hosts the static website and the web app frontend (React).
- **Amazon CloudFront** distributes the web content globally and handles SSL/TLS encryption.

### Authentication & Authorization

- **Amazon Cognito** manages user sign-up, sign-in, and identity federation, ensuring secure access for both end-users and coaches.

### Application Layer

- **AWS Lambda** powers serverless services such as payment webhooks, scheduled jobs, and lightweight API operations.
- **Coach Chat/Video:** enables real-time chat/video sessions with coaches.
- **Payments:** Integrate with a third-party payment gateway (e.g., Stripe/VNPay/MoMo) via signed webhooks and tokenized flows; keys stored in **AWS Secrets Manager**.
- **Network Load Balancer (NLB)** distributes requests across backend EC2 instances.
- **EC2 Instances (Private Subnet)** host core microservices:
    - **User Service** (profiles, auth integration)
    - **Cessation Service** (plans, tracking, achievements, relapse branching)
    - **Social Service** (community feed, comments, reactions)
    - **AI Orchestration Service** (AI coaching prompts and guardrails)

### Data Layer

- **PostgreSQL Databases** (Amazon EC2) for user, plan, and transaction data.
- **MongoDB**(Amazon EC2) for social features and unstructured content.
- **S3 Buckets** store assets (images, exports) and periodic encrypted database backups.

### DevOps Pipeline

- **Code Pipeline** automates builds, containerizes services, and pushes images to **Amazon ECR**, then deploys to EC2/Auto Scaling groups.
- **VPC Endpoint** ensures secure communication with AWS services without exposing traffic to the public internet.
- **EC2 Instance Connect Endpoint** enables controlled administrative access.

---

## 5. Security and Compliance

- **Data Encryption:** All sensitive data encrypted in transit (TLS) and at rest (AES-256).
- **IAM Policies:** Fine-grained access control for different system roles.
- **Private Subnets:** Backend and databases are isolated from public exposure.
- **VPC Link & Endpoints:** Secure internal communication between services.
- **Payments:** No card data is stored on the platform. Card handling is delegated to the payment provider; secrets managed in **AWS Secrets Manager**. PCI-DSS scope remains with the provider.
- **Privacy & Consent:** User consent, profile privacy controls, and data retention/lifecycle policies are enforced.
- **Backup Strategy:** Automated daily backups to S3 with versioning and lifecycle policies.

---

## 6. Scalability and Performance

- **Auto Scaling:** EC2 and Lambda functions scale based on user demand.
- **CDN Caching:** CloudFront caches static content for faster global delivery.
- **Load Balancing:** NLB ensures traffic distribution and fault tolerance.
- **Decoupled Microservices:** Modular design allows independent scaling of user, cessation, social, and notification services.
- **Event-Driven Workloads:** EventBridge decouples notification scheduling, badge issuance, and relapse branching workflows.

---


## 7. Pricing Estimate
**What Will Be FREE ($0.00)**:

- 2 Lambda Functions: The Free Tier includes 1 million requests and 400,000 GB-seconds of compute time per month. This is more than enough for a demo.

- 2 S3 Buckets: The Free Tier includes 5 GB of S3 Standard storage.

- 1 ECR Repository: The Free Tier includes 500 MB per month of storage for private repositories.

- 1 CloudFront Distribution: The Free Tier includes 1 TB of data transfer out and 10 million HTTP/S requests per month.

- 1 EC2 Instance: The Free Tier covers 750 hours/month of a t4g.micro instance.

- 1 CodePipeline: The Free Tier includes 1 free active pipeline per month.

- Cognito User Pool: The Free Tier includes 50,000 monthly active users.

**What is not Free (Estimated Monthly Cost)**:

- 3 EC2 Instances (t4g.small) for microservices and databases: ~$45

- 1 Network Load Balancer: ~$18

- 1 VPC Endpoint: ~$5

**Total Estimated Monthly Cost**: ~$68


## 8. Expected Outcomes

- Increased smoking cessation success rates.
- Improved user motivation and engagement.
- Scalable platform capable of supporting large user bases.
- Secure, compliant, and maintainable cloud infrastructure.

---
