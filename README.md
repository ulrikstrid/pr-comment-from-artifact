# Post PR Comment from Artifact

This GitHub Action allows you to post a pull request comment from an artifact.

The artifact is downloaded, extracted and the first file in it is used for the comment. The file name is used as the pull request number (excluding the file extension), while the content of the file is used as the comment body.

"Upsert" mode is used for multiple comments to the same pull request (the comment is updated if it already exists).

Typically, this action is used in conjunction with a workflow that creates an artifact (like [live-codes/preview-in-livecodes](https://github.com/live-codes/preview-in-livecodes)), and is triggered by a successful workflow run (see usage below).

## Inputs

- `GITHUB_TOKEN`: Github token of the repository - (default: `${{ github.token }}` [automatically created by Github])
- `artifact`: Name of the artifact - (default: "pr")

## Outputs

- `id`: ID of the comment
- `body`: Body of the comment
- `html_url`: HTML URL of the comment

## Usage

```yaml
name: Comment on pull request

on:
  workflow_run:
    workflows: ["create-artifact"] # the workflow that created the artifact
    types:
      - completed

jobs:
  upload:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
    if: >
      github.event.workflow_run.event == 'pull_request' &&
      github.event.workflow_run.conclusion == 'success'

    steps:
      - name: Post comment
        uses: live-codes/pr-comment-from-artifact@v1
        with:
          GITHUB_TOKEN: ${{ github.token }}
```

## Credits

Based on:

- https://securitylab.github.com/research/github-actions-preventing-pwn-requests/
- https://github.com/thollander/actions-comment-pull-request

## License

MIT License
