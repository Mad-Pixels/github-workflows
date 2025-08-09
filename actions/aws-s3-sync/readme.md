# ☁️ Sync Directory to S3
Upload a local directory to an Amazon S3 bucket with optional key prefix, deletion of removed files, exclude patterns, and cache headers.

## ✅ Features
- Sync any local directory to S3 with optional prefix subpath
- Optional deletion of objects missing in source (keep destination clean)
- Exclude files via space‑separated patterns (e.g., ".git/* *.tmp")
- Optional Cache-Control header applied to uploaded objects
- Works with AWS OIDC role assumption or static credentials
- Summary with counts (uploaded/deleted) and total size

## 📖 Related Documentation
- AWS CLI S3 sync: https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html
- S3 bucket naming rules: https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html
- GitHub OIDC for AWS: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html

## 🚀 Prerequisites
Your workflow must:
- Run on `ubuntu-latest` with AWS CLI available
- Provide AWS credentials (static keys) or OIDC role assumption
- Ensure the target S3 bucket already exists and is accessible

## 🔧 Quick Example
name: Sync Web Assets to S3

on:
  push:
    branches: [main]

jobs:
  s3-sync:
    runs-on: ubuntu-latest
    permissions:
      id-token: write      # required only if using role_to_assume (OIDC)
      contents: read
    steps:
      - name: Sync dist/ to S3 (OIDC)
        uses: Mad-Pixels/github-workflows/actions/s3-sync@v1
        with:
          aws_region: us-east-1
          role_to_assume: arn:aws:iam::123456789012:role/GHA-OIDC-Deploy
          bucket_name: my-static-site-bucket
          source_dir: dist
          bucket_prefix: web
          delete_removed: 'true'
          exclude_patterns: ".git/* .github/* .DS_Store"
          cache_control: "public, max-age=31536000, immutable"

## 📥 Inputs
| **Name**           | **Required** | **Description**                                                                                  | **Default**                                |
|--------------------|--------------|--------------------------------------------------------------------------------------------------|--------------------------------------------|
| `aws_access_key`   | ❌ No        | AWS access key ID (optional if using OIDC)                                                       | -                                          |
| `aws_secret_key`   | ❌ No        | AWS secret access key (optional if using OIDC)                                                   | -                                          |
| `role_to_assume`   | ❌ No        | AWS IAM role ARN to assume (OIDC)                                                                | -                                          |
| `aws_region`       | ✅ Yes       | AWS region                                                                                        | -                                          |
| `bucket_name`      | ✅ Yes       | Target S3 bucket name                                                                             | -                                          |
| `source_dir`       | ✅ Yes       | Local path to sync                                                                                | -                                          |
| `bucket_prefix`    | ❌ No        | Optional subpath prefix inside the bucket (trimmed of leading/trailing slashes)                  | `""`                                       |
| `delete_removed`   | ❌ No        | Remove objects in S3 that are not present in `source_dir` (`true`/`false`)                       | `true`                                     |
| `exclude_patterns` | ❌ No        | Space‑separated exclude patterns passed to `aws s3 sync --exclude`                               | `.git/* .github/* .gitignore .gitattributes` |
| `cache_control`    | ❌ No        | Value for `Cache-Control` header applied to uploads                                              | -                                          |

## 📤 Outputs
| **Name**          | **Description**                          |
|-------------------|------------------------------------------|
| `files_uploaded`  | Number of uploaded files                 |
| `files_deleted`   | Number of deleted files                  |
| `total_size`      | Total size in bytes of local files synced |
| `s3_url`          | Final S3 sync URL (e.g., `s3://bucket/prefix`) |

## 📋 Examples
[View example →](./examples/base.yml)
