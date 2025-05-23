# 🧬 Task Runner · GitHub Composite Action

This composite GitHub Action runs a task command using a Taskfile, optionally setting up Go and exporting environment variables.

## ✅ Features
- Runs any task target
- Automatically installs go-task
- Optionally sets up Go environment
- Supports custom working directory
- Allows passing dynamic environment variables

## 🔧 Usage Example
```yaml
name: Run Taskfile Command

on:
  push:
    branches: [main]

jobs:
  task:
    runs-on: ubuntu-latest
    steps:
      - uses: your-org/task-runner@v1
        with:
          go_dir:     "Mad-Pixels/github-workflows/.github/actions/taskfile-runner@main"
          go_version: "1.22"
          command:    "build"
          vars:       "ENV:production,DEBUG:false"
```

## 📥 Inputs
| **Name**     | **Required** | **Description**                                                             |
|--------------|--------------|-----------------------------------------------------------------------------|
| `command`    | ✅ Yes       | Name of the task to run (e.g. build, deploy)                                |
| `go_version` | ❌ No        | Version of Go to set up (default: 1.24)                                     |
| `go_dir`     | ❌ No        | Directory to run the task from (default: ./)                                |
| `vars`       | ❌ No        | Comma-separated key:value pairs to export as env variables before task runs |