# 🧬 Taskfile Runner
Invoke [Taskfile](https://taskfile.dev/) commands.

## ✅ Features
- Linux runners only (`ubuntu-latest`), amd64 and arm64
- Auto-installs `Taskfile` for a specified version (cached per version+arch)
- Supports custom working directory and environment variables
- Captures full stdout/stderr and exposes it via outputs

## 📖 Related Documentation
- [📋 Taskfile Documentation](https://taskfile.dev/)
- [💡 Usage concept](../../../Concept.md)

## 🚀 Prerequisites
Your repository must contain:
- `Taskfile.yml` or `Taskfile.yaml` in the directory you run the action from
- The tasks you plan to invoke

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
| **Name**        | **Required** | **Description**                                                                                                                              | **Default** |
|-----------------|--------------|----------------------------------------------------------------------------------------------------------------------------------------------|-------------|
| `command`       | ✅ Yes       | Name of the task to run (e.g. build, test, lint)                                                                                             | -           |
| `vars`          | ❌ No        | Comma-separated key=value pairs. Values may contain = and newlines; commas are not allowed. Leading/trailing spaces around pairs are trimmed | -           |
| `dir`           | ❌ No        | Working directory for the Taskfile                                                                                                           | `.`         |
| `version`       | ❌ No        | Version of go-task to install                                                                                                                | `3.44.1`    |
| `show_summary`  | ❌ No        | Print summary with task output in job summary                                                                                                | `true`    |
| `summary_limit` | ❌ No        | Max number of output lines to show in summary                                                                                                | `250`    |

## 📤 Outputs
| **Name**       | **Description**                    |
|----------------|------------------------------------|
| `task_version` | Installed Task version             |
| `task_command` | Task command                       |
| `task_output`  | Complete output from task command  |

## 📋 Examples
[View example →](./examples/base.yml)

