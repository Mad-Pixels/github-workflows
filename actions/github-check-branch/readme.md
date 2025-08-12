# 🧬 Branch Validator
Verify that a tag or commit is reachable from a specified branch.

## ✅ Features
- Checks if a commit/tag/HEAD is in the history of a target branch
- Supports explicit commit SHA (`commit_sha`) and tag (`tag_name`)
- Optional soft mode via `fail_on_invalid: 'false'`

## 📖 Related Documentation
- [Git merge-base Documentation](https://git-scm.com/docs/git-merge-base)

## 🚀 Prerequisites
Your workflow must:
- Run on a runner with Git installed (default `ubuntu-latest` meets this)

## 🔧 Quick Example
```yaml
name: Validate Tag Origin

on:
  workflow_dispatch:
    inputs:
      tag:
        description: 'Tag to validate'
        required: true
        type: string

jobs:
  validate-branch:
    runs-on: ubuntu-latest
    steps:
      - name: Check if tag is from main
        uses: Mad-Pixels/github-workflows/actions/branch-validator@v1
        with:
          target_branch: main
          tag_name: ${{ github.event.inputs.tag }}
```

## 📥 Inputs
|**Name**|**Required**|**Description**|**Default**|
|---|---|---|---|
|`target_branch`|❌ No|Branch to validate against (e.g., `main`, `release/v1`)|`main`|
|`tag_name`|❌ No|Tag name to validate; if empty, validates current `HEAD`|` `|
|`commit_sha`|❌ No|Explicit commit SHA to validate (overrides `tag_name/HEAD`)|` `|
|`fail_on_invalid`|❌ No|Fail the action if not reachable ('true'/'false')|` `|
|`show_summary`|❌ No|Print summary with task output in job summary|`true`|
|`summary_limit`|❌ No|Max number of output lines to show in summary|`250`|

## 📤 Outputs
|**Name**|**Description**|
|---|---|
|`is_valid`|`true` if commit/tag is reachable from target branch, else `false`|
|`commit`|The validated commit SHA|
|`subject`|What was validated (`HEAD:<sha>`, `tag:<name>`, or `commit:<sha>`)|
|`target_branch`|The branch used for validation|
|`merge_base`|Common ancestor SHA (only set when validation fails and histories intersect)|

## 📋 Examples
[View example →](./examples/base.yml)

