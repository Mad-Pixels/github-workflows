# 🏷️ Tag Creator
Create and manage Git tags with validation.

## ✅ Features
- Validates tag format using customizable regex
- Supports annotated and lightweight tags
- Can overwrite existing tags with `force: true`
- Creates tags from any branch
- Optional custom tag message
- Outputs tag SHA, existence flag, and URL
- Verifies remote tag after push

## 📖 Related Documentation
- [Git Tag Documentation](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

## 🚀 Prerequisites
Your workflow must:
- Run on a runner with Git installed (default `ubuntu-latest` meets this)
- Provide a token with `contents: write` permission to push tags
- Ensure the branch to tag from exists in the remote repository

## 🔧 Quick Example
```yaml
name: Create Release Tag

on:
  workflow_dispatch:
    inputs:
      tag:
        description: 'Tag to create (e.g., v1.0.0)'
        required: true

jobs:
  tag:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Create Git Tag
        uses: Mad-Pixels/github-workflows/actions/tag-creator@v1
        with:
          tag: ${{ github.event.inputs.tag }}
          token: ${{ secrets.GITHUB_TOKEN }}
          message: "Release ${{ github.event.inputs.tag }}"
```

## 📥 Inputs
| **Name**       | **Required** | **Description**                                                           | **Default** |
|----------------|--------------|---------------------------------------------------------------------------|-------------|
| `tag`          | ✅ Yes       | Tag to create (e.g., v1.0.0)                                              | -           |
| `token`        | ✅ Yes       | GitHub token or PAT with `contents: write` permissions                    | -           |
| `force`        | ❌ No        | Overwrite existing tag if it exists (`true`/`false`)                      | `false`     |
| `branch`       | ❌ No        | Branch to tag from                                                        | `main`      |
| `tag_format`   | ❌ No        | Regex to validate tag format                                              | `^v[0-9]+\.[0-9]+\.[0-9]+(-.*)?$` |
| `message`      | ❌ No        | Message for annotated tag (ignored for lightweight tags)                  | -           |
| `lightweight`  | ❌ No        | Create lightweight tag (overrides message) (`true`/`false`)               | `false`     |

## 📤 Outputs
| **Name**     | **Description**                                  |
|--------------|--------------------------------------------------|
| `tag_sha`    | SHA of the commit the tag points to              |
| `tag_exists` | Whether the tag already existed before creation  |
| `tag_url`    | GitHub URL to view the created tag               |

## 📋 Examples
[View example →](./examples/base.yml)