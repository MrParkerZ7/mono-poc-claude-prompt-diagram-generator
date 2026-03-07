# Architecture Analysis: AWS Terraform Lambda Python

## Overview
- **Project**: fork-lesson-aws-terraform-lambda-python
- **Type**: Serverless / Infrastructure-as-Code (IaC)
- **Language**: Python 3.9, Terraform (HCL)
- **Framework**: AWS Lambda, Terraform
- **Generated**: 2024

## Architecture Pattern
- **Pattern**: Serverless / Event-Driven
- **Layers**:
  1. **Infrastructure Layer**: Terraform IaC definitions
  2. **Compute Layer**: AWS Lambda function
  3. **Security Layer**: IAM roles and policies
  4. **Logging Layer**: CloudWatch Logs

## Entry Points

### Lambda Function
| Function | Handler | Runtime | Timeout | Description |
|----------|---------|---------|---------|-------------|
| terraform_python | hello_lambda.lambda_handler | Python 3.9 | 90s | Main Lambda function that invokes another Lambda |

### Terraform Entry
| Command | Description |
|---------|-------------|
| terraform init | Initialize Terraform providers |
| terraform plan | Preview infrastructure changes |
| terraform apply | Deploy infrastructure to AWS |
| terraform destroy | Remove all infrastructure |

## Components

### Infrastructure Components (Terraform)

#### main.tf
- **Path**: `main.tf`
- **Purpose**: Primary Terraform configuration
- **Resources Defined**:
  - `aws_iam_role.iam_for_lambda` - IAM execution role
  - `aws_iam_role_policy.frontend_lambda_role_policy` - IAM policy attachment
  - `aws_lambda_function.lambda` - Lambda function resource
- **Data Sources**:
  - `aws_caller_identity.current` - Get AWS account ID
  - `archive_file.lambda_source_zip` - Package source code
  - `aws_iam_policy_document.policy` - Assume role policy
  - `aws_iam_policy_document.lambda_log_and_invoke_policy` - Lambda permissions

#### variables.tf
- **Path**: `variables.tf`
- **Purpose**: Input variables
- **Variables**:
  - `aws_region` - AWS region (default: ap-southeast-1)

#### outputs.tf
- **Path**: `outputs.tf`
- **Purpose**: Output values
- **Outputs**:
  - `lambda` - Lambda function qualified ARN

### Application Components (Python)

#### hello_lambda.py
- **Path**: `src/hello_lambda.py`
- **Purpose**: Main Lambda handler
- **Dependencies**: boto3, json, os, typing
- **Key Functions**:
  - `lambda_handler(event, context)` - Entry point
  - `invokeLambdaFunction(functionName, payload)` - Helper to invoke other Lambdas
- **Environment Variables Used**:
  - `ACCOUNT_ID` - AWS account ID
  - `REGION` - AWS region

#### mock_common.py
- **Path**: `src/common/mock_common.py`
- **Purpose**: Placeholder for shared utilities
- **Status**: Empty file

## External Integrations

| Service | Type | Purpose | Config Location |
|---------|------|---------|-----------------|
| AWS Lambda | AWS Service | Invoke external Lambda function | `src/hello_lambda.py` |
| CloudWatch Logs | AWS Service | Function execution logging | `main.tf` (IAM policy) |
| AWS IAM | AWS Service | Authentication & authorization | `main.tf` |
| AWS STS | AWS Service | Assume role | `main.tf` (policy document) |

## Cross-Cutting Concerns

### Authentication & Authorization
- **Method**: IAM Role-based
- **Role**: `iam_for_lambda`
- **Permissions**:
  - CloudWatch Logs: CreateLogGroup, CreateLogStream, PutLogEvents
  - Lambda: InvokeFunction (all functions in account)

### Logging
- **Service**: AWS CloudWatch Logs
- **Log Group**: Auto-created by Lambda
- **Retention**: Default (never expires)

### Configuration Management
- **Terraform Variables**: `variables.tf`
- **Lambda Environment Variables**: Injected via Terraform
  - `ACCOUNT_ID`
  - `REGION`
  - `greeting`

### Build & Packaging
- **Method**: Terraform archive_file data source
- **Source**: `src/` directory
- **Output**: `build/lightweight_package.zip`
