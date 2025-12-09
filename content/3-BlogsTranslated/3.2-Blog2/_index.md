---
title: "Blog 2"
date: "2025-01-01"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---


# Automate document translation and standardization with Amazon Bedrock and Amazon Translate | Artificial Intelligence

**by Nadhya Polanco and Steve Bell**
*Published on 01 MAY 2025*
*Categories: Advanced (300), Amazon Bedrock, Amazon Translate, Customer Solutions*

---

Multinational organizations face the complex challenge of effectively managing a workforce and operations across different countries, cultures, and languages. Maintaining consistency and alignment across these global operations can be difficult, especially when it comes to updating and sharing business documents and processes. Delays or miscommunications can lead to productivity losses, operational inefficiencies, or potential business disruptions. Accurate and timely sharing of translated documents across the organization is an important step in making sure that employees have access to the latest information in their native language.

In this post, we show how you can automate language localization through translating documents using Amazon Web Services (AWS). The solution combines **Amazon Bedrock** and **AWS Serverless technologies**, a suite of fully managed event-driven services for running code, managing data, and integrating applications—all without managing servers. Amazon Bedrock is a fully managed service that offers a choice of high-performing foundation models (FMs) from leading AI companies like AI21 Labs, Anthropic, Cohere, Meta, Mistral AI, and Stability AI, accessible through a single API, along with a broad set of capabilities you need to build generative AI applications with security, privacy, and responsible AI.

## Solution Overview

The solution uses **AWS Step Functions** to orchestrate the translation of the source document into the specified language (English, French, or Spanish) using **AWS Lambda** functions to call **Amazon Translate**. Amazon Translate currently supports translation of 75 languages, but only three are chosen for this demo. It then uses **Amazon Bedrock** to refine the translation and create natural, flowing content.

Building this solution on AWS fully managed and serverless technologies eliminates the need to operate infrastructure, manage capacity, or invest significant funding upfront. The compute and AI services used to process documents for translation run only on demand, resulting in a consumption-based billing model.

### Solution Architecture

[Diagram of the document translation and standardization workflow]

The document translation and standardization workflow consists of the following steps:

1.  The user uploads their source document requiring translation to the input **Amazon Simple Storage Service (Amazon S3)** bucket. The bucket has three folders: English, French, and Spanish. The document is uploaded to the folder that matches its current language.
2.  The presence of a new document in the input bucket initiates the **Step Functions** workflow using **Amazon S3 Event Notifications**.
3.  The first step is an **AWS Lambda function** that retrieves the source document and calls the **Amazon Translate API** `TranslateDocument`.
4.  The second step is another **Lambda function** that queries **Amazon Bedrock** using a pre-generated prompt, including the translated document. This prompt instructs Amazon Bedrock to perform a **transcreation check** to validate that the intent, style, and tone of the document is maintained. The final version is saved in the output S3 bucket.
5.  The last step uses **Amazon Simple Notification Service (Amazon SNS)** to notify an SNS topic of the workflow's outcome (success or failure), sending an email to subscribers.
6.  The user downloads their translated document from the output S3 bucket.

This solution is available on GitHub and provides the **AWS Cloud Development Kit (AWS CDK)** code to deploy in your own AWS account.

## Prerequisites

For this walkthrough, you should have the following prerequisites:

* An AWS account.
* An AWS Identity and Access Management (IAM) role with sufficient permissions (administrator access is sufficient).
* The AWS CDK installed on your local machine, or an AWS Cloud9 environment.
* Python 3.9 or later.
* Docker.

## Deployment Steps

To deploy this solution:

1.  Open your code editor and authenticate to your AWS account.
2.  Clone the solution from the GitHub repository:
    ```bash
    git clone [https://github.com/aws-samples/sample-document-standardization-with-bedrock-and-translate.git](https://github.com/aws-samples/sample-document-standardization-with-bedrock-and-translate.git)
    ```
3.  Follow the deployment instructions in the repository README file.
4.  After the stack is deployed, navigate to the S3 bucket created: `docstandardizationstack-inputbucket`.
5.  Upload the `word_template.docx` file included in the repository to automatically create the English, French, and Spanish folders.
6.  Navigate to the **Amazon SNS console** and create a subscription to the topic `DocStandardizationStack-ResultTopic`. **Confirm the subscription** via the automated email.

## Language Translation

To test the workflow, upload a `.docx` file (like the included `tone_test.docx`) to the folder corresponding to the document's original language (e.g., the `English` folder).

The Step Functions state machine will start, and translated versions of your source document will be added to the other language folders.

### Transcreation Process

The translated documents are then processed using **Amazon Bedrock**. Amazon Bedrock reviews the documents' intent, style, and tone for use in a business setting. You can customize the output tone and style by modifying the Amazon Bedrock prompt.

The final documents are added to the output S3 bucket (`DocStandardizationStack-OutputBucket`) with a suffix of `_corrected`.


The prompt used for the transcreation task is designed to produce consistent and valid adjustments by including specific instructions and rules to define boundaries that control adjustments.

When the documents have been processed, you will receive an SNS notification.

## Clean up

To delete the deployed resources, run the command `cdk destroy` in your terminal, or use the CloudFormation console to delete the CloudFormation stack `DocStandardizationStack`.

## Conclusion

This post demonstrated how to automate the translation of business documents using AWS AI and serverless technologies. This automated process can improve communication, consistency, and alignment across global operations. By embracing the capabilities of AWS, companies can focus on their core business objectives without creating additional IT infrastructure overhead.

*Bonne traduction!*

*Feliz traducción!*

*Happy translating!*

### Further reading

The solution includes a zero-shot prompt with specific instructions. To adjust your results, you can use the **Amazon Bedrock Prompt Management** tool to quickly edit and test the impact of changes to the prompt text.

For additional examples, visit the [AWS Workshops page](https://aws.amazon.com/training/aws-workshops/).
