# 🚀 Lambda Restart
Update an AWS Lambda function to a new container image (ECR). Supports direct `image_uri` or `repository` + `image_tag`, OIDC or static AWS credentials, and optional wait for completion.

## ✅ Features
- Update Lambda to a specific container image (ECR)
- Accept either full `image_uri` or `repository` + `image_tag`
- AWS auth via OIDC role assumption or static credentials
- Validates Lambda existence before update
- Optional wait until function update completes
- Summary output with key details

## 📖 Related Documentation
- AWS Lambda container images: https://docs.aws.amazon.com/lambda/latest/dg/images-create.html
- Update function code (CLI): https://docs.aws.amazon.com/cli/latest/reference/lambda/update-function-code.html
- ECR repositories and images: https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html
- GitHub OIDC for AWS: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html

## 🚀 Prerequisites
Your workflow must:
- Run on `ubuntu-latest` with AWS CLI and `jq` available
- Provide AWS credentials or an assumable IAM role
- Ensure the target ECR image exists and the Lambda function is configured for images

## 🔧 Quick Example
```yaml
name: Restart Lambda

on:
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Update Lambda to specific image URI (OIDC)
        uses: Mad-Pixels/github-workflows/actions/lambda-restart@v1
        with:
          aws_region: us-east-1
          aws_account_id: 123456789012
          role_to_assume: arn:aws:iam::123456789012:role/GHA-OIDC
          function_name: my-service-prod
          image_uri: 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-service@sha256:deadbeef
          wait_for_update: 'true'

      # Alternative: use repository + tag instead of full image_uri
      # - name: Update Lambda with repository/tag
      #   uses: Mad-Pixels/github-workflows/actions/lambda-restart@v1
      #   with:
      #     aws_region: us-east-1
      #     aws_account_id: 123456789012
      #     role_to_assume: arn:aws:iam::123456789012:role/GHA-OIDC
      #     function_name: my-service-prod
      #     repository: my-service
      #     image_tag: latest
      #     wait_for_update: 'true'
```

## 📥 Inputs
| **Name**                 | **Required** | **Description**                                                                      | **Default** |
|--------------------------|--------------|--------------------------------------------------------------------------------------|-------------|
| `aws_access_key_id`      | ❌ No        | AWS access key ID (optional if using OIDC)                                           | -           |
| `aws_secret_access_key`  | ❌ No        | AWS secret access key (optional if using OIDC)                                       | -           |
| `aws_region`             | ✅ Yes       | AWS region                                                                           | -           |
| `aws_account_id`         | ✅ Yes       | AWS account ID (12 digits)                                                           | -           |
| `role_to_assume`         | ❌ No        | AWS IAM role ARN to assume (for OIDC authentication)                                 | -           |
| `function_name`          | ✅ Yes       | Full Lambda function name                                                            | -           |
| `image_uri`              | ❌ No        | Full ECR image URI (overrides `repository`/`image_tag` when provided)                | -           |
| `repository`             | ❌ No        | ECR repository name (used if `image_uri` not provided)                               | -           |
| `image_tag`              | ❌ No        | ECR image tag (used with `repository`)                                               | `latest`    |
| `wait_for_update`        | ❌ No        | Wait for function update to complete (`true`/`false`)                                 | `true`      |

## 📤 Outputs
| **Name**         | **Description**                         |
|------------------|-----------------------------------------|
| `function_arn`   | Lambda function ARN                     |
| `last_modified`  | Function last modified timestamp        |
| `code_sha256`    | Lambda code SHA256                      |

## 📋 Examples
[View example →](./examples/base.yml)