# 🐳 Docker Build & Push.
Build and push multi-platform Docker images. Supports Docker Hub (default) and custom registries, artifact-based contexts, build cache, and digest output.

## ✅ Features
- Multi-arch builds via Buildx + QEMU (`linux/amd64`, `linux/arm64`, etc.)
- Push to Docker Hub or custom registry (`registry` input)
- Optional `:latest` tagging (`push_latest`)
- Use uploaded artifact as build context (CI-friendly)
- JSON-driven build args (stable parsing with `jq`)
- GitHub Actions cache for faster rebuilds
- Emits manifest-list digest for downstream steps

## 📖 Related Documentation
- Docker Buildx: https://docs.docker.com/build/buildx/
- Docker Hub: https://hub.docker.com/
- OCI Image Index (multi-arch): https://github.com/opencontainers/image-spec

## 🚀 Prerequisites
Your workflow must:
- Run on `ubuntu-latest` with Docker available
- Provide registry credentials (`docker_user`, `docker_token`)
- If using artifact context: ensure the artifact is produced earlier in the workflow

## 🔧 Quick Example
name: Build & Push Image

on:
  push:
    branches: [main]

jobs:
  docker:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - name: Build & Push
        uses: Mad-Pixels/github-workflows/actions/docker-build-push@v1
        with:
          docker_user: ${{ secrets.DOCKERHUB_USERNAME }}
          docker_token: ${{ secrets.DOCKERHUB_TOKEN }}
          repository: myuser/myimage
          tag: ${{ github.sha }}
          push_latest: 'false'
          platforms: linux/amd64,linux/arm64
          build_args: '{"VERSION":"${{ github.sha }}","NODE_ENV":"production"}'
          context_path: .
          dockerfile_path: Dockerfile

## 📥 Inputs
| **Name**         | **Required** | **Description**                                                                                     | **Default**                                 |
|------------------|--------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| `docker_user`    | ✅ Yes       | Registry username (Docker Hub by default)                                                           | -                                           |
| `docker_token`   | ✅ Yes       | Registry access token / password                                                                    | -                                           |
| `registry`       | ❌ No        | Registry host (e.g. `docker.io`, `ghcr.io`)                                                         | `docker.io`                                 |
| `repository`     | ✅ Yes       | Image repository (e.g. `user/image` or `ghcr.io/org/image` with non-default registry)              | -                                           |
| `tag`            | ✅ Yes       | Image tag (e.g. `v1.0.0`, `sha`)                                                                    | -                                           |
| `push_latest`    | ❌ No        | Also tag and push `:latest` (`true`/`false`)                                                        | `false`                                     |
| `platforms`      | ❌ No        | Target platforms (comma-separated)                                                                  | `linux/amd64,linux/arm64`                   |
| `build_args`     | ❌ No        | Build args as JSON object (values kept intact; requires valid JSON)                                 | `{}`                                        |
| `artifact_name`  | ❌ No        | If set, downloads artifact and uses it as build context                                             | `''`                                        |
| `context_path`   | ❌ No        | Build context path (relative to repo root or artifact root)                                         | `.`                                         |
| `dockerfile_path`| ❌ No        | Path to Dockerfile (relative to `context_path`)                                                     | `Dockerfile`                                |

## 📤 Outputs
| **Name**        | **Description**                          |
|-----------------|------------------------------------------|
| `image_digest`  | Pushed image manifest-list digest (sha)  |

## 📋 Examples
[View example →](./examples/base.yml)