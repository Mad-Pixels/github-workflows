# 🚀 Lambda Restart
Update an AWS Lambda function to a new container image (ECR). 

## ✅ Features
- Update Lambda to a specific container image (ECR)
- Accept either full `image_uri` or `repository` + `image_tag`
- Validates Lambda existence before update
- Optional wait until function update completes

## 📖 Related Documentation
- AWS Lambda container images: https://docs.aws.amazon.com/lambda/latest/dg/images-create.html
- Update function code (CLI): https://docs.aws.amazon.com/cli/latest/reference/lambda/update-function-code.html
- ECR repositories and images: https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html
- GitHub OIDC for AWS: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html

## 🚀 Prerequisites
Your workflow must:
- Run on `ubuntu-latest`
- Have access to AWS credentials or an assumable IAM role
- Ensure the target ECR image exists and the Lambda function is configured for images

## 🔧 Quick Example
```yaml
name: Restart Lambda (image_uri)

on:
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Update Lambda to specific image URI
        uses: Mad-Pixels/github-workflows/actions/lambda-restart@v1
        with:
          aws_region: us-east-1
          role_to_assume: arn:aws:iam::123456789012:role/GHA-OIDC
          function_name: my-service-prod
          image_uri: 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-service@sha256:deadbeef
          wait_for_update: 'true'
```

## 📥 Inputs
| **Name**                 | **Required** | **Description**                                                                      | **Default** |
|--------------------------|--------------|--------------------------------------------------------------------------------------|-------------|
| `function_name`          | ✅ Yes       | Full Lambda function name                                                            | -           |
| `aws_region`             | ✅ Yes       | AWS region                                                                           | -           |
| `aws_account_id`         | ⚠️ Cond.     | AWS account ID (12 digits) — required only when using `repository` + `image_tag`     | -           |
| `aws_access_key_id`      | ❌ No        | AWS access key ID (optional if using OIDC)                                           | -           |
| `aws_secret_access_key`  | ❌ No        | AWS secret access key (optional if using OIDC)                                       | -           |
| `role_to_assume`         | ❌ No        | AWS IAM role ARN to assume (for OIDC authentication)                                 | -           |
| `image_uri`              | ❌ No        | Full ECR image URI (overrides `repository`/`image_tag` when provided)                | -           |
| `repository`             | ❌ No        | ECR repository name (used if `image_uri` not provided)                               | -           |
| `image_tag`              | ❌ No        | ECR image tag (used with `repository`)                                               | `latest`    |
| `wait_for_update`        | ❌ No        | Wait for function update to complete (`true`/`false`)                                | `true`      |
| `show_summary`           | ❌ No        | Print summary with task output in job summary                                        | `true`      |
| `summary_limit`          | ❌ No        | Max number of output lines to show in summary                                        | `250`       |

## 📤 Outputs
| **Name**         | **Description**                         |
|------------------|-----------------------------------------|
| `function_arn`   | Lambda function ARN                     |
| `last_modified`  | Function last modified timestamp        |
| `code_sha256`    | Lambda code SHA256                      |
| `imgae_url`      | Resolved image URI                      |

## 📋 Examples
[View example →](./examples/base.yml)

