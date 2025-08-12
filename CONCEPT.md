# Concept: Taskfile-Centric Development
## Why
Teams waste time fighting environment drift between local dev and CI. 
Scripts scatter across repos, CI has hidden steps, and debugging becomes trial-and-error. 
We fix this by making **Taskfile the single source of truth** and running everything in **secure containers** — locally and in CI.

## Who This Is For
- Teams with complex build/test pipelines
- Infrastructure-heavy projects (AWS, Terraform, CD)
- Organizations wanting reproducibility and quick onboarding

## Core Principles
1. **Reproducibility** — same Task runs identically everywhere
2. **Isolation** — no host dependencies beyond Docker + Task
3. **Security by default** — non-root, dropped caps, no-new-privileges
4. **Transparency** — no CI-only magic; everything in `Taskfile.yml`
5. **Separation of concerns** — CI (dev tasks) in Taskfile; CD (deploy) in Actions

## Container Security Defaults
- `--cap-drop=ALL` — no privileged capabilities
- `--security-opt no-new-privileges` — prevent privilege escalation
- `--user $(id -u):$(id -g)` — non-root execution
- `--workdir /workspace` — consistent working directory
- Project mount only: `/workspace` (read-write)

## Container Runtime Pattern
Pull image once (quiet):
```yaml
   _docker/pull:
     internal: true
     cmds:
       - |
         if ! docker image inspect "{{.IMAGE}}" >/dev/null 2>&1; then
           docker pull -q "{{.IMAGE}}" >/dev/null 2>&1 || {
             echo "Failed to pull image: {{.IMAGE}}"
             exit 1
           }
         fi
     silent: true
     requires:
       vars: [IMAGE]
```
Run securely (never pull during execution):
```yaml
   _docker/run:
     internal: true
     dir: "{{.git_root}}"
     deps:
       - task: _docker/pull
         vars: { IMAGE: "{{.IMAGE}}" }
     cmd: |
       docker run --rm --init --pull=never {{if .TTY}}-it{{end}} \
         --cap-drop=ALL \
         --security-opt no-new-privileges \
         --user $(id -u):$(id -g) \
         --workdir /workspace \
         {{if .ENVS}}{{range $e := .ENVS}}--env {{$e}} {{end}}{{end}}\
         {{if .PORTS}}{{range $p := .PORTS}}--publish {{$p}} {{end}}{{end}}\
         {{if .VOLUMES}}{{range $v := .VOLUMES}}--volume {{$v}} {{end}}{{end}}\
         --volume "{{.git_root}}/{{.MOUNT_DIR}}:/workspace:rw" \
         "{{.IMAGE}}" \
         {{.CMD}}
     silent: true
     requires:
       vars: [IMAGE, CMD, MOUNT_DIR]
```

### Runtime Variables
| Variable | Purpose | Example |
|---|---|---|
|`IMAGE`|Docker image (pin in CI)|`node:20`, `golang:1.22@sha256:...`|
|`CMD`|Command to execute|`sh -c 'npm ci && npm test'`|
|`MOUNT_DIR`|Project directory to mount|`"."`, `"site"`, `"infra"`|
|`ENVS`|Environment variables|`["GOOS=linux","NPM_CONFIG_CACHE=/cache"]`|
|`PORTS`|Port mappings for dev servers|`["3000:3000"]`|
|`VOLUMES`|Additional mounts|`["$HOME/.ssh:/ssh:ro"]`|
|`TTY`|Interactive mode|`"true"` for dev servers|

## Task Examples
```yaml
lint:
  desc: "Run code linting"
  cmds:
    - task: _docker/run
      vars:
        IMAGE: "node:20"
        MOUNT_DIR: "."
        ENVS: ["NPM_CONFIG_CACHE=/workspace/.cache"]
        CMD: "sh -c 'npm ci && npx eslint .'"

  test:
    desc: "Run test suite"
    cmds:
      - task: _docker/run
        vars:
          IMAGE: "golang:1.22"
          MOUNT_DIR: "."
          CMD: "go test ./..."

  dev:
    desc: "Development server with hot reload"
    cmds:
      - task: _docker/run
        vars:
          IMAGE: "node:20"
          MOUNT_DIR: "."
          PORTS: ["3000:3000"]
          TTY: "true"
          CMD: "sh -c 'npm ci && npm run dev -- --host 0.0.0.0'"
```

## CI Integration
Use the same tasks in GitHub Actions:
```yaml
jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
         
      - uses: Mad-Pixels/github-workflows/actions/taskfile-runner@v1
        with:
          command: "lint"
             
      - uses: Mad-Pixels/github-workflows/actions/taskfile-runner@v1
        with:
          command: "test"
```

## What Goes Where
|✅ Include in Taskfile|❌ Keep in Actions|
|---|---|
|Code linting/formatting|AWS deployments|
|Unit/integration tests|Infrastructure provisioning|
|Building/compilation|Production secrets handling|
|Development servers|Cloud resource management|
|Static analysis|Terraform apply operations|

## Benefits
- 💰 **Reduced maintenance** — no more "works on my machine"
- 🚀 **Faster delivery** — fewer environment-specific bugs
- 🔐 **Stronger security** — containerized, non-root execution
- 📋 **Clear audit trails** — Git history = deployment history
- 🧠 **Better onboarding** — new engineers only need Docker and Task

## Local-to-CI Guarantee
If it works locally, it works in CI — **guaranteed**:
- Same Docker images and commands
- Same environment variables and mounts  
- No CI-specific scripts or hidden steps
- Debug locally, not through failed pipelines

## Best Practices
- **Pin images by digest in CI** for determinism: `node:20@sha256:...`
- **Use project-local caches** under `/workspace/.cache`
- **Mount read-only where possible**: `["$HOME/.ssh:/ssh:ro"]`
- **Validate inputs** in task descriptions and error messages
- **Keep secrets out of Taskfile** — handle via Actions only

## Real Project Examples
| Name | Description |
|---|---|
|**[about](https://github.com/mr-chelyshkin/about)**|VueJS static site with containerized build and AWS deployment|

