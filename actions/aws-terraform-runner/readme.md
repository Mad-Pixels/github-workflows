# 🌍 Terraform Runner  
Run Terraform commands in GitHub Actions with built-in AWS authentication, backend configuration, and workspace management.

## ✅ Features
- Supports `plan`, `apply`, and `destroy` commands
- AWS authentication via static credentials or OIDC role assumption
- Automatic backend configuration for S3 remote state
- Optional workspace selection and creation
- Built-in validation before running commands
- Execution summary in GitHub Actions summary
- Optionally stores Terraform plan output as an artifact

## 📖 Related Documentation
- Terraform CLI: https://developer.hashicorp.com/terraform/cli
- Terraform Backends (S3): https://developer.hashicorp.com/terraform/language/settings/backends/s3
- GitHub Actions for Terraform: https://github.com/hashicorp/setup-terraform
- AWS Authentication for GitHub Actions: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html

## 🚀 Prerequisites
Your workflow must:
- Run on `ubuntu-latest`
- Have access to AWS credentials or an assumable IAM role
- Ensure `tf_dir` contains a valid Terraform configuration

## 🔧 Quick Example
```yaml
name: Terraform Plan

on:
  workflow_dispatch:

jobs:
  terraform:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Terraform Plan
        uses: Mad-Pixels/github-workflows/actions/terraform-runner@v1
        with:
          aws_region: us-east-1
          role_to_assume: arn:aws:iam::123456789012:role/TerraformRole
          tf_dir: infra/
          tf_workspace: staging
          tf_command: plan
          backend_bucket: my-terraform-state
          backend_key: staging/terraform.tfstate
          backend_region: us-east-1
```

## 📥 Inputs
| **Name**                | **Required** | **Description**                                                                 | **Default**  |
|-------------------------|--------------|---------------------------------------------------------------------------------|--------------|
| `aws_access_key_id`     | ❌ No        | AWS access key ID (optional if using OIDC)                                      | -            |
| `aws_secret_access_key` | ❌ No        | AWS secret access key (optional if using OIDC)                                  | -            |
| `aws_region`            | ✅ Yes       | AWS region                                                                      | -            |
| `role_to_assume`        | ❌ No        | AWS IAM role ARN for OIDC authentication                                        | -            |
| `tf_dir`                | ✅ Yes       | Path to Terraform configuration directory                                       | -            |
| `tf_workspace`          | ❌ No        | Terraform workspace name                                                        | `""`         |
| `tf_command`            | ✅ Yes       | Terraform command: `plan`, `apply`, or `destroy`                                | -            |
| `tf_vars`               | ❌ No        | Extra CLI `-var` flags                                                           | `""`         |
| `tf_version`            | ❌ No        | Terraform version                                                               | `1.8.5`      |
| `backend_bucket`        | ✅ Yes       | S3 bucket for storing Terraform state                                           | -            |
| `backend_key`           | ✅ Yes       | S3 key (path) for Terraform state                                               | -            |
| `backend_region`        | ✅ Yes       | AWS region for S3 backend                                                        | -            |

## 📤 Outputs
| **Name**         | **Description**                                  |
|------------------|--------------------------------------------------|
| `plan_artifact`  | Name of the uploaded Terraform plan artifact     |

## 📋 Examples
[View example →](./examples/base.yml)