---
title: "Blog 1"
date: "2025-01-01"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
Application Portfolio Rationalization using Generative AI with NorthBay Solutions
by Sujit Singh, Robert Murphy, and Ed Chow on 31 MAR 2025 in AWS Partner Network, Migration, Migration Solutions, Partner solutions Permalink  Comments  Share
By Sujit Singh, Partner Solutions Architect – AWS
By Robert Murphy, VP Solutions & Services – NorthBay Solutions
By Ed Chow, Director AI/ML – NorthBay Solutions

NorthBay-Solutions-AWS-Partners
NorthBay Solutions
NorthBay-Solutions-APN-Blog-CTA-Button-2025
Migrating to the cloud is a complex process with numerous trade-offs and considerations requiring a strategic approach to mitigate risks and optimize outcomes. NorthBay Solutions’ Application Portfolio Rationalization (APR), powered by Generative AI (Gen AI), helps organizations assess their application inventory and decide whether to Refactor, Replatform, Rehost, Retain, Repurchase, Retire, or Relocate each application. However, traditional APR methods that rely on manual processes can be slow, costly, and prone to errors.

As an AWS Specialization Partner and AWS Marketplace Seller, NorthBay Solutions helps organizations leverage AWS to modernize their applications efficiently.

In this post, we explore how NorthBay Solutions‘ Gen AI-powered APR solution streamlines the process, addressing key challenges and reduces costs.

Challenges of Application Portfolio Rationalization
APR presents several key challenges for enterprise organizations:

Complex Decision Making – Multiple factors must be considered for each application, such as functionality, technical debt, business value, and strategic alignment with organizational goals.
Scale – Assessing hundreds or thousands of applications simultaneously creates complexity that overwhelms manual evaluation methods.
Complex Dependencies – Larger portfolios make it harder to map and predict the impact of modernization.
Incomplete Documentation – Outdated or missing records slow down assessment processes and increase decision risks.
Knowledge Loss – Staff turnover leads to information gaps that affect decision quality.
Cost Management – Prolonged evaluations strain modernization budgets and impact ROI calculations.
NorthBay’s Generative AI APR Solution
NorthBay’s Gen AI APR solution addresses the challenges of the complex APR process and helps enterprises optimize their IT portfolios by identifying redundant, costly, and inefficient applications. The solution uses Amazon Q Business, a fully managed Gen AI-powered assistant that can answer questions, provide summaries, generate content, and securely complete tasks based on data and information in your enterprise systems. The solution also uses Amazon Q connectors. Amazon Q Business offers multiple prebuilt connectors to a variety of data sources, including Atlassian Jira, Atlassian Confluence, Amazon S3, Microsoft SharePoint, Salesforce, and many more and can help you create your generative AI solution with minimal configuration. For a full list of Amazon Q supported data source connectors, see Amazon Q connectors.

Solution Architecture
NorthBay’s APR Solution Architecture and Flow

Figure 1. NorthBay’s APR Solution Architecture and Flow

Components of NorthBay’s Gen AI APR Solution
Amazon Q web application – A customized web application built on top of the Amazon Q Business API, designed with a look and feel aligned with your organization’s brand.
AWS IAM Identity Center – Integrated with Amazon Q Business to provide centralized user authentication and access management. It can connect to your organization’s identity provider, such as Okta or Entra ID, via SSO. This makes sure that only authorized users can access the Amazon Q web application while leveraging IAM roles for secure permissions.
Application Portfolio Management Tool – Users have the option to manually upload exports from their application inventory tools or create custom data source connectors to pull data directly if the tool provides an API.
Amazon Q Business – Q Business supports connecting with your data sources both on the AWS cloud and externally.
AWS Data Sources – Users can upload relevant application data to Amazon S3 or Amazon RDS for consumption by Amazon Q Business’ engine.
External Data Sources – Amazon Q Business supports many external data sources that can contain relevant information that could support the APR process. Below are examples of external data sources that can support your APR process:
Custom Data Connectors to Application Portfolio Management (APM) Tools : With custom data connectors, a direct connection to your existing APM tools can be established via API. Amazon Q can understand stakeholders, application investment, business capabilities, and redundancies to validate your rationalization process.
Slack and Microsoft Teams : Offers insights from team discussions to understand the use and significance of various applications in your workflow.
Microsoft SharePoint and Google Drive : Document management systems contain crucial documentation about application architecture, user guides, and process workflows, which Amazon Q can analyze to understand application functionality and interdependencies.
Confluence : Integration provides access to detailed documentation, meeting notes, and project plans, giving Amazon Q context about application development and deployment.
NorthBay’s Gen AI APR Solution Process Flow
Amazon Q Business provides an intelligent, chat-based interface that aggregates data from multiple organizational sources. By leveraging Gen AI, the solution offers real-time insights into each application’s role and strategic value. The system works by:

Pulling relevant data from defined organizational sources.
Identifying key insights using Gen AI.
Generating natural language responses to user queries.
Enabling follow-up questions for deeper understanding.
Let’s look at some example questions and responses using the solution with mock data designed for demonstration purposes. Your organization would have different data and different outcomes.

Example 1 – Identify Applications for APR Based on Key Dimensions
To start, we can ask the APR solution for a summary of applications that are prime candidates for rationalization using cost, dependencies, criticality and risk level as dimensions for justifying the recommendations.

APR Solution Rationalization Prompt and Response

Figure 2. APR Solution Rationalization Prompt and Response

Amazon Q Business analyzes application data across multiple dimensions to generate strategic recommendations. In this example, the system identifies consolidation opportunities across different categories by evaluating application characteristics. For example:

HR Application Consolidation
Applications to Rationalize: TalentWare
Cost: $350,000/year
Criticality: Medium
Risk: Medium
Consolidation Potential: High due to overlapping features with TalentSync HCM and Nebula HCM Cloud
CRM Consolidation
Applications to Rationalize: LinkPoint CRM
Cost: $120,000/year
Criticality: Medium
Risk: Low
Consolidation Potential: High, as it has overlapping capabilities with HorizonConnect CRM. Consolidation would eliminate redundant CRM functionalities.
These two applications have overlapping ERP and financial management capabilities. A detailed assessment could identify opportunities to consolidate functionality.

Example 2 – Identify Applications for APR Based on Overlapping Features
Next, we can look for candidates for rationalization based on overlapping features only.

APR Solution Overlapping Features Prompt and Response

Figure 3. APR Solution Overlapping Features Prompt and Response

Amazon Q Business identifies multiple candidate applications for rationalization based on overlapping features only. For example, for ERP/Finance Systems it is recommended to

conduct detailed assessment to potentially consolidate OmniCore Suite and NexusFlow ERP as they have significant overlapping capabilities in ERP, financials, and supply chain management.

Example 3 – Identify Applications for APR Based on Criticality and Annual Cost
In the final example, we compare the criticality of each application against its annual cost. The objective is to identify applications that are high-cost but low criticality as candidates for meeting cost-saving goals.

APR Solution Criticality Prompt and Response

Figure 4. APR Solution Criticality Prompt and Response

In this example, Amazon Q Business identifies multiple candidate applications for rationalization based on their criticality versus annual cost. For example:

TalentWave is a high-cost application with no strategic fit. It has overlapping features with TalentSync HCM, VitalThrive, and Nebula HCM Cloud. Therefore, it is recommended to decommission it and consolidate functionality into existing HR applications for potential savings of $350,000.

Benefits of a Gen AI APR Solution
A successful APR yields several key outcomes that significantly benefit the organization and accelerates your migration onto the AWS cloud:

Optimized Application Portfolio: By thoroughly evaluating and categorizing each application, a successful APR makes sure that your application portfolio is streamlined and aligned with your business objectives. This includes retaining high-value applications, modernizing those with potential, and sunsetting obsolete or redundant ones. The result is a leaner, more cost-effective set of applications that better serve your business needs.
Enhanced Agility and Performance: A rationalized application portfolio is easier to manage and more responsive to changing business demands. Modernized applications typically perform better, are more scalable, and offer improved security and compliance features. This enhanced agility enables your IT department to support business innovation and growth more effectively.
Informed Decision-Making: A successful APR process provides detailed insights and documentation about your application landscape. This information is invaluable for strategic planning and decision-making, enabling IT leaders to make informed choices about future investments and initiatives.
Improved Risk Management: By identifying and addressing technical debt and security vulnerabilities in outdated applications, a successful APR helps mitigate risks associated with legacy systems. This proactive approach to risk management enhances overall system reliability and security.
These outcomes collectively drive business value, positioning the organization for future success and innovation.

Conclusion
In this post we showed how NorthBay’s APR Solution enables organizations to efficiently manage their application portfolios by leveraging Gen AI provided by Amazon Q Business.

Get started with NorthBay’s Generative AI APR Solution by subscribing to it on the AWS Marketplace and achieve the business objectives for your organization.