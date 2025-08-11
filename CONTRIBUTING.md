# Contributing Guide

Thank you for your interest in contributing to GitHub Workflows & Actions! 

## Project Structure
```bash
   .
   ├── actions/                   # All reusable actions
   │   ├── action-name/
   │   │   ├── action.yml         # Action definition
   │   │   ├── readme.md          # Documentation
   │   │   └── examples/          # Usage examples
   │   └── internal/              # Internal composite actions
   ├── .github/workflows/         # CI/CD workflows
   ├── Taskfile.yml               # Development tasks
   └── README.md                  # Main documentation
```

## Getting Started
### Prerequisites
- [Task CLI](https://taskfile.dev/installation/)
- Docker
- Git

### Development Workflow
1. `Fork and clone` the repository
2. `Create a feature branch`:
```bash
git checkout -b feature/my-new-action
```
3. `Run linting`:
```bash
task yamllint
```
4. `Test your changes` using example workflows

## Adding a New Action
1. **Create directory structure**:
```bash
      actions/my-new-action/
      ├── action.yml
      ├── readme.md
      └── examples/
          └── base.yml
```
2. **Follow existing patterns**:
  - Look at [docker-build-push](./actions/docker-build-push/) as reference
  - Use composite actions for shell scripts
  - Validate all inputs
  - Provide clear error messages

3. **Documentation requirements**:
  - Complete `readme.md` with inputs/outputs tables
  - Practical usage examples
  - Prerequisites and limitations

4. **Testing**:
  - Add example workflow in `examples/`
  - Test manually with the example
  - Ensure `task yamllint` passes

## Action Standards
### Required Inputs
All actions must include these standard inputs:
```yaml
show_summary:
  description: 'Print summary in the job summary'
  required: false
  default: 'true'
```

### Summary Implementation
- Generate job summary using `$GITHUB_STEP_SUMMARY`
- Respect `show_summary` inputs
- Include key outputs, status, and relevant details

## Code Standards
- **YAML**: Follow `.yamllint.yml` rules
- **Shell scripts**: Use `set -euo pipefail`
- **Security**: Follow principle of least privilege
- **Error handling**: Fail fast with clear messages

## Submitting Changes
1. **Test thoroughly**:
```bash
task yamllint
```
2. **Commit with clear messages**:
```bash
git commit -m "[feat] add new action for xyz"
```
3. **Push and create PR**:
- Describe what the action does
- Include usage examples
- Reference any related issues

## Questions?
Open an [issue](https://github.com/Mad-Pixels/github-workflows/issues) 
or start a [discussion](https://github.com/Mad-Pixels/github-workflows/discussions)!

