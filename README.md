<picture>
 <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Mad-Pixels/.github/raw/main/profile/banner.png">
 <source media="(prefers-color-scheme: light)" srcset="https://github.com/Mad-Pixels/.github/raw/main/profile/banner.png">
 <img alt="MadPixels" src="https://github.com/Mad-Pixels/.github/raw/main/profile/banner.png">
</picture>

[![License](https://img.shields.io/github/license/Mad-Pixels/github-workflows?style=flat-square)](LICENSE) [![Latest Release](https://img.shields.io/github/v/release/Mad-Pixels/github-workflows?style=flat-square&logo=github)](https://github.com/Mad-Pixels/github-workflows/releases/latest)

# GitHub Workflows & Actions
This repository contains a collection of reusable **GitHub Actions** and **Reusable Workflows** designed to simplify CI/CD pipelines.  
 
> Each action and workflow is versioned and documented individually.  
> You can browse their respective `README.md` files for details, inputs, outputs, and usage examples.

Please visit [concept](./CONCEPT.md) docs to see how you can use [Taskfile](http://taskfile.dev) for CI process.

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

## Versioning

We use git tags. Pin consumers to stable majors, or exact releases when you need reproducibility. Tags are **repo-wide** (apply to all actions/workflows in this repo).

| Ref | Stability | Description | When to use |
|---|---|---|---|
| `@main` | ⚠️ Unstable | Latest commit on default branch | Testing and development |
| `@v1` | ✅ Stable | Moving major tag (non-breaking updates only) | **Recommended for production** |
| `@v1.2.3` | ✅ Stable | Exact released version (immutable) | Reproducible builds, controlled rollouts |
| `@<sha>` | 🔒 Frozen | Pinned commit (immutable) | Compliance/audit, maximum control |

**Notes:**
- Per-action tags like `@action-name/v1` are **not** supported by GitHub; tags are repository-scoped.
- Breaking changes will bump the major (e.g., `v2`). The `v1` tag will continue to receive backward-compatible updates.

**Examples:**
```yaml
uses: Mad-Pixels/github-workflows/actions/taskfile-runner@v1
uses: Mad-Pixels/github-workflows/actions/taskfile-runner@v1.2.3
uses: Mad-Pixels/github-workflows/actions/github-create-tag@v1
```

## Support
- 🐛 **Issues:** [GitHub Issues](https://github.com/Mad-Pixels/github-workflows/issues)
- 💬 **Discussions:** [GitHub Discussions](https://github.com/Mad-Pixels/github-workflows/discussions)

## Contributing
We ❤️ community contributions! Please see our [Contributing Guide](./CONTRIBUTING.md) for details.

## License
This repository is licensed under the [license](./LICENSE) _(Apache License 2.0)_.
