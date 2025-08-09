# 🧬 DockerHub Push · GitHub Composite Action

This composite GitHub Action builds and pushes multi-platform Docker images to DockerHub with support for artifacts and custom build arguments.

## ✅ Features

Multi-platform image building (linux/amd64, linux/arm64)
Support for build arguments via JSON
Works with artifacts or direct repository checkout
Automatic Docker Buildx setup with QEMU emulation
Custom Dockerfile and context path support

## 🔧 Usage Example
```yaml
name: Build and Push to DockerHub

on:
  push:
    branches: [main]

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - name: Push to DockerHub
        uses: "Mad-Pixels/github-workflows/.github/actions/dockerhub-push@main"
        with:
          docker_user:    ${{ secrets.DOCKER_USER }}
          docker_token:   ${{ secrets.DOCKER_TOKEN }}
          repository:     "myuser/myapp"
          tag:            "v1.0.0"
          platforms:      "linux/amd64,linux/arm64"
          build_args:     '{"NODE_ENV": "production", "VERSION": "1.0.0"}'
```

## 📥 Inputs
| **Name**          | **Required** | **Description**                                   |
|-------------------|--------------|---------------------------------------------------|
| `docker_user`     | ✅ Yes       | DockerHub username                                |
| `docker_token`    | ✅ Yes       | DockerHub access token                            |
| `repository`      | ✅ Yes       | DockerHub repository name (username/repository)   |
| `tag`             | ✅ Yes       | Docker image tag                                  |
| `platforms`       | ❌ No        | Comma-separated list of platforms                 |
| `build_args`      | ❌ No        | JSON object with build arguments                  |
| `artifact_name`   | ❌ No        | Artifact name to download (if using artifacts)    |
| `context_path`    | ❌ No        | Docker build context path                         |
| `dockerfile_path` | ❌ No        | Path to Dockerfile relative to context            |