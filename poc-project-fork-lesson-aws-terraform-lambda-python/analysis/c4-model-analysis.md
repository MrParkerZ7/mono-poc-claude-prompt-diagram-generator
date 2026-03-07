# C4 Model Analysis: AWS Terraform Lambda Python

## Level 1: System Context

### System
- **Name**: Terraform Python Lambda System
- **Description**: A serverless system that deploys an AWS Lambda function capable of invoking other Lambda functions, managed via Terraform IaC.

### Actors
| Actor | Type | Description | Interactions |
|-------|------|-------------|--------------|
| Developer | Person | Deploys and maintains the infrastructure | Run Terraform commands, view logs |
| External Caller | System/Person | Invokes the Lambda function | Trigger Lambda via SDK/API/Console |

### External Systems
| System | Type | Description | Data Exchange |
|--------|------|-------------|---------------|
| demo-python Lambda | External AWS Lambda | Target function to be invoked | Receive JSON payload, return response |
| AWS CloudWatch | External AWS Service | Logging and monitoring | Receive log events, store metrics |
| AWS IAM | External AWS Service | Identity and access management | Provide execution permissions |

### Context Diagram Data
```
┌─────────────────────────────────────────────────────────────────┐
│                        AWS Cloud                                 │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │              Terraform Python Lambda System               │  │
│  │  ┌─────────────────────────────────────────────────────┐  │  │
│  │  │           terraform_python Lambda                    │  │  │
│  │  │  - Receives invocation requests                      │  │  │
│  │  │  - Invokes demo-python Lambda                        │  │  │
│  │  │  - Returns aggregated response                       │  │  │
│  │  └─────────────────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────────────────┘  │
│                              │                                   │
│                              │ Invokes                          │
│                              ▼                                   │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │                  demo-python Lambda                        │  │
│  │                  [External System]                         │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                  │
│  ┌────────────────────┐    ┌────────────────────┐              │
│  │   CloudWatch Logs  │    │      AWS IAM       │              │
│  │  [External System] │    │  [External System] │              │
│  └────────────────────┘    └────────────────────┘              │
└─────────────────────────────────────────────────────────────────┘
          ▲                              ▲
          │ Invokes                      │ Deploys
          │                              │
┌─────────┴─────────┐          ┌─────────┴─────────┐
│  External Caller  │          │     Developer     │
│     [Person]      │          │     [Person]      │
└───────────────────┘          └───────────────────┘
```

---

## Level 2: Container Diagram

### Containers
| Container | Technology | Description | Responsibilities |
|-----------|------------|-------------|------------------|
| terraform_python Lambda | Python 3.9 / AWS Lambda | Serverless compute function | Handle invocation, call demo-python, return response |
| Terraform Configuration | HCL / Terraform | Infrastructure as Code | Define AWS resources, manage deployments |
| Source Package | Python / ZIP | Lambda deployment package | Contains application code |

### Container Relationships
| From | To | Protocol | Description |
|------|-----|----------|-------------|
| Developer | Terraform Configuration | CLI | Run terraform commands |
| Terraform Configuration | AWS Lambda | HTTPS/API | Deploy Lambda function |
| Terraform Configuration | AWS IAM | HTTPS/API | Create roles and policies |
| External Caller | terraform_python Lambda | HTTPS/AWS SDK | Invoke function |
| terraform_python Lambda | demo-python Lambda | HTTPS/AWS SDK | Invoke external function |
| terraform_python Lambda | CloudWatch Logs | HTTPS/API | Write log events |

### Container Diagram Data
```
┌─────────────────────────────────────────────────────────────────┐
│                   Terraform Python Lambda System                 │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                Terraform Configuration                    │   │
│  │  ┌────────────┐ ┌────────────┐ ┌────────────┐           │   │
│  │  │  main.tf   │ │variables.tf│ │ outputs.tf │           │   │
│  │  │            │ │            │ │            │           │   │
│  │  │ - Lambda   │ │ - Region   │ │ - ARN      │           │   │
│  │  │ - IAM Role │ │            │ │            │           │   │
│  │  │ - Policies │ │            │ │            │           │   │
│  │  └────────────┘ └────────────┘ └────────────┘           │   │
│  └──────────────────────────────────────────────────────────┘   │
│                              │                                   │
│                              │ Deploys                          │
│                              ▼                                   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                    AWS Resources                          │   │
│  │  ┌─────────────────────┐  ┌─────────────────────┐        │   │
│  │  │ terraform_python    │  │    iam_for_lambda   │        │   │
│  │  │      Lambda         │──│      IAM Role       │        │   │
│  │  │                     │  │                     │        │   │
│  │  │ Runtime: Python 3.9 │  │ - Assume Role       │        │   │
│  │  │ Handler: hello_*    │  │ - Logs permissions  │        │   │
│  │  │ Timeout: 90s        │  │ - Invoke permissions│        │   │
│  │  └─────────────────────┘  └─────────────────────┘        │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                    Source Package                         │   │
│  │  ┌─────────────────────┐  ┌─────────────────────┐        │   │
│  │  │  hello_lambda.py    │  │   mock_common.py    │        │   │
│  │  │                     │  │                     │        │   │
│  │  │ - lambda_handler()  │  │   (empty)           │        │   │
│  │  │ - invokeLambda()    │  │                     │        │   │
│  │  └─────────────────────┘  └─────────────────────┘        │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

---

## Level 3: Component Diagram

### Container: terraform_python Lambda

| Component | Technology | Description | Interfaces |
|-----------|------------|-------------|------------|
| lambda_handler | Python function | Entry point for Lambda invocation | lambda_handler(event, context) |
| invokeLambdaFunction | Python function | Helper to invoke other Lambdas | invokeLambdaFunction(functionName, payload) |
| boto3 Lambda Client | AWS SDK | AWS Lambda service client | client.invoke() |
| Environment Config | os.environ | Runtime configuration | ACCOUNT_ID, REGION, greeting |

### Component Relationships
| From | To | Description |
|------|-----|-------------|
| Lambda Runtime | lambda_handler | Entry point invocation |
| lambda_handler | Environment Config | Read ACCOUNT_ID, REGION |
| lambda_handler | invokeLambdaFunction | Delegate Lambda invocation |
| invokeLambdaFunction | boto3 Lambda Client | AWS SDK call |
| boto3 Lambda Client | AWS Lambda Service | API request |

### Component Diagram Data
```
┌─────────────────────────────────────────────────────────────────┐
│                    terraform_python Lambda                       │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                   Lambda Runtime                          │   │
│  └──────────────────────────┬───────────────────────────────┘   │
│                              │ invoke                            │
│                              ▼                                   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                  lambda_handler()                         │   │
│  │  - Reads environment variables                            │   │
│  │  - Creates payload                                        │   │
│  │  - Calls invokeLambdaFunction                             │   │
│  │  - Logs and returns response                              │   │
│  └──────────────────────────┬───────────────────────────────┘   │
│              │               │                                   │
│              │ reads         │ calls                            │
│              ▼               ▼                                   │
│  ┌────────────────┐  ┌──────────────────────────────────────┐   │
│  │ Environment    │  │       invokeLambdaFunction()         │   │
│  │ Config         │  │  - Validates functionName             │   │
│  │                │  │  - Serializes payload                 │   │
│  │ - ACCOUNT_ID   │  │  - Calls boto3 client.invoke          │   │
│  │ - REGION       │  └──────────────────────────────────────┘   │
│  │ - greeting     │                    │                        │
│  └────────────────┘                    │ uses                   │
│                                        ▼                        │
│                       ┌──────────────────────────────────────┐  │
│                       │        boto3 Lambda Client           │  │
│                       │  - client = boto3.client('lambda')   │  │
│                       │  - InvocationType: RequestResponse   │  │
│                       └──────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                                        │
                                        │ HTTPS/API
                                        ▼
                        ┌──────────────────────────────────────┐
                        │        AWS Lambda Service            │
                        │        [External System]             │
                        └──────────────────────────────────────┘
```

---

## Cross-Level Summary

| Level | Focus | Key Elements |
|-------|-------|--------------|
| L1 Context | Big picture | System, Developer, External Caller, External Lambdas |
| L2 Container | Deployable units | Terraform Config, Lambda Function, Source Package |
| L3 Component | Code structure | lambda_handler, invokeLambdaFunction, boto3 Client |
