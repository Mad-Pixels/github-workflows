# 🧬 Terraform Runner · GitHub Composite Action

This GitHub composite action provides a standardized way to run Terraform commands (plan, apply, destroy) with S3 remote backend, AWS credentials, and optional workspace support.

✅ Features
- Built-in AWS credentials support
- Remote backend configuration with backend_aws.hcl
- Workspace selection and creation
- Supports additional -var flags
- Terraform version is configurable

## 🔧 Usage Example
```yaml
name: Deploy Infra

on:
  push:
    branches: [main]

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - uses: "Mad-Pixels/github-workflows/.github/actions/terraform-fmt@main"
        with:
          aws_access_key_id:      ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws_secret_access_key:  ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws_region:             "us-east-1"

          tf_dir:                 "./infra"
          tf_workspace:           "prod"
          tf_command:             "apply"
          tf_vars:                "-var-file=prod.tfvars"
          tf_version:             "1.6.1"

          backend_bucket:         "your-terraform-state"
          backend_key:            "envs/prod/terraform.tfstate"
          backend_region:         "us-east-1"
```

## 📥 Inputs
| **Name**                | **Required** | **Description**                                   |
|-------------------------|--------------|---------------------------------------------------|
| `aws_access_key_id`     | ✅ Yes       | AWS access key ID                                 |
| `aws_secret_access_key` | ✅ Yes       | AWS secret access key                             |
| `aws_region`            | ✅ Yes       | AWS region (e.g. us-east-1)                       |
| `backend_bucket`        | ✅ Yes       | S3 bucket name used for remote state              |
| `backend_key`           | ✅ Yes       | S3 object key for the .tfstate file               |
| `backend_region`        | ✅ Yes       | Bucket AWS region (e.g. us-east-1)                |
| `tf_dir`                | ✅ Yes       | Path to Terraform directory                       |
| `tf_command`            | ✅ Yes       | Terraform command: plan, apply, or destroy        |
| `tf_workspace`          | ❌ No        | Terraform workspace to select/create              |
| `tf_vars`               | ❌ No        | Additional CLI flags (e.g. -var-file=prod.tfvars) |
| `tf_version`            | ❌ No        | Terraform version (default: 1.6.1)                |