<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Mad-Pixels/.github/raw/main/profile/banner.png">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/Mad-Pixels/.github/raw/main/profile/banner.png">
  <img alt="MadPixels" src="https://github.com/Mad-Pixels/.github/raw/main/profile/banner.png">
</picture>

# GitHub Workflows & Actions
This repository contains a collection of reusable **GitHub Actions** and **Reusable Workflows** designed to simplify CI/CD pipelines.  
  
> Each action and workflow is versioned and documented individually.
> You can browse their respective `README.md` files for details, inputs, outputs, and usage examples.

## Available Actions

### 📋 Taskfile
<table width="100%">
<tr>
<th>Action</th>
<th>Description</th>
</tr>
<tr>
<td><strong><a href="./actions/taskfile-runner">taskfile-runner</a></strong></td>
<td>Execute Taskfile commands</td>
</tr>
</table>

### 🐳 Container & Build
<table width="100%">
<tr>
<th>Action</th>
<th>Description</th>
</tr>
<tr>
<td><strong><a href="./actions/docker-build-push">docker-build-push</a></strong></td>
<td>Multi-platform Docker builds</td>
</tr>
</table>

### ☁️  AWS Infrastructure  
<table width="100%">
<tr>
<th>Action</th>
<th>Description</th>
</tr>
<tr>
<td><strong><a href="./actions/aws-terraform-runner">aws-terraform-runner</a></strong></td>
<td>Terraform with S3 backend</td>
</tr>
<tr>
<td><strong><a href="./actions/aws-lambda-restart">aws-lambda-restart</a></strong></td>
<td>Update Lambda container images</td>
</tr>
<tr>
<td><strong><a href="./actions/aws-s3-sync">aws-s3-sync</a></strong></td>
<td>Sync directory to S3</td>
</tr>
<tr>
<td><strong><a href="./actions/aws-cloudfront-invalidation">aws-cloudfront-invalidation</a></strong></td>
<td>Invalidate CloudFront cache</td>
</tr>
</table>

### 🏷️ Git Operations
<table width="100%">
<tr>
<th>Action</th>
<th>Description</th>
</tr>
<tr>
<td><strong><a href="./actions/github-create-tag">github-create-tag</a></strong></td>
<td>Create validated git tags</td>
</tr>
<tr>
<td><strong><a href="./actions/github-check-branch">github-check-branch</a></strong></td>
<td>Validate commit ancestry</td>
</tr>
</table>

### 📋 Taskfile
| Action                                               | Description                  |
|------------------------------------------------------|------------------------------|
| **[taskfile-runner](./actions/taskfile-runner)**     | Execute Taskfile commands    |

### 🐳 Container & Build
| Action                                               | Description                  |
|------------------------------------------------------|------------------------------|
| **[docker-build-push](./actions/docker-build-push)** | Multi-platform Docker builds |

### ☁️  AWS Infrastructure  
| Action                                                                   | Description                    |
|--------------------------------------------------------------------------|--------------------------------|
| **[aws-terraform-runner](./actions/aws-terraform-runner)**               | Terraform with S3 backend      |
| **[aws-lambda-restart](./actions/aws-lambda-restart)**                   | Update Lambda container images |
| **[aws-s3-sync](./actions/aws-s3-sync)**                                 | Sync directory to S3           |
| **[aws-cloudfront-invalidation](./actions/aws-cloudfront-invalidation)** | Invalidate CloudFront cache    |

### 🏷️ Git Operations
| Action                                                   | Description               |
|----------------------------------------------------------|---------------------------|
| **[github-create-tag](./actions/github-create-tag)**     | Create validated git tags |
| **[github-check-branch](./actions/github-check-branch)** | Validate commit ancestry  |

## Contributing
We ❤️ community contributions!

## License
This repository is licensed under the [license](./LICENSE) _(Apache License 2.0)_.

