# 🧬 Branch Validator
Verify that a tag or commit is reachable from a specified branch.

## ✅ Features
- Validates if a given commit or tag is part of the history of a target branch
- Defaults to checking `HEAD` against `main` branch
- Supports custom branch and tag inputs
- Returns a boolean output for easy conditional use in workflows

## 📖 Related Documentation
- [Git merge-base Documentation](https://git-scm.com/docs/git-merge-base)

## 🚀 Prerequisites
Your workflow must:
- Run on a runner with Git installed (`ubuntu-latest` is fine)
- Ensure the target branch exists in the remote repository
- Fetch full history (`fetch-depth: 0`) to allow ancestry checks

## 🔧 Quick Example
name: Validate Tag Origin

on:
  workflow_dispatch:
    inputs:
      tag:
        description: 'Tag to validate'
        required: true

jobs:
  validate-branch:
    runs-on: ubuntu-latest
    steps:
      - name: Check if tag is from main
        uses: Mad-Pixels/github-workflows/actions/branch-validator@v1
        with:
          target_branch: main
          tag_name: ${{ github.event.inputs.tag }}

## 📥 Inputs
| **Name**        | **Required** | **Description**                                                        | **Default** |
|-----------------|--------------|------------------------------------------------------------------------|-------------|
| `target_branch` | ❌ No        | Branch to validate against (e.g., `main`, `release/v1`)                | `main`      |
| `tag_name`      | ❌ No        | Tag name to validate; if empty, validates current `HEAD`               | ` `         |

## 📤 Outputs
| **Name**   | **Description**                                             |
|------------|-------------------------------------------------------------|
| `is_valid` | `true` if commit/tag is reachable from target branch, else `false` |

## 📋 Examples
[View example →](./examples/base.yml)