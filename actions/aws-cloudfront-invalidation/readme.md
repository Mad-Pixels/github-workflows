# ⚡️ CloudFront Invalidation
Create a CloudFront invalidation to purge cached content after deploys. Supports OIDC or static AWS credentials, multiple paths (space‑separated), optional custom caller reference, and rich summary output.

## ✅ Features
- Create invalidations for one or many paths (supports wildcards)
- Auto‑generated caller reference (or provide your own)
- OIDC role assumption or static AWS credentials
- Input validation (distribution ID format, path count, path prefixes)
- Summary output with invalidation ID, status, and paths

## 📖 Related Documentation
- CloudFront Invalidation API: https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html
- AWS CLI cloudfront create‑invalidation: https://docs.aws.amazon.com/cli/latest/reference/cloudfront/create-invalidation.html
- GitHub OIDC for AWS: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html

## 🚀 Prerequisites
Your workflow must:
- Run on `ubuntu-latest` with AWS CLI and `jq` available
- Provide AWS credentials (static keys) or OIDC role assumption
- Have a valid CloudFront distribution ID

## 🔧 Quick Example
```yaml
name: Invalidate CloudFront Cache

on:
  workflow_dispatch:

jobs:
  invalidate:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Create invalidation via OIDC
        uses: Mad-Pixels/github-workflows/actions/cloudfront-invalidation@v1
        with:
          aws_region: us-east-1
          role_to_assume: arn:aws:iam::123456789012:role/GHA-OIDC
          distribution_id: E1234567890ABC
          paths: "/* /index.html /assets/*"
```

## 📥 Inputs
| **Name**           | **Required** | **Description**                                                                                         | **Default** |
|--------------------|--------------|---------------------------------------------------------------------------------------------------------|-------------|
| `aws_access_key`   | ❌ No        | AWS access key ID (optional if using OIDC)                                                               | -           |
| `aws_secret_key`   | ❌ No        | AWS secret access key (optional if using OIDC)                                                           | -           |
| `aws_region`       | ✅ Yes       | AWS region (used by the CLI)                                                                             | -           |
| `role_to_assume`   | ❌ No        | AWS IAM role ARN to assume (OIDC)                                                                        | -           |
| `distribution_id`  | ✅ Yes       | CloudFront distribution ID (format: E + 13 alphanumeric chars, e.g. `E1234567890ABC`)                   | -           |
| `paths`            | ❌ No        | Space‑separated list of paths to invalidate (must start with `/`; max 1000 entries; wildcards allowed)   | `/*`        |
| `caller_reference` | ❌ No        | Custom caller reference for idempotency (auto‑generated if not provided)                                 | -           |

## 📤 Outputs
| **Name**          | **Description**                   |
|-------------------|-----------------------------------|
| `invalidation_id` | ID of the created invalidation    |
| `status`          | Status returned by CloudFront     |

## 📋 Examples
[View example →](./examples/base.yml)