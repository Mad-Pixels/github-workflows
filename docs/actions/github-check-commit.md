# 🧬 Is Commit · GitHub Composite Action

This composite GitHub Action determines if a push event is a normal commit (proceed=true) or the result of a merged/squashed pull request (proceed=false) by inspecting GitHub API data associated with the current commit SHA.

## ✅ Features
- Detects whether a commit was made directly or as part of a PR merge/squash
- Outputs proceed=true only for direct commits
- Supports fallback detection via GitHub Search API
- Useful for conditionally skipping workflows on PR merges

## 🔧 Usage Example
```yaml
name: Check Commit Type

on:
  push:
    branches: [main]

jobs:
  check-commit:
    runs-on: ubuntu-latest
    outputs:
      proceed: ${{ steps.commit-check.outputs.proceed }}
    steps:
      - name: Check if it's a plain commit
        id: commit-check
        uses: "Mad-Pixels/github-workflows/.github/actions/github-check-commit@main"
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}

      - name: Proceed only on plain commits
        if: steps.commit-check.outputs.proceed == 'true'
        run: echo "This is a normal commit — continue pipeline"
```

## 🔐 Required Secrets
| **Name**         | **Description**                             |
|------------------|---------------------------------------------|
| `GITHUB_TOKEN`   | Automatically provide gy GitHub (no action) |

## 📤 Outputs
| **Name**     | **Description**                                                         |
|--------------|-------------------------------------------------------------------------|
| `proceed`    | 'true' if this is a direct commit, 'false' if part of a PR merge/squash |
| `pr_numbers` | Comma-separated list of PR numbers touching the commit (if any)         |