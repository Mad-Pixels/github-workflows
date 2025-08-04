<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Mad-Pixels/.github/raw/main/profile/banner.png">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/Mad-Pixels/.github/raw/main/profile/banner.png">
  <img alt="MadPixels" src="https://github.com/Mad-Pixels/.github/raw/main/profile/banner.png">
</picture>

# 🚀 GitHub Workflows & Actions Library

## Table of Contents
- [Concept](#concept)
  - [The Problem](#the-problem)
  - [Core Philosophy: Taskfile-Centric Development](#core-philosophy-taskfile-centric-development)
  - [Secure Container Execution](#secure-container-execution)
  - [GitOps-Driven by Design](#gitops-driven-by-design)
  - [Local-to-CI Reproducibility](#local-to-ci-reproducibility)
  - [Deployment: Controlled & Isolated](#deployment-controlled--isolated)
  - [Business Benefits](#business-benefits)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Step 1: Structure Your Project](#step-1-structure-your-project)
  - [Step 2: Create Secure Container Runtime](#step-2-create-secure-container-runtime)
  - [Step 3: Design Your Task Structure](#step-3-design-your-task-structure)
  - [Step 4: Connect to CI](#step-4-connect-to-ci)
- [Real-World examples](#real-world-examples)
- [Contributing](#contributing)
- [License](#license)

## Concept

### The Problem
Modern teams often face a gap between local development and CI/CD environments.  
Scripts and tooling are scattered, environments drift, and debugging CI becomes a trial-and-error nightmare.

### Traditional vs Our Approach
```bash
┌─────────────────────────────────────────────────────────────┐
│  Local Dev            CI Environment       Production       │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐  │
│  │ Install     │  ❌  │ Install     │  ❌  │ Yet another │  │
│  │ deps        │  ≠≠  │ different   │  ≠≠  │ environment │  │
│  │ locally..   │      │ versions..  │      │             │  |
│  │ Run tests   │      │ Run CI      │      │ Deploy      |  |
│  └─────────────┘      └─────────────┘      └─────────────┘  │
│         │                   │                     │         │
│         ▼                   ▼                     ▼         │
│   local machine      different behavior   surprises on Prod │
│        🖥️                  😩                    💣         │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                      OUR APPROACH                           │
├─────────────────────────────────────────────────────────────┤
│  Local Dev           CI Environment        Production       │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐  │
│  │ task lint   │  ✅  │ taskfile-   │  ✅  │ Reusable    │  │
│  │ task test   │  ══  │ runner      │  ══  │ Actions     │  │
│  │ task build  │      │ (same tasks)│      │ (controlled)│  │
│  └─────────────┘      └─────────────┘      └─────────────┘  │
│         │                    │                    │         │
│         ▼                    ▼                    ▼         │
│   docker containers   docker containers      GitOps flow    │
│        🐳                   🐳                   🔁         │
└─────────────────────────────────────────────────────────────┘
```

### Core Philosophy: Taskfile-Centric Development
We define all developer-facing tasks in `Taskfile.yml` ([taskfile](https://taskfile.dev/)), making it the **single source of truth** for every operation:
- 🟢 `Consistent across environments` — same commands work locally and in CI
- 🐳 `Containerized by default` — all tasks run securely in isolated Docker containers
- 🔧 `Empowers developers` — no hidden magic, all tools and commands are transparent and reproducible

### Secure Container Execution
```bash
┌─────────────────────────────────────────────────────────────┐
│  Host System                                                │
│  ┌───────────────────────────────────────────────────────┐  │
│  │    Docker Container (Hardened)                        │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │ 🔒 --cap-drop=ALL                               │  │  │
│  │  │ 🚫 --security-opt no-new-privileges             │  │  │
│  │  │ 👤 --user $(id -u):$(id -g)                     │  │  │
│  │  │ 📁 /workspace (read-write, project only)        │  │  │
│  │  │                                                 │  │  │
│  │  │ Your Task Execution:                            │  │  │
│  │  │ • npm ci && npx eslint .                        │  │  │
│  │  │ • cargo test                                    │  │  │
│  │  │ • go build                                      │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────────────┘  │
│  🎯 Result: Isolated, reproducible, secure execution        │
└─────────────────────────────────────────────────────────────┘
```
Each task runs in a hardened, ephemeral container:
- 🔒 `Security-first` — dropped capabilities, non-root users, read-only mounts
- 📦 `Explicit tooling` — versions are locked (e.g. Node 23, Terraform 1.8.5)
- 🔁 `Unified interface` — all orchestration handled by a single internal `_docker/run` task

### GitOps-Driven by Design
This approach follows GitOps principles:
- ✍️ `Declarative` — all tasks defined and version-controlled in Git
- 🔁 `Reproducible` — identical environments at every run
- 🕵️‍♂️ `Auditable` — clear separation between dev and deploy operations

### Local-to-CI Reproducibility
If it works locally, it works in CI — **guaranteed**:
- Developers run `task build`, `task lint`, or `task type-check` inside Docker
- CI invokes the exact same tasks via the `taskfile-runner` GitHub Action
- No CI-specific scripts or hidden steps
- Debugging happens locally, not through failed pipelines

### Deployment: Controlled & Isolated
Deployment operations live in **reusable GitHub Actions**, completely separate from development tasks:
- 🧱 `Clear separation` — developers focus on code, platform handles deployment  
- 🔐 `Secure by design` — production access restricted to approved CI events
- 🔁 `Reusable workflows` — standardized deployment patterns across projects

### Business Benefits
- 💰 `Reduced maintenance cost` — no more “works on my machine” syndrome
- 🚀 `Faster delivery` — fewer environment-specific bugs
- 🔐 `Stronger security` — containerized, read-only, non-root execution
- 📋 `Clear audit trails` — Git history = deployment history
- 🧠 `Better onboarding` — new engineers only need Docker and `task`

## Getting Started
```bash
┌─────────────────────────────────────────────────────────────┐
│  1. Local Development                                       │
│   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐     │
│   │ task lint   │───▶│ task test   │───▶│ task build  │     │
│   └─────────────┘    └─────────────┘    └─────────────┘     │
│          │                   │                   │          │
│          ▼                   ▼                   ▼          │
│     ✅ Success          ✅ Success          ✅ Success      │
│                                                             │
│  2. Push to GitHub                                          │
│   ┌─────────────────────────────────────────────────────┐   │
│   │ GitHub Actions runs SAME tasks:                     │   │
│   │ • taskfile-runner: "lint"                           │   │
│   │ • taskfile-runner: "test"                           │   │
│   │ • taskfile-runner: "build"                          │   │
│   └─────────────────────────────────────────────────────┘   │
│          │                   │                   │          │
│          ▼                   ▼                   ▼          │
│     ✅ Success          ✅ Success          ✅ Success      │
│                                                             │
│  3. Deployment (Separate Actions)                           │
│   ┌─────────────────────────────────────────────────────┐   │
│   │ • dockerhub-build-push                              │   │
│   │ • aws-lambda-restart                                │   │
│   │ • terraform-runner                                  │   │
│   │ • etc ...                                           │   │
│   └─────────────────────────────────────────────────────┘   │
│  🎯 Guaranteed: Local success = CI success                  │
└─────────────────────────────────────────────────────────────┘
```
### Prerequisites
- 🐳 `Docker` — all tasks run in containers
- 📋 `Task` — install from [taskfile.dev](https://taskfile.dev/installation/)
- 🐙 `Git` — for version control and workflow triggers

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
      --volume "{{.git_root}}/{{.MOUNT_DIR}}:/workspace:rw" \
      {{if .ENVS}}{{range $env := .ENVS}}--env {{$env}} {{end}}{{end}} \
      {{if .PORTS}}{{range $port := .PORTS}}--publish {{$port}} {{end}}{{end}} \
      {{.IMAGE}} \
      {{.CMD}}
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
        CMD: "sh -c 'npm ci && npx eslint .'"
        MOUNT_DIR: "."
        ENVS:
          - "NPM_CONFIG_CACHE=/workspace/.cache"
          - "NPM_CONFIG_UPDATE_NOTIFIER=false"

test:
  desc: "Run test suite"
  cmds:
    - task: _docker/run
      vars:
        IMAGE: "golang:1.21"
        CMD: "go test ./..."
        MOUNT_DIR: "."

run_dev:
    desc: Run dev on {{ .dev_port }}.
    cmds: 
      - task: _docker/run 
        vars: 
          IMAGE: "node:{{.node_version}}"
          CMD: "sh -c 'npm ci && npm run dev -- --host 0.0.0.0 --port {{ .dev_port }}'"
          MOUNT_DIR: "site"
          PORTS:
            - "{{ .dev_port }}:{{ .dev_port }}"
          ENVS:
            - "NPM_CONFIG_CACHE=/workspace/.cache"
            - "NPM_CONFIG_UPDATE_NOTIFIER=false"
          TTY: "true" # Interactive mode for dev servers
```

### Step 3: Design Your Task Structure
Build tasks around developer-facing operations that need local execution and debugging:  
| ✅ Include in Taskfile examples             | ❌ Keep out of Taskfile examples      |
|---------------------------------------------|---------------------------------------|
| lint for code quality checks                | deployment operations                 |
| code formatting                             | infrastructure management             |
| unit and integration tests                  | production secrets handling           |
| compilation and bundling                    | cloud resource provisioning           |
| local development server                    |                                       |
| static analysis                             |                                       |

### Step 4: Connect to CI
Use our [taskfile-runner](https://github.com/Mad-Pixels/github-workflows/blob/main/.github/actions/taskfile-runner/action.yml) action to execute the same tasks in GitHub Actions:
```yaml
# .github/workflows/ci.yml
name: CI Pipeline
on: [push, pull_request]

jobs:
  quality-checks:
    runs-on: ubuntu-latest
    steps:
      - name: Run linting
        uses: Mad-Pixels/github-workflows/.github/actions/taskfile-runner@main
        with:
          command: "lint"
          
      - name: Run tests  
        uses: Mad-Pixels/github-workflows/.github/actions/taskfile-runner@main
        with:
          command: "test"

  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build
        uses: Mad-Pixels/github-workflows/.github/actions/taskfile-runner@main
        with:
          command: "build"

    # CD part which not implement in Taskfile
    deploy:
      runs-on: ubuntu-latest
      ... your deploy process ...
```

## Real-World Examples
To make adoption as smooth as possible, we’ve prepared several opinionated example projects under [examples/](https://github.com/Mad-Pixels/github-workflows/tree/main/examples).  
Each contains its own `Taskfile.yml` and CI workflow, and is fully runnable out of the box:

| Example                               | Description                                                                                                   |
|---------------------------------------|---------------------------------------------------------------------------------------------------------------|
| [nodejs](./examples/flows/nodejs)     | Minimal Node.js starter: lint, format, unit tests, build, deploy (external job) and dev server all in Docker. |

## Contributing
We ❤️ community contributions! To get started:
1. **Fork** the repo and create your feature branch:
```bash
git clone git@github.com:Mad-Pixels/.github.git
cd .github
git checkout -b feature/my-cool-task
```
2. Make your changes in Taskfile.yml, workflows, or docs.
3. Run the examples locally to verify nothing breaks
4. Commit your changes
5. Push and open a Pull Request against `main`.
6. We’ll review, suggest feedback, and merge when ready!

Feel free to open issues for bugs or feature requests, and tag them with appropriate labels.

## License
This repository is licensed under the [license](./LICENSE) _(Apache License 2.0)_.
