# Infrastructure Analysis: AWS Terraform Lambda Python

## Cloud Overview
- **Provider**: AWS (Amazon Web Services)
- **Region**: ap-southeast-1 (Singapore) - configurable via variable
- **IaC Tool**: Terraform
- **Environment**: Single environment (no staging/prod separation)

## Terraform Configuration

### Providers
| Provider | Purpose | Version |
|----------|---------|---------|
| aws | AWS resource management | Default (latest) |
| archive | Zip file creation | Default (latest) |

### Variables
| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| aws_region | string | ap-southeast-1 | AWS deployment region |

### Outputs
| Output | Value | Description |
|--------|-------|-------------|
| lambda | aws_lambda_function.lambda.qualified_arn | Lambda function ARN with version |

## Compute Resources

### AWS Lambda Function: terraform_python
| Property | Value |
|----------|-------|
| **Function Name** | terraform_python |
| **Runtime** | python3.9 |
| **Handler** | hello_lambda.lambda_handler |
| **Timeout** | 90 seconds |
| **Memory** | 128 MB (default) |
| **Source** | build/lightweight_package.zip |
| **Role** | iam_for_lambda |

**Environment Variables**:
| Variable | Source | Value |
|----------|--------|-------|
| ACCOUNT_ID | aws_caller_identity | Dynamic (AWS account ID) |
| REGION | var.aws_region | ap-southeast-1 |
| greeting | Hardcoded | "Hello" |

## Security

### IAM Role: iam_for_lambda
| Property | Value |
|----------|-------|
| **Role Name** | iam_for_lambda |
| **Trust Policy** | Allow Lambda service to assume role |
| **Trusted Entity** | lambda.amazonaws.com |

### IAM Policy: frontend-lambda-role-policy
| Statement | Effect | Actions | Resources |
|-----------|--------|---------|-----------|
| CloudWatch Logs | Allow | logs:CreateLogGroup, logs:CreateLogStream, logs:PutLogEvents | * |
| Lambda Invoke | Allow | lambda:InvokeFunction | arn:aws:lambda:{region}:{account}:function:* |

**Trust Policy Document**:
```json
{
  "Effect": "Allow",
  "Principal": {
    "Service": "lambda.amazonaws.com"
  },
  "Action": "sts:AssumeRole"
}
```

## Build & Deployment

### Source Packaging
| Property | Value |
|----------|-------|
| **Source Directory** | src/ |
| **Output Path** | build/lightweight_package.zip |
| **Archive Type** | zip |
| **Method** | Terraform archive_file data source |

### Deployment Pipeline (Manual)
```
1. Developer modifies code in src/
2. Run: terraform init (first time only)
3. Run: terraform plan (preview changes)
4. Run: terraform apply (deploy to AWS)
5. Terraform zips src/ automatically
6. Lambda function updated with new code
```

## Resource Dependencies

### Dependency Graph
```
variables.tf (aws_region)
       │
       ▼
aws_caller_identity.current ──────────────┐
       │                                   │
       ▼                                   │
archive_file.lambda_source_zip            │
       │                                   │
       ▼                                   │
aws_iam_policy_document.policy            │
       │                                   │
       ▼                                   │
aws_iam_role.iam_for_lambda               │
       │                                   │
       ▼                                   │
aws_iam_policy_document.lambda_log_and_invoke_policy
       │                                   │
       ▼                                   │
aws_iam_role_policy.frontend_lambda_role_policy
       │                                   │
       ▼                                   ▼
aws_lambda_function.lambda ◀──────────────┘
       │
       ▼
outputs.tf (lambda ARN)
```

## Integration Services

### External Lambda Function
| Property | Value |
|----------|-------|
| **Function Name** | demo-python |
| **ARN Pattern** | arn:aws:lambda:{region}:{account}:function:demo-python |
| **Invocation Type** | RequestResponse (synchronous) |
| **Status** | External dependency (not managed by this Terraform) |

### CloudWatch Logs
| Property | Value |
|----------|-------|
| **Log Group** | /aws/lambda/terraform_python (auto-created) |
| **Retention** | Default (never expires) |
| **Managed By** | AWS Lambda service |

## Monitoring

### Available Metrics (CloudWatch)
| Metric | Description |
|--------|-------------|
| Invocations | Number of function invocations |
| Duration | Execution time in milliseconds |
| Errors | Number of failed invocations |
| Throttles | Number of throttled invocations |
| ConcurrentExecutions | Number of concurrent executions |

### Logging
- All `print()` statements → CloudWatch Logs
- Lambda response logged: `print(f'response:{response}')`

## Cost Considerations
| Resource | Pricing Model |
|----------|---------------|
| Lambda | Pay per request + compute time |
| CloudWatch Logs | Pay per GB ingested + stored |
| IAM | Free |

## Terraform State
| Property | Value |
|----------|-------|
| **Backend** | Local (default) |
| **State File** | terraform.tfstate |
| **Lock File** | terraform.tfstate.lock |
| **Ignored** | .terraform/, *.tfstate* (via .gitignore) |
