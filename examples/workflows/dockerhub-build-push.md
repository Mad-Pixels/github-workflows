# 🌍 DockerHub Push · GitHub Reusable Workflow

This reusable GitHub Actions workflow builds and pushes Docker images to DockerHub using buildx with multi-platform support.

## ✅ Features
- Multi-platform image builds via Docker buildx
- Support for custom build arguments
- Secure DockerHub authentication via secrets
- Artifact-based Docker context

## 🔧 Usage Example
```yaml
name: Push Docker Image

on:
  push:
    branches: [main]

jobs:
  docker-push:
    uses: your-org/your-repo/.github/workflows/dockerhub-push.yml@main
    with:
      repository: your-user/your-image
      tag:        v1.0.0
      build_args: '{"ENV":"prod","VERSION":"1.0.0"}'
      platforms:  linux/amd64,linux/arm64
      context:    docker/
    secrets:
      docker_user:  ${{ secrets.DOCKERHUB_USERNAME }}
      docker_token: ${{ secrets.DOCKERHUB_TOKEN }}
```

## 🔐 Required Secrets
| **Name**         | **Description**        |
|------------------|------------------------|
| `docker_user`    | DockerHub username     |
| `docker_token`   | DockerHub access token |

## 📥 Inputs
| **Name**     | **Required** | **Description**                                                             |
|--------------|--------------|-----------------------------------------------------------------------------|
| `repository` | ✅ Yes       | DockerHub image repository (e.g. user/image                                 |
| `tag`        | ✅ Yes       | Docker image tag (e.g. v1.0.0)                                              |
| `build_args` | ❌ No        | JSON object with build args (e.g. {"ENV":"prod"})                           |
| `platforms`  | ❌ No        | Comma-separated list of target platforms (default: linux/amd64,linux/arm64) |
| `context`    | ❌ No        | Docker build context directory (default: ".")                               |