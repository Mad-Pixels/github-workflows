# 🧬 [Taskfile](https://taskfile.dev/) Runner
Execute Taskfile commands.

## ✅ Features
- Automatically installs go-task with specified version
- Validates Taskfile existence before execution
- Supports custom working directory and environment variables
- Linux runners (ubuntu-latest) with AMD64 and ARM64 support
- Detailed output capture and error handling
- Security-focused execution model

## 📖 Related Documentation
- [📋 Taskfile Documentation](https://taskfile.dev/)
- [💡 Usage concept](../../../Concept.md)

## 🚀 Prerequisites
Your repository must contain:
- `Taskfile.yml` or `Taskfile.yaml` in the specified directory
- The Taskfile must contain the tasks you intend to run via this action

## 🔧 Quick Example
```yaml
name: CI Pipeline

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Run tests
        uses: Mad-Pixels/github-workflows/actions/taskfile-runner@v1
        with:
          command: "test"
```

## 📥 Inputs
| **Name**     | **Required** | **Description**                                                             | **Default** |
|--------------|--------------|-----------------------------------------------------------------------------|-------------|
| `command`    | ✅ Yes       | Name of the task to run (e.g. build, test, lint)                            | -           |
| `vars`       | ❌ No        | Comma-separated key:value pairs (values must not contain ',' or ':')        | -           |
| `dir`        | ❌ No        | Directory to run the task from                                              | `.`         |
| `version`    | ❌ No        | Version of go-task to install                                               | `3.44.1`    |

## 📤 Outputs
| **Name**       | **Description**                    |
|----------------|------------------------------------|
| `task_version` | Installed Task version             |
| `task_output`  | Complete output from task command  |

## 📋 Examples
[View example →](./examples/base.yml)
