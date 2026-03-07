# Sequence Diagram Analysis: AWS Terraform Lambda Python

## Overview
- **Project**: fork-lesson-aws-terraform-lambda-python
- **Analyzed**: 2024
- **Total Sequences**: 2

## Lambda Invocation Sequences

### Sequence: Lambda Handler Execution
**Trigger**: External invocation of terraform_python function
**Participants**: Caller, Lambda Runtime, hello_lambda, boto3 Client, AWS Lambda Service, demo-python

| # | From | To | Message | Return | Notes |
|---|------|-----|---------|--------|-------|
| 1 | Caller | Lambda Runtime | Invoke terraform_python | - | Event payload (any) |
| 2 | Lambda Runtime | hello_lambda | lambda_handler(event, context) | - | Entry point |
| 3 | hello_lambda | hello_lambda | Read os.environ['ACCOUNT_ID'] | account_id | Environment variable |
| 4 | hello_lambda | hello_lambda | Read os.environ['REGION'] | region | Environment variable |
| 5 | hello_lambda | hello_lambda | Create payload dict | {"something": "Bad News!"} | Hardcoded |
| 6 | hello_lambda | hello_lambda | Build function ARN | arn:aws:lambda:... | String format |
| 7 | hello_lambda | invokeLambdaFunction | invokeLambdaFunction(functionName, payload) | - | Helper call |
| 8 | invokeLambdaFunction | invokeLambdaFunction | Validate functionName != None | - | Guard clause |
| 9 | invokeLambdaFunction | boto3 Client | client.invoke(...) | response | AWS SDK call |
| 10 | boto3 Client | AWS Lambda Service | InvokeFunction API | - | HTTP request |
| 11 | AWS Lambda Service | demo-python | Execute function | result | Sync invocation |
| 12 | demo-python | AWS Lambda Service | - | response | Function result |
| 13 | AWS Lambda Service | boto3 Client | - | response object | API response |
| 14 | boto3 Client | invokeLambdaFunction | - | response | SDK response |
| 15 | invokeLambdaFunction | hello_lambda | - | response | Return value |
| 16 | hello_lambda | hello_lambda | print(f'response:{response}') | - | Log to CloudWatch |
| 17 | hello_lambda | Lambda Runtime | - | f'response:{response}' | Return string |
| 18 | Lambda Runtime | Caller | - | response | Final result |

**Alternative Flow - Missing Function Name**:
| # | From | To | Message | Return |
|---|------|-----|---------|--------|
| 8a | invokeLambdaFunction | invokeLambdaFunction | functionName is None | - |
| 9a | invokeLambdaFunction | hello_lambda | raise Exception | ERROR: functionName parameter cannot be NULL |

---

### Sequence: Terraform Deployment
**Trigger**: Developer runs terraform apply
**Participants**: Developer, Terraform CLI, AWS Provider, Archive Provider, AWS IAM, AWS Lambda

| # | From | To | Message | Return | Notes |
|---|------|-----|---------|--------|-------|
| 1 | Developer | Terraform CLI | terraform init | - | Initialize |
| 2 | Terraform CLI | AWS Provider | Initialize | ready | Download plugin |
| 3 | Terraform CLI | Archive Provider | Initialize | ready | Download plugin |
| 4 | Developer | Terraform CLI | terraform apply | - | Deploy |
| 5 | Terraform CLI | AWS Provider | GetCallerIdentity | account_id | Data source |
| 6 | Terraform CLI | Archive Provider | CreateZip(src/) | .zip file | Package code |
| 7 | Terraform CLI | AWS IAM | CreateRole(iam_for_lambda) | role_arn | IAM role |
| 8 | Terraform CLI | AWS IAM | PutRolePolicy(logs+invoke) | - | Attach policy |
| 9 | Terraform CLI | AWS Lambda | CreateFunction(terraform_python) | function_arn | Create Lambda |
| 10 | AWS Lambda | Terraform CLI | - | qualified_arn | Output |
| 11 | Terraform CLI | Developer | - | Apply complete | Success |

---

## Message Patterns

### Synchronous Calls
- Lambda client.invoke with InvocationType="RequestResponse"
- Caller waits for response from demo-python
- Used for: Lambda-to-Lambda invocation

### Environment Variable Access
- os.environ dictionary access
- Synchronous, in-memory lookup
- Used for: ACCOUNT_ID, REGION configuration

## Payload Definitions

### Lambda Invoke Payload
```json
{
  "something": "Bad News!"
}
```

### client.invoke() Parameters
```python
{
  "FunctionName": "arn:aws:lambda:{region}:{account}:function:demo-python",
  "InvocationType": "RequestResponse",
  "Payload": "{\"something\": \"Bad News!\"}"
}
```

### Response Structure
```python
{
  'StatusCode': 200,
  'FunctionError': None,
  'LogResult': '...',
  'Payload': StreamingBody(...),
  'ExecutedVersion': '$LATEST'
}
```
