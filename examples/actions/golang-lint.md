# 🧬 GolangCI Lint · GitHub Composite Action

This composite GitHub Action runs golangci-lint against your Go project with configurable version and working directory.

## ✅ Features
- Automatically sets up Go environment
- Installs and runs golangci-lint with configurable version
- Supports custom working directory
- Fast and CI-friendly with timeout built-in

## 🔧 Usage Example
```yaml
name: Lint Go Code

on:
  push:
    branches: [main]

jobs:
  lint:
    uses: your-org/your-repo/.github/actions/golangci-lint@main
    with:
      go_dir: "Mad-Pixels/github-workflows/.github/actions/golang-lint@main"
      go_version: "1.21"
      golangci_lint_version: "v1.56.2"
```

## 📥 Inputs
| **Name**                | **Required** | **Description**                                         |
|-------------------------|--------------|---------------------------------------------------------|
| `golangci_lint_version` | ❌ No        | Version of golangci-lint to install (default: "v2.1.2") |
| `go_version`            | ❌ No        | Go version to install (default: "1.24")                 |
| `go_dir`                | ❌ No        | Directory with Go code (default: "./")                  |