# 🧬 CloudFront Invalidation · GitHub Composite Action

This composite GitHub Action creates CloudFront invalidations to clear cache for updated content.

## ✅ Features
- Uses AWS CLI to create CloudFront invalidations
- Supports custom paths or wildcard invalidation
- Auto-generates unique caller reference
- Simple and fast cache clearing for static sites

## 🔧 Usage Example
```yaml
name: Deploy Static Site

on:
 push:
   branches: [main]

jobs:
 deploy:
   runs-on: ubuntu-latest
   steps:
     - name: Deploy to S3
       # ... your S3 sync step ...

     - name: Invalidate CloudFront
       uses: "Mad-Pixels/github-workflows/.github/actions/aws-cloudfront-invalidation@main"
       with:
         aws_access_key:     ${{ secrets.AWS_ACCESS_KEY_ID }}
         aws_secret_key:     ${{ secrets.AWS_SECRET_ACCESS_KEY }}
         aws_region:         "us-east-1"
         distribution_id:    ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID }}
         paths:              "/*"
```

## 📥 Inputs
| **Name**                | **Required** | **Description**                                   |
|-------------------------|--------------|---------------------------------------------------|
| `aws_access_key_id`     | ✅ Yes       | AWS access key ID                                 |
| `aws_secret_access_key` | ✅ Yes       | AWS secret access key                             |
| `aws_region`            | ✅ Yes       | AWS region (e.g. us-east-1)                       |
| `distribution_id`       | ✅ Yes       | CloudFront distribution ID                        |
| `paths`                 | ❌ No        | Paths to invalidate (default: "/*")               |
| `caller_reference`      | ❌ No        | Unique reference (auto-generated if not provided) |
