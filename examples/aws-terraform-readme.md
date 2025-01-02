# AWS Terraform CI/CD
Welcome to the AWS Terraform CI/CD documentation!

This guide provides a comprehensive overview of setting up Continuous Integration and Continuous Deployment (CI/CD) workflows for deploy AWS infra via Terraform.

## Workflow Overview

The workflow defined in [aws-terraform.yml](https://github.com/Mad-Pixels/github-workflows/blob/main/.github/workflows/aws-terraform.yml) simplifies and secures Terraform operations using the [ci-actions](https://github.com/Mad-Pixels/ci-actions) project. This project acts as a wrapper for executing Terraform commands while masking sensitive information, ensuring the safety and confidentiality of secrets and variables.

This workflow supports common Terraform operations such as plan and apply, with configurable parameters and secure integration with AWS.

### Workflow configuration

#### Inputs
| Input Name            | Required | Default     | Description                                                 |
|-----------------------|----------|-------------|-------------------------------------------------------------|
| `terraform_version`   | 🔴       | `1.6.1`     | The Terraform version to be used in the workflow.           |
| `terraform_command`   | 🟢       | -           | The Terraform command to execute (e.g., `plan`, `apply`).   |
| `terraform_workspace` | 🔴       | -           | The Terraform workspace to use.                             |
| `working_directory`   | 🔴       | `terraform` | The directory containing the Terraform configuration files. |
| `actions_verions`     | 🟢       | -           | The version of the CI-action Docker image to use.           |

#### Secrets
| Input Name                     | Required | Description                                                        |
|--------------------------------|----------|--------------------------------------------------------------------|
| `AWS_ACCESS_KEY_ID`            | 🟢       | AWS access key ID for authentication.                              |
| `AWS_SECRET_ACCESS_KEY`        | 🟢       | AWS secret access key for authentication.                          |
| `AWS_SESSION_TOKEN`            | 🔴       | AWS session token (if temporary credentials are used).             |
| `TF_BACKEND_JSON`              | 🔴       | A JSON object containing Terraform backend variables               | 
| `TF_VARS_JSON`                 | 🔴       | A JSON object containing variables for Terraform in TF_VAR_ format.|

## Environment Variables

### Overview
GitHub Actions has a built-in masking mechanism that hides entire lines containing any secret value. This can make logs unreadable when multiple individual secrets are used. To avoid this, we use a single JSON-formatted secret for each configuration group, allowing for more precise control over sensitive data masking.

### TF_VARS_JSON
This secret manages all Terraform variables in a single JSON object, preventing GitHub's aggressive masking behavior. Instead of defining multiple individual secrets (which would trigger masking for any line containing them), use this consolidated approach.

Valid formats:

1. With `TF_VAR_` prefix (explicit):
```json
{
  "TF_VAR_project": "my_project",
  "TF_VAR_aws_region": "us-east-1",
  "TF_VAR_domain": "example.com"
}
```
2. Without prefix (prefix will be added automatically):
```json
{
  "project": "my_project",
  "aws_region": "us-east-1",
  "domain": "example.com"
}
```

### TF_BACKEND_JSON
Similar to TF_VARS_JSON, this secret manages Terraform backend configuration in a single JSON object. This approach provides better log readability while maintaining security.

Valid formats:

1. With BACKEND_ prefix (explicit):
```json
{
  "BACKEND_region": "us-east-1",
  "BACKEND_bucket": "my-terraform-state",
  "BACKEND_key": "terraform.tfstate"
}
```

2. Without prefix (prefix will be added automatically):
```json
{
  "region": "us-east-1",
  "bucket": "my-terraform-state",
  "key": "terraform.tfstate"
}
```

### Benefits of Using JSON Secrets
- Better Log Readability: Instead of GitHub masking entire lines containing any secret value, only the specific sensitive values are masked
- Simplified Management: Manage related variables as a single unit instead of multiple individual secrets
Consistent Masking: Ensures sensitive data is properly masked without affecting surrounding context
- Flexible Format: Supports both prefixed and non-prefixed variable names
- Reduced Configuration: Minimizes the number of GitHub secrets needed for your workflow

## Using Workflow example

1. Prepare project secrets
2. Prepare `TF_VAR_*` as single secret in JSON format.

```yml
name: Release

on:
  push:
    tags:
      - "v[0-9]+.[0-9]+.[0-9]+"

jobs:
  deploy:
    uses: Mad-Pixels/github-workflows/.github/workflows/aws-terraform.yml@main
    with:
      terraform_command: apply
      working_directory: "terraform/provisioners/infra/"
    secrets:
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_KEY }}

      TF_VARS_JSON: ${{ secrets.TF_VARS_JSON }}
      TF_BACKEND_JSON: ${{ secrets.TF_BACKEND_JSON }}
```

#### Result

> With additional maskers for AWS output.

![Workflow](../media/aws-terraform.svg)
