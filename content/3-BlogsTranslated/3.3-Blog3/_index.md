---
title: "Blog 3"
date: "2025-01-01"
weight: 1
chapter: false
pre: " <b> 3.6. </b> "
---

# Autonomous mortgage processing using Amazon Bedrock Data Automation and Amazon Bedrock Agents | Artificial Intelligence

**by Wrick Talukdar, Jessie-Lee Fry, Farshad Bidanjiri, Jady Liu, Keith Mascarenhas, and Raj Jayaraman**
*Published on 01 MAY 2025*
*Categories: Amazon Bedrock, Amazon Bedrock Agents, Technical How-to*

---

Mortgage processing is a complex, document-heavy workflow that demands accuracy, efficiency, and compliance. Traditional mortgage operations rely on manual review, rule-based automation, and disparate systems, often leading to delays, errors, and a poor customer experience. Recent industry surveys indicate that only about half of borrowers express satisfaction with the mortgage process, largely due to the manual, error-prone nature of traditional processing.

In this post, we introduce **agentic automatic mortgage approval**, a next-generation sample solution that uses autonomous AI agents powered by **Amazon Bedrock Agents** and **Amazon Bedrock Data Automation**. These agents orchestrate the entire mortgage approval process—intelligently verifying documents, assessing risk, and making data-driven decisions with minimal human intervention.

## Why Agentic IDP?

**Agentic Intelligent Document Processing (IDP)** revolutionizes document workflows by driving efficiency and autonomy.

* **Precision and Real-time Correction:** It automates tasks with precision, enabling systems to extract, classify, and process information while identifying and correcting errors in real time.
* **Deeper Insights:** It goes beyond simple extraction by grasping context and intent, adding deeper insights to documents that fuel smarter decision-making.
* **Adaptability:** Powered by **Amazon Bedrock Data Automation**, it adapts to changing document formats and data sources, further reducing manual work.
* **Speed and Scale:** It processes high volumes of documents quickly, reducing delays and optimizing critical business operations.
* **Workflow Automation:** Seamlessly integrating with AI agents and enterprise systems, it automates complex workflows, cutting operational costs.

## IDP in Mortgage Processing

Mortgage processing involves multiple steps, including loan origination, document verification, underwriting, and closing, each requiring significant manual effort. These disjointed steps lead to slow processing times (weeks instead of minutes), high operational costs, and an increased risk of human errors and fraud.

Organizations face numerous technical challenges when manually managing document-intensive workflows, including:

* **Document overload:** Mortgage applications require verification of extensive documentation (tax records, income statements, appraisals, etc.).
* **Data entry errors:** Manual processing introduces inconsistencies, inaccuracies, and missing information.
* **Delays in decision-making:** Backlogs extend processing times and negatively affect borrower satisfaction.
* **Regulatory compliance complexity:** Evolving regulations require extensive manual updates, leading to increased processing times and error rates.

## Solution: Agentic Workflows in Mortgage Processing

[Diagram illustrating the challenges of manually managing document-intensive workflows in mortgage processing]

The proposed solution is self-contained; the applicant primarily interacts with the **mortgage applicant supervisor agent** to upload documents and check application status.

### Workflow Steps:

1.  Applicant uploads documents to apply for a mortgage.
2.  The **supervisor agent** confirms receipt of documents.
3.  Applicant can view and retrieve application status.
4.  The underwriter updates the status of the application and sends approval documents to the applicant.

### The Supervisor Agent

At the core is a **supervisor agent** that orchestrates the entire workflow, manages sub-agents, and makes final decisions. **Amazon Bedrock Agents** lets developers create AI-powered assistants capable of understanding user requests and executing complex tasks. They break down requests, interact with tools and data, and maintain conversation context.

The supervisor agent delegates tasks to specialized sub-agents. By aggregating insights from sub-agents, the supervisor agent applies established business rules and risk criteria to either **automatically approve** qualifying loans or **flag complex cases for human review**.

### Specialized Sub-Agents

#### 1. Data Extraction Agent

[Diagram illustrating the data extraction agent workflow]

* The agent uses **Amazon Bedrock Data Automation** to extract critical insights from mortgage application packages (pay stubs, W-2 forms, bank statements, etc.).
* **Amazon Bedrock Data Automation** is a Gen AI-powered capability that streamlines the development of Gen AI applications and automates workflows involving documents.
* The agent ensures the validation and compliance agents receive accurate and structured data.
* **Extraction Workflow:** The supervisor assigns the task -> Data extraction agent invokes Amazon Bedrock Data Automation -> Extracted info is stored in an Amazon S3 bucket.

#### 2. Validation Agent

[Diagram illustrating the validation agent workflow]

* The agent **cross-checks extracted data with external resources** (e.g., IRS tax records and credit reports), flagging discrepancies for review.
* It checks for inconsistencies like doctored PDFs, low credit scores, and calculates key ratios like **Debt-to-Income (DTI)** ratio and **Loan-to-Value (LTV)** limit.
* **Validation Process:** Supervisor assigns task -> Validation agent retrieves details from S3 -> Cross-checks details against third-party resources -> Generates validation status -> Sends status to supervisor agent.

#### 3. Compliance Agent

[Diagram illustrating the compliance agent workflow]

* The agent verifies that the extracted and validated data **adheres to regulatory requirements** and corporate lending rules.
* **Example Rule:** Loans are approved only if the borrower’s DTI ratio is below 43%, or applications with a credit score below 620 are declined.
* **Compliance Workflow:** Supervisor assigns task -> Compliance agent retrieves details from S3 -> Validates details against mortgage processing rules -> Calculates DTI ratio -> Generates compliance status -> Sends status to supervisor agent.

#### 4. Underwriting Agent

* The underwriting agent generates an underwriting document for the underwriter to review.

## Conclusion

The sample automated loan application solution demonstrates how **Amazon Bedrock Agents** and **Amazon Bedrock Data Automation** can transform mortgage loan processing workflows. This approach significantly **reduces manual effort, shortens processing times, and accelerates decision-making**.

Beyond mortgage processing, this solution can be adapted to streamline claims processing or other complex document-processing scenarios, helping organizations achieve greater operational efficiency, maintain consistent compliance, and deliver exceptional customer experiences.
