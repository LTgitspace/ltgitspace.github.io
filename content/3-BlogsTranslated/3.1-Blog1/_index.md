---
title: "Blog 1"
date: "2025-01-01"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Application Portfolio Rationalization using Generative AI with NorthBay Solutions | AWS Partner Network (APN) Blog

**By Sujit Singh, Robert Murphy, and Ed Chow**
*Published on 31 MAR 2025*
*Categories: AWS Partner Network, Migration, Migration Solutions, Partner solutions*

---

Migrating to the cloud is a complex process with numerous trade-offs and considerations requiring a strategic approach to mitigate risks and optimize outcomes. **NorthBay Solutions' Application Portfolio Rationalization (APR)**, powered by **Generative AI (Gen AI)**, helps organizations assess their application inventory and decide whether to **Refactor, Replatform, Rehost, Retain, Repurchase, Retire, or Relocate** each application. However, traditional APR methods that rely on manual processes can be slow, costly, and prone to errors.

As an AWS Specialization Partner and AWS Marketplace Seller, NorthBay Solutions helps organizations leverage AWS to modernize their applications efficiently.

In this post, we explore how NorthBay Solutions' Gen AI-powered APR solution streamlines the process, addressing key challenges and reduces costs.

## Challenges of Application Portfolio Rationalization

APR presents several key challenges for enterprise organizations:

* **Complex Decision Making:** Multiple factors must be considered for each application, such as functionality, technical debt, business value, and strategic alignment with organizational goals.
* **Scale:** Assessing hundreds or thousands of applications simultaneously creates complexity that overwhelms manual evaluation methods.
* **Complex Dependencies:** Larger portfolios make it harder to map and predict the impact of modernization.
* **Incomplete Documentation:** Outdated or missing records slow down assessment processes and increase decision risks.
* **Knowledge Loss:** Staff turnover leads to information gaps that affect decision quality.
* **Cost Management:** Prolonged evaluations strain modernization budgets and impact ROI calculations.

---

## NorthBay's Generative AI APR Solution

NorthBay's Gen AI APR solution addresses the challenges of the complex APR process and helps enterprises optimize their IT portfolios by identifying **redundant, costly, and inefficient applications**. The solution uses **Amazon Q Business**, a fully managed Gen AI-powered assistant that can answer questions, provide summaries, generate content, and securely complete tasks based on data and information in your enterprise systems.

The solution also uses **Amazon Q connectors**. Amazon Q Business offers multiple prebuilt connectors to a variety of data sources, including Atlassian Jira, Atlassian Confluence, Amazon S3, Microsoft SharePoint, Salesforce, and many more, helping you create your generative AI solution with minimal configuration.

### Solution Architecture



#### Components of NorthBay's Gen AI APR Solution

1.  **Amazon Q web application:** A customized web application built on top of the Amazon Q Business API.
2.  **AWS IAM Identity Center:** Integrated with Amazon Q Business to provide centralized user authentication and access management (can connect to external identity providers like Okta or Entra ID via SSO).
3.  **Application Portfolio Management Tool:** Users can manually upload exports from their application inventory tools or create custom data source connectors to pull data directly.
4.  **Amazon Q Business:** Q Business supports connecting with your data sources both on the AWS cloud and externally.
5.  **AWS Data Sources:** Users can upload relevant application data to Amazon S3 or Amazon RDS for consumption.
6.  **External Data Sources:** Amazon Q Business supports many external data sources that can support your APR process:
    * **Custom Data Connectors to Application Portfolio Management (APM) Tools:** A direct connection can be established via API. Amazon Q can understand stakeholders, application investment, business capabilities, and redundancies.
    * **Slack and Microsoft Teams:** Offers insights from team discussions.
    * **Microsoft SharePoint and Google Drive:** Contain crucial documentation about application architecture and workflows.
    * **Confluence:** Provides access to detailed documentation, meeting notes, and project plans.

#### NorthBay's Gen AI APR Solution Process Flow

Amazon Q Business provides an intelligent, chat-based interface that aggregates data from multiple organizational sources. By leveraging Gen AI, the solution offers real-time insights into each application's role and strategic value. The system works by:

* Pulling relevant data from defined organizational sources.
* Identifying key insights using Gen AI.
* Generating natural language responses to user queries.
* Enabling follow-up questions for deeper understanding.

Let's look at some example questions and responses using the solution with mock data designed for demonstration purposes.

### Example 1 – Identify Applications for APR Based on Key Dimensions



**Objective:** Ask the APR solution for a summary of applications that are prime candidates for rationalization using cost, dependencies, criticality, and risk level as dimensions.

**Results:** Amazon Q Business analyzes application data across multiple dimensions to generate strategic recommendations.

* **HR Application Consolidation:**
    * **Applications to Rationalize:** TalentWare
    * **Cost:** $350,000/year, Criticality: Medium, Risk: Medium
    * **Consolidation Potential:** High, due to overlapping features with TalentSync HCM and Nebula HCM Cloud.
* **CRM Consolidation:**
    * **Applications to Rationalize:** LinkPoint CRM
    * **Cost:** $120,000/year, Criticality: Medium, Risk: Low
    * **Consolidation Potential:** High, as it has overlapping capabilities with HorizonConnect CRM.

### Example 2 – Identify Applications for APR Based on Overlapping Features



**Objective:** Look for candidates for rationalization based on overlapping features only.

**Results:** Amazon Q Business identifies multiple candidate applications for rationalization.

* **ERP/Finance Systems:** Recommended to conduct a detailed assessment to potentially consolidate **OmniCore Suite** and **NexusFlow ERP** as they have significant overlapping capabilities in ERP, financials, and supply chain management.

### Example 3 – Identify Applications for APR Based on Criticality and Annual Cost



**Objective:** Compare the criticality of each application against its annual cost to identify high-cost but low-criticality candidates for cost-saving goals.

**Results:** Amazon Q Business identifies multiple candidate applications for rationalization based on their criticality versus annual cost.

* **TalentWave** is a high-cost application with no strategic fit. It has overlapping features with TalentSync HCM, VitalThrive, and Nebula HCM Cloud.
* **Recommendation:** Decommission it and consolidate functionality into existing HR applications for potential savings of **$350,000**.

---

## Benefits of a Gen AI APR Solution

A successful APR yields several key outcomes that significantly benefit the organization and accelerate migration onto the AWS cloud:

* **Optimized Application Portfolio:** The portfolio is streamlined and aligned with business objectives, resulting in a **leaner, more cost-effective** set of applications.
* **Enhanced Agility and Performance:** A rationalized portfolio is easier to manage and more responsive. Modernized applications typically perform better, are more scalable, and offer improved security and compliance features.
* **Informed Decision-Making:** The APR process provides detailed insights and documentation, enabling IT leaders to make **data-driven strategic decisions** about future investments.
* **Improved Risk Management:** By identifying and addressing technical debt and security vulnerabilities in outdated applications, the APR helps **mitigate risks** associated with legacy systems.

---

## Conclusion

NorthBay's APR Solution enables organizations to efficiently manage their application portfolios by leveraging **Gen AI provided by Amazon Q Business**.

Get started with NorthBay's Generative AI APR Solution by subscribing to it on the [AWS Marketplace](https://aws.amazon.com/marketplace/) and achieve the business objectives for your organization.
