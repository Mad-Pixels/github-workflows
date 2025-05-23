# 🧬 Terraform Format Check · GitHub Composite Action

This composite GitHub Action checks Terraform code formatting using terraform fmt -check -diff -recursive.

## ✅ Features
- Validates Terraform formatting recursively
- Uses the specified Terraform version
- Can be reused across multiple repositories or workflows

## 🔧 Usage Example
```yaml
name: Check Terraform Formatting

on:
  pull_request:
    paths:
      - '**.tf'

jobs:
  fmt:
    uses: "Mad-Pixels/github-workflows/.github/actions/terraform-fmt@main"
    with:
      tf_dir: "infra"
      tf_version: "1.6.1"
```

## 📥 Inputs
| **Name**                | **Required** | **Description**                    |
|-------------------------|--------------|------------------------------------|
| `tf_dir`                | ❌ No        | Path to Terraform directory        |
| `tf_version`            | ❌ No        | Terraform version (default: 1.6.1) |