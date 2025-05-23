# 🧬 ECR Push · GitHub Composite Action

This composite GitHub Action authenticates to AWS ECR and pushes a local Docker image to the specified ECR repository.

## ✅ Features
- Authenticates with AWS ECR using provided credentials
- Supports automatic tagging and pushing of Docker images
- Simple and reusable for deployment workflows

## 🔧 Usage Example
```yaml
name: Push Docker Image to ECR

on:
  push:
    branches: [main]

jobs:
  push-ecr:
    runs-on: ubuntu-latest
    steps:
      - name: Build Docker image
        run: docker build -t my-service:latest .

      - name: Push to ECR
        uses: "Mad-Pixels/github-workflows/.github/actions/aws-ecr-push@main"
        with:
          aws_access_key_id:      ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws_secret_access_key:  ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws_region:             "us-east-1"
          aws_account_id:         "123456789012"
          image:                  "my-service:latest"
```

## 📥 Inputs
| **Name**                | **Required** | **Description**                                   |
|-------------------------|--------------|---------------------------------------------------|
| `aws_access_key_id`     | ✅ Yes       | AWS access key ID                                 |
| `aws_secret_access_key` | ✅ Yes       | AWS secret access key                             |
| `aws_region`            | ✅ Yes       | AWS region (e.g. us-east-1)                       |
| `aws_account_id`        | ✅ Yes       | AWS Account ID (used to construct ECR image URI)  |
| `image`                 | ✅ Yes       | Local image name with tag (e.g. my-repo:latest)   |