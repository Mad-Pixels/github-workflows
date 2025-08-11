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
| Action | Description |
|---|---|
| **[taskfile-runner](./actions/taskfile-runner)** | Execute Taskfile commands |

### 🐳 Container & Build
| Action | Description |
|---|---|
| **[docker-build-push](./actions/docker-build-push)** | Multi-platform Docker builds |

### ☁️ AWS Infrastructure
| Action | Description |
|---|---|
| **[aws-terraform-runner](./actions/aws-terraform-runner)** | Terraform with S3 backend |
| **[aws-lambda-restart](./actions/aws-lambda-restart)** | Update Lambda container images |
| **[aws-s3-sync](./actions/aws-s3-sync)** | Sync directory to S3 |
| **[aws-cloudfront-invalidation](./actions/aws-cloudfront-invalidation)** | Invalidate CloudFront cache |

### 🏷️ Git Operations
| Action | Description |
|---|---|
| **[github-create-tag](./actions/github-create-tag)** | Create validated git tags |
| **[github-check-branch](./actions/github-check-branch)** | Validate commit ancestry |    |

## Contributing
We ❤️ community contributions!

## License
This repository is licensed under the [license](./LICENSE) _(Apache License 2.0)_.
