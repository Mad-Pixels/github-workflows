# 🧬 S3 Sync · GitHub Composite Action

This GitHub composite action uploads a local directory to an AWS S3 bucket using aws s3 sync. It supports optional path prefixing and cleans up removed files with --delete.

## ✅ Features
- Upload any local folder to S3
- AWS credentials via inputs
- Optional bucket_prefix
- Excludes common .git files
- Cleans up deleted files with --delete

## 🔧 Usage Example
```yaml
name: Upload Static Files

on:
  push:
    branches: [main]

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code (if needed)
        uses: actions/checkout@v4

      - name: Upload to S3
        uses: "Mad-Pixels/github-workflows/.github/actions/aws-s3-sync@main"
        with:
          aws_access_key:     ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws_secret_key:     ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws_region:         us-east-1
          bucket_name:        my-app-assets
          source_dir:         ./public
          bucket_prefix:      static/
```

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Build site
        run: npm run build
        
      - name: Upload artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build-files
          path: dist/

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Download artifacts
        uses: actions/download-artifact@v4
        with:
          name: build-files
          path: build-output
          
      - name: Upload to S3
        uses: "Mad-Pixels/github-workflows/.github/actions/aws-s3-sync@main"
        with:
          aws_access_key: ${{ secrets.AWS_ACCESS_KEY }}
          aws_secret_key: ${{ secrets.AWS_SECRET_KEY }}
          aws_region: ${{ secrets.AWS_REGION }}
          bucket_name: ${{ secrets.BUCKET_NAME }}
          source_dir: "build-output"
```

## 📥 Inputs
| **Name**                | **Required** | **Description**                            |
|-------------------------|--------------|--------------------------------------------|
| `aws_access_key`        | ✅ Yes       | AWS access key ID                          |
| `aws_secret_key`        | ✅ Yes       | AWS secret access key                      |
| `aws_region`            | ✅ Yes       | AWS region (e.g. us-east-1)                |
| `bucket_name`           | ✅ Yes       | Name of the S3 bucket                      |
| `source_dir`            | ✅ Yes       | Local directory to sync                    |
| `bucket_prefix`         | ❌ No        | Optional path inside bucket (e.g. static/) |
