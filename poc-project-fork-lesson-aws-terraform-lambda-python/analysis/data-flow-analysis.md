# Data Flow Analysis: AWS Terraform Lambda Python

## Overview
- **Project**: fork-lesson-aws-terraform-lambda-python
- **Analyzed**: 2024
- **Total Flows**: 2

## Lambda Invocation Flow

### Flow: Lambda-to-Lambda Invocation
**Trigger**: External invocation of `terraform_python` Lambda function
**Actors**: External Caller, terraform_python Lambda, demo-python Lambda

```
[External Caller]
   │ Invoke terraform_python
   │ (via AWS SDK, Console, or API Gateway)
   ▼
[terraform_python Lambda]
   │ 1. Read environment variables (ACCOUNT_ID, REGION)
   │ 2. Create payload: {"something": "Bad News!"}
   │ 3. Build target ARN: arn:aws:lambda:{region}:{account}:function:demo-python
   ▼
[invokeLambdaFunction()]
   │ 4. Call boto3 client.invoke()
   │    - FunctionName: demo-python ARN
   │    - InvocationType: RequestResponse (sync)
   │    - Payload: JSON serialized
   ▼
[AWS Lambda Service]
   │ 5. Route request to demo-python
   ▼
[demo-python Lambda]
   │ 6. Process payload
   │ 7. Return response
   ▼
[terraform_python Lambda]
   │ 8. Receive response
   │ 9. Print to CloudWatch Logs
   │ 10. Return formatted response
   ▼
[External Caller]
   │ Receive final response
   ▼
[End]
```

**Data Transformations**:
1. Input event → Not used (hardcoded payload)
2. Dict payload → JSON string (json.dumps)
3. Lambda response → Formatted string ("response:{response}")

**Payload Structure**:
```json
{
  "something": "Bad News!"
}
```

---

## Deployment Flow

### Flow: Terraform Infrastructure Deployment
**Trigger**: Developer runs `terraform apply`
**Actors**: Developer, Terraform, AWS

```
[Developer]
   │ terraform init
   ▼
[Terraform]
   │ 1. Download AWS provider
   │ 2. Download Archive provider
   │ 3. Initialize state
   ▼
[Developer]
   │ terraform apply
   ▼
[Terraform - Plan Phase]
   │ 4. Read variables.tf (aws_region)
   │ 5. Query aws_caller_identity (get account ID)
   │ 6. Archive src/ → build/lightweight_package.zip
   │ 7. Calculate resource changes
   ▼
[Terraform - Apply Phase]
   │ 8. Create IAM assume role policy document
   ▼
[AWS IAM]
   │ 9. Create iam_for_lambda role
   ▼
[Terraform]
   │ 10. Create IAM policy document (logs + invoke)
   ▼
[AWS IAM]
   │ 11. Attach policy to role
   ▼
[Terraform]
   │ 12. Upload lambda zip to AWS
   ▼
[AWS Lambda]
   │ 13. Create terraform_python function
   │ 14. Configure handler, runtime, timeout
   │ 15. Set environment variables
   ▼
[Terraform]
   │ 16. Output lambda ARN
   ▼
[Developer]
   │ Deployment complete
   ▼
[End]
```

**Data Transformations**:
1. `src/*.py` → `build/lightweight_package.zip` (archive_file)
2. Terraform variables → AWS resource configurations
3. AWS caller identity → Environment variables (ACCOUNT_ID, REGION)

---

## Environment Variable Flow

### Flow: Configuration Injection
**Source**: Terraform → Lambda Runtime

| Variable | Source | Used In | Purpose |
|----------|--------|---------|---------|
| ACCOUNT_ID | aws_caller_identity.current.account_id | hello_lambda.py | Build target Lambda ARN |
| REGION | var.aws_region | hello_lambda.py | Build target Lambda ARN |
| greeting | Hardcoded "Hello" | (unused) | Example env var |

```
[variables.tf]
   │ aws_region = "ap-southeast-1"
   ▼
[main.tf]
   │ data.aws_caller_identity.current.account_id
   │ var.aws_region
   ▼
[aws_lambda_function.lambda.environment]
   │ ACCOUNT_ID, REGION, greeting
   ▼
[Lambda Runtime]
   │ os.environ['ACCOUNT_ID']
   │ os.environ['REGION']
   ▼
[hello_lambda.py]
   │ Build ARN: arn:aws:lambda:{region}:{account}:function:demo-python
   ▼
[End]
```

---

## Logging Flow

### Flow: CloudWatch Logging
**Trigger**: Lambda execution

```
[Lambda Execution]
   │ print(f'response:{response}')
   ▼
[Lambda Runtime]
   │ stdout → CloudWatch Logs agent
   ▼
[CloudWatch Logs]
   │ /aws/lambda/terraform_python
   │ Log stream: {date}/{instance-id}
   ▼
[CloudWatch Console / CLI]
   │ View logs
   ▼
[End]
```

**IAM Permissions Required**:
- `logs:CreateLogGroup`
- `logs:CreateLogStream`
- `logs:PutLogEvents`
