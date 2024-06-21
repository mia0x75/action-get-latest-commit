### Get Latest Commit
==================

A simple Github action to get the latest commit from another repository. No authentication required.

### Configuration
=============

Example Repository - https://github.com/mia0x75/action-get-latest-release

**Inputs**

| Name  | Description                                              | Example                   |
| ----- | -------------------------------------------------------- | ------------------------- |
| owner | The Github user or organization that owns the repository | mia0x75                   |
| repo  | The repository name                                      | action-get-latest-release |

**or**
| Name       | Description                 | Example                           |
| ---------- | --------------------------- | --------------------------------- |
| repository | The repository name in full | mia0x75/action-get-latest-release |

**Additional Inputs (Optional)**
| Name   | Description                                  | Example                                                                 |
| ------ | -------------------------------------------- | ----------------------------------------------------------------------- |
| branch | Specify branch. If blank the default branch. | "main"                                                                  |
| token  | GitHub token or personal access token        | `${{ secrets.GITHUB_TOKEN }}` or `${{ secrets.PERSONAL_ACCESS_TOKEN }}` |

Using the `GITHUB_TOKEN` will avoid the action [failing due to hitting API rate limits](https://github.com/pozetroninc/github-action-get-latest-release/issues/24) from the IP address of the GitHub runner your action is running on. Using a `PERSONAL_ACCESS_TOKEN` is required to get the release information from a private repo. You can read about [how to create a personal access token here](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) and how to [add this as a repository secret here](https://docs.github.com/en/github/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets).

**Outputs**

| Name        | Description                  | Example                    |
| ----------- | ---------------------------- | -------------------------- |
| shorthash   | First 7 of the commit's hash | 0bd441a                    |
| hash        | Full sha hash for commit     | 0bd441a8a1d62dc22fb3704... |
| description | Commit message               | This is an example commit  |

### Usage Example

```yaml
name: Docker Build and Push

on:
  repository_dispatch:
    types: [build-docker-image]
  workflow_dispatch:

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - id: lastcommit
        uses: mia0x75/action-get-latest-commit@main
        with:
          owner: mia0x75
          repo: action-get-latest-commit
          branch: main

      - name: show commit data
        run: |
          echo "shorthash:   ${{ steps.lastcommit.outputs.shorthash }}"
          echo "hash:        ${{ steps.lastcommit.outputs.hash }}"
          echo "description: ${{ steps.lastcommit.outputs.description }}"
```

To use the current repo:

```yaml
with:
  repository: ${{ github.repository }}
```

To use authentication token:

```yaml
with:
  token: ${{ secrets.GITHUB_TOKEN }}
```
