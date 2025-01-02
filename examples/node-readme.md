# Node.js CI/CD
Welcome to the Node.js CI/CD documentation!

This guide provides a comprehensive overview of setting up Continuous Integration and Continuous Deployment (CI/CD) workflows for Node.js projects using GitHub Actions.

## Workflow Overview
### Node Build
The [node-build.yml](https://github.com/Mad-Pixels/github-workflows/blob/main/.github/workflows/node-build.yml) workflow is responsible for building Node.js projects (e.g., Vue.js) across multiple platforms. It installs dependencies, runs builds, and prepares build artifacts for deployment.

#### Key Features:
- Node.js Version Management: Supports customizable Node.js versions via inputs.
- Dependency Caching: Uses GitHub Actions cache for `npm` to speed up builds.
- Artifact Management: Collects and uploads build artifacts for further use.

#### Inputs:
| Input Name          | Required | Default | Description                                     |
|---------------------|----------|---------|-------------------------------------------------|
| `working-directory` | 🔴       | ""      | Directory containing Vue project (package.json) |
| `node-version`      | 🔴       | `20`    | Node.js version to use                          |

## Using Workflows
The workflow [node-build.yml](https://github.com/Mad-Pixels/github-workflows/blob/main/.github/workflows/node-build.yml) can be reused across different CI/CD scenarios. Below are examples of how to integrate this workflow into your project.

### Pull Request (main)
Triggers on pull requests targeting the main branch and runs the Node.js build process.

```yaml
name: PR Main

on:
  pull_request:
    types: [opened, synchronize, reopened]
    branches:
      - main

jobs:
  build:
    uses: Mad-Pixels/github-workflows/.github/workflows/node-build.yml@main
    with:
      working-directory: "./frontend" # Directory containing Vue project
      node-version: "20"              # Node.js version to use
```

### Release (example with sync to AWS S3 bucket)
Triggers on pushes that create version tags following the semantic versioning pattern (e.g., v1.2.3). This workflow verifies the tag, builds, and sync data with AWS S3.

```yaml
name: Release

on:
  push:
    tags:
      - "v[0-9]+.[0-9]+.[0-9]+"

jobs:
  verify:
    uses: Mad-Pixels/github-workflows/.github/workflows/github-check-tag.yml@main

  build:
    uses: Mad-Pixels/github-workflows/.github/workflows/node-build.yml@main
    with:
      working-directory: ./app
      node-version: '20'

  discover:
    needs: [build]
    runs-on: ubuntu-24.04
    steps:
      - name: Download artifacts
        uses: actions/download-artifact@v4
        with:
          name: vue-dist
          path: dist_files
    
  discover:
    needs: [build]
    runs-on: ubuntu-24.04
    steps:
      - name: Download artifacts
        uses: actions/download-artifact@v4
        with:
          name: dist
          path: dist_files

  upload:
    needs: [discover]
    uses: Mad-Pixels/github-workflows/.github/workflows/aws-s3-sync.yml@main
    with:
      artifacts: "dist"
      working_directory: "dist_files"
      actions_verions: "0.0.1"
    secrets:
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_KEY }}
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY }}
      BUCKET_DESTINATION: ${{ secrets.DOCS_BUCKET }}
```