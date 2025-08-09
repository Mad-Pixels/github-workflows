# 🧬 Taskfile Runner
Execute Taskfile commands.

## ✅ Features
- Automatically installs go-task with specified version
- Validates Taskfile existence before execution
- Supports custom working directory and environment variables
- Cross-platform support (AMD64, ARM64)
- Detailed output capture and error handling
- Security-focused execution model

## 🔧 Quick Example
```yaml
name: CI Pipeline

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Run tests
        uses: Mad-Pixels/github-workflows/actions/ci/taskfile-runner@v1.0.0
        with:
          command: "test"
```

## 📥 Inputs
| **Name**     | **Required** | **Description**                                                             | **Default** |
|--------------|--------------|-----------------------------------------------------------------------------|-------------|
| `command`    | ✅ Yes       | Name of the task to run (e.g. build, test, lint)                            | -           |
| `vars`       | ❌ No        | Comma-separated key:value pairs to export as env variables before task runs | -           |
| `dir`        | ❌ No        | Directory to run the task from                                              | `.`         |
| `version`    | ❌ No        | Version of go-task to install                                               | `3.44.1`    |

## 📤 Outputs
| **Name**       | **Description**                    |
|----------------|------------------------------------|
| `task_version` | Installed Task version             |
| `task_output`  | Complete output from task command  |


## 📋 Examples
### Basic Usage
[View example →](./examples/basic.yml)

### Advanced with Environment Variables
[View example →](./examples/advanced.yml)

## 📖 Related Documentation
- [📋 Taskfile Documentation](https://taskfile.dev/)
- [🚀 Getting Started Guide](../../../docs/quick-start.md)
- [💡 Best Practices](../../../docs/best-practices.md)