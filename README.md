<picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Mad-Pixels/.github/raw/main/profile/banner.png">
    <source media="(prefers-color-scheme: light)" srcset="https://github.com/Mad-Pixels/.github/raw/main/profile/banner.png">
    <img alt="MadPixels" src="https://github.com/Mad-Pixels/.github/raw/main/profile/banner.png">
</picture>

# 🚀 GitHub Workflows & Actions Library

## Concept

### The Problem
Modern teams often face a gap between local development and CI/CD environments.  
Scripts and tooling are scattered, environments drift, and debugging CI becomes a trial-and-error nightmare.

### Core Philosophy: Taskfile-Centric Development
We define all developer-facing tasks in `Taskfile.yml` ([taskfile](https://taskfile.dev/)), making it the **single source of truth** for every operation:
- 🟢 **Consistent across environments** — same commands work locally and in CI
- 🐳 **Containerized by default** — all tasks run securely in isolated Docker containers
- 🔧 **Empowers developers** — no hidden magic, all tools and commands are transparent and reproducible

### Secure Container Execution
Each task runs in a hardened, ephemeral container:
- 🔒 **Security-first** — dropped capabilities, non-root users, read-only mounts
- 📦 **Explicit tooling** — versions are locked (e.g. Node 23, Terraform 1.8.5)
- 🔁 **Unified interface** — all orchestration handled by a single internal `_docker/run` task

### GitOps-Driven by Design
This approach follows GitOps principles:
- ✍️ **Declarative** — all tasks defined and version-controlled in Git
- 🔁 **Reproducible** — identical environments at every run
- 🕵️‍♂️ **Auditable** — clear separation between dev and deploy operations

### Local-to-CI Reproducibility
If it works locally, it works in CI — **guaranteed**:
- Developers run `task build`, `task lint`, or `task type-check` inside Docker
- CI invokes the exact same tasks via the `taskfile-runner` GitHub Action
- No CI-specific scripts or hidden steps
- Debugging happens locally, not through failed pipelines

### Deployment: Controlled & Isolated
Deployment operations live in **reusable GitHub Actions**, completely separate from development tasks:
- 🧱 **Clear separation** — developers focus on code, platform handles deployment  
- 🔐 **Secure by design** — production access restricted to approved CI events
- 🔁 **Reusable workflows** — standardized deployment patterns across projects

### Business Benefits
- 💰 **Reduced maintenance cost** — no more “works on my machine” syndrome
- 🚀 **Faster delivery** — fewer environment-specific bugs
- 🔐 **Stronger security** — containerized, read-only, non-root execution
- 📋 **Clear audit trails** — Git history = deployment history
- 🧠 **Better onboarding** — new engineers only need Docker and `task`

## Getting Started

### Prerequisites
- 🐳 **Docker** — all tasks run in containers
- 📋 **Task** — install from [taskfile.dev](https://taskfile.dev/installation/)
- 🐙 **Git** — for version control and workflow triggers

### Step 1: Structure Your Project
Create the foundation for Taskfile-centric development:
```bash
your-project/
├── Taskfile.yml          # Your development tasks
├── .github/
│   └── workflows/
│       └── ci.yml        # CI using your tasks
└── src/                  # Your application code
```

### Step 2: Create Secure Container Runtime
Establish the foundation for containerized task execution with our proven security pattern:
```yaml
# Taskfile.yml - Internal task for secure container runtime
_docker/run:
  internal: true
  cmd: |
    docker run --rm --init {{if .TTY}}-it{{end}} \
      --cap-drop=ALL \
      --security-opt no-new-privileges \
      --user $(id -u):$(id -g) \
      --workdir /workspace \
      --volume "{{.PWD}}/{{.MOUNT_DIR}}:/workspace:rw" \
      {{.IMAGE}} {{.CMD}}
  requires:
    vars: [IMAGE, CMD, MOUNT_DIR]
```

Example usage patterns:
```yaml
lint:
  desc: "Run code linting"
  cmds:
    - task: _docker/run
      vars:
        IMAGE: "node:20"
        CMD: "npm run lint"
        MOUNT_DIR: "."

test:
  desc: "Run test suite"
  cmds:
    - task: _docker/run
      vars:
        IMAGE: "golang:1.21"
        CMD: "go test ./..."
        MOUNT_DIR: "."
...
```

Step 3: Design Your Task Structure
Build tasks around developer-facing operations that need local execution and debugging:
✅ Include in Taskfile:
lint — code quality checks
format — code formatting
test — unit and integration tests
build — compilation and bundling
dev — local development server
type-check — static analysis

❌ Keep out of Taskfile:
Deployment operations
Infrastructure management
Production secrets handling
Cloud resource provisioning

Step 4: Connect to CI
Use our `taskfile-runner` action to execute the same tasks in GitHub Actions:
```yaml
# .github/workflows/ci.yml
name: CI Pipeline
on: [push, pull_request]

jobs:
  quality-checks:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Run linting
        uses: Mad-Pixels/github-workflows/.github/actions/taskfile-runner@main
        with:
          command: "lint"
          
      - name: Run tests  
        uses: Mad-Pixels/github-workflows/.github/actions/taskfile-runner@main
        with:
          command: "test"
```
