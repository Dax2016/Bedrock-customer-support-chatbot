# Bedrock-customer-support-chatbot


A customer support chatbot built with **Amazon Bedrock Flows** for a fictional online shopping platform. The chatbot is designed to handle two primary categories of customer requests:

1. **Platform FAQ questions** — answered using the provided online shop FAQ knowledge.
2. **Bug reports** — collected and submitted through a backend tool that creates a support ticket.

## Project Overview

This project demonstrates how Amazon Bedrock Flows can be used to orchestrate a customer support workflow that routes customer requests to the appropriate processing path.

The system is designed to distinguish between informational FAQ requests and technical bug reports.

For bug reports, the workflow collects the required information and invokes a backend function that stores the resulting support ticket in Amazon DynamoDB.

## Architecture

```text
Customer
   │
   ▼
Amazon Bedrock Flow
   │
   ├── FAQ Request
   │       │
   │       ▼
   │   Online Shop FAQ
   │       │
   │       ▼
   │   Customer Response
   │
   └── Bug Report
           │
           ▼
      Bug Information
           │
           ▼
     create_bug_report
        Lambda Function
           │
           ▼
      Amazon DynamoDB
           │
           ▼
      Ticket ID / Status
```

## Core Components

### Amazon Bedrock Flows

The Bedrock Flow orchestrates the customer support workflow and determines how incoming customer requests are processed.

### FAQ Knowledge

`online_shop_faq.md` contains the fictional online shopping platform's FAQ information used to answer supported platform questions.

### Bug Report Lambda

`create_bug_report.py` implements the backend function responsible for creating bug-report tickets.

The Lambda function:

* Receives structured parameters from the Bedrock workflow.
* Validates the bug description.
* Captures reproduction steps and environment information.
* Generates a unique ticket ID.
* Creates an `OPEN` support ticket.
* Stores the ticket in Amazon DynamoDB.
* Returns the ticket ID and status.

### Amazon DynamoDB

Bug reports are persisted in a DynamoDB table whose name is supplied through the Lambda environment variable:

```text
TABLE_NAME
```

Each ticket contains information such as:

```text
ticketId
description
stepsToReproduce
environment
status
createdAt
```

## Repository Structure

```text
Bedrock-customer-support-chatbot/
│
├── README.md
├── .gitignore
├── cloudformation-testing.yaml
├── cloudformation-tool.yaml
├── create_bug_report.py
├── flow-tests-template.json
├── generate-eval-dataset.py
├── online_shop_faq.md
└── requirements.txt
```

## Requirements

The project uses Python and the AWS SDK for Python (`boto3`).

Dependencies are defined in:

```text
requirements.txt
```

Install them with:

```bash
pip install -r requirements.txt
```

AWS resources used by the project require appropriate IAM permissions.

## Configuration

The bug-report Lambda function expects the following environment variable:

```text
TABLE_NAME
```

`TABLE_NAME` should contain the name of the DynamoDB table used to store bug reports.

AWS credentials should **never** be committed to this repository.

Use the AWS credential mechanism appropriate for your execution environment, such as:

* IAM roles
* AWS CLI profiles
* Udacity-provided AWS credentials
* IAM Identity Center
* Environment-specific credential configuration

## Infrastructure

The repository includes CloudFormation templates for project infrastructure and testing:

```text
cloudformation-tool.yaml
cloudformation-testing.yaml
```

Review these templates before deployment and ensure that the AWS resources and permissions match the intended project environment.

## Testing

The repository includes resources for testing the Bedrock Flow:

```text
flow-tests-template.json
```

The project also includes:

```text
generate-eval-dataset.py
```

which can be used as part of the evaluation/testing workflow.

Testing should cover both major customer-support scenarios:

### FAQ Scenario

A customer asks a question that can be answered using the online shop FAQ.

Expected behavior:

```text
Customer question
       ↓
FAQ handling
       ↓
Relevant FAQ information
       ↓
Customer response
```

### Bug Report Scenario

A customer reports a technical problem.

Expected behavior:

```text
Customer bug report
       ↓
Required information collected
       ↓
create_bug_report
       ↓
DynamoDB
       ↓
Ticket ID returned
```

## Security

Do not commit:

* AWS access keys
* AWS secret keys
* API keys
* passwords
* session credentials
* private configuration files
* other sensitive credentials

The `.gitignore` file should be maintained to prevent accidental inclusion of sensitive files.

## Project Status

The repository contains the starter project resources and implementation components for the Amazon Bedrock customer support chatbot.

The Bedrock Flow configuration and AWS-managed resources are deployed and managed within the AWS environment.

## Technology Stack

* **Amazon Bedrock**
* **Amazon Bedrock Flows**
* **AWS Lambda**
* **Amazon DynamoDB**
* **AWS CloudFormation**
* **Python**
* **Boto3**
* **Git / GitHub**

## Project Context

This project is part of the **Udacity AWS AI & ML Scholars / Future AWS Agent Engineer learning experience** and demonstrates practical application of AWS generative AI and agentic workflow concepts.

## License

This repository is intended for educational and project purposes.
