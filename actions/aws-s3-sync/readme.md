# ☁️ Sync Directory to S3
Upload a local directory to an Amazon S3 bucket.

## ✅ Features
- Optional deletion of objects missing in source (keep destination clean)
- Exclude files via space‑separated patterns (e.g., ".git/* *.tmp")
- Optional Cache-Control header applied to uploaded objects
- Automatic content-type detection (can be disabled)
- Summary with counts (uploaded/deleted) and total size

## 📖 Related Documentation
- AWS CLI S3 sync: https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html
- S3 bucket naming rules: https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html
- GitHub OIDC for AWS: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html

## 🚀 Prerequisites
Your workflow must:
- Run on `ubuntu-latest`
- Have access to AWS credentials or an assumable IAM role
- Ensure the target S3 bucket already exists and is accessible

## 🔧 Quick Example
```yaml
name: Sync Web Assets to S3

on:
  push:
    branches: [main]

jobs:
  s3-sync:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Sync dist/ to S3
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

```

## 📥 Inputs
|**Name**|**Required**|**Description**|**Default**|
|---|---|---|---|
|`aws_region`|✅ Yes|AWS region|-|
|`bucket_name`|✅ Yes|Target S3 bucket name|-|
|`source_dir`|✅ Yes|Local path to sync|-|
|`aws_access_key`|❌ No|AWS access key ID (optional if using OIDC)|-|
|`aws_secret_key`|❌ No|AWS secret access key (optional if using OIDC)|-|
|`role_to_assume`|❌ No|AWS IAM role ARN to assume (OIDC)|-|
|`bucket_prefix`|❌ No|Optional subpath prefix inside the bucket (trimmed of leading/trailing slashes)|`""`|
|`delete_removed`|❌ No|Remove objects in S3 that are not present in `source_dir` (`true`/`false`)|`true`|
|`exclude_patterns`|❌ No|Space‑separated exclude patterns passed to `aws s3 sync --exclude`|`.git/* .github/* .gitignore .gitattributes`|
|`cache_control`|❌ No|Value for `Cache-Control` header applied to uploads|-|
|`content_type_detection`|❌ No|Enable automatic content-type guessing based on file extension (true/false)|true|
|`show_summary`| ❌ No|Print summary with task output in job summary|`true`|
|`summary_limit`|❌ No|Max number of output lines to show in summary|`250`|

## 📤 Outputs
|**Name**|**Description**|
|---|---|
|`files_uploaded`|Number of uploaded files|
|`files_deleted`|Number of deleted files|
|`total_size`|Total size in bytes of local files synced|
|`file_count`|Total number of local files considered for sync|
|`sync_duration`|Sync duration in seconds|
|`s3_url`|Final S3 sync url|

## 📋 Examples
[View example →](./examples/base.yml)

