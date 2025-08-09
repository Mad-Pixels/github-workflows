# 🧬 Lambda Restart · GitHub Composite Action

This composite GitHub Action updates an AWS Lambda function with the latest container image from ECR.

## ✅ Features
- Uses AWS CLI to update Lambda with ECR image
- Automatically waits for function update to complete
- Supports per-function suffix/tag image mapping
- Simple and fast container-based Lambda deploys

## 🔧 Usage Example
```yaml
name: Restart Lambda Function

on:
  workflow_dispatch:

jobs:
  restart-lambda:
    runs-on: ubuntu-latest
    steps:
      - uses: "Mad-Pixels/github-workflows/.github/actions/aws-lambda-restart@main"
        with:
          aws_access_key_id:     ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws_secret_access_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws_region:            "us-east-1"
          aws_account_id:        "123456789012"
          repository:            "my-repo"
          function:              "my-func"
```

## 📥 Inputs
| **Name**                | **Required** | **Description**                                   |
|-------------------------|--------------|---------------------------------------------------|
| `aws_access_key_id`     | ✅ Yes       | AWS access key ID                                 |
| `aws_secret_access_key` | ✅ Yes       | AWS secret access key                             |
| `aws_region`            | ✅ Yes       | AWS region (e.g. us-east-1)                       |
| `aws_account_id`        | ✅ Yes       | AWS Account ID (used to construct ECR image URI)  |
| `repository`            | ✅ Yes       | Name of your ECR repository                       |
| `function`              | ✅ Yes       | Lambda function suffix                            |
