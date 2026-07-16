# Codex GitHub Action

## Overview

The Codex GitHub Action (`openai/codex-action@v1`) enables you to trigger Codex actions from GitHub Events by running Codex in CI/CD jobs, applying patches, or posting reviews from workflows.

## Use Cases

The action is useful when you want to:

- Automate Codex feedback on pull requests or releases without managing the CLI yourself
- Gate changes on Codex-driven quality checks as part of your CI pipeline
- Run repeatable Codex tasks (code review, release prep, migrations) from a workflow file

## Prerequisites

- Store your OpenAI key as a GitHub secret (for example `OPENAI_API_KEY`)
- Run the job on a Linux or macOS runner; for Windows, set `safety-strategy: unsafe`
- Check out your code before invoking the action
- Decide which prompts to run via `prompt` or `prompt-file`

## Example Workflow

```yaml
name: Codex pull request review
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  codex:
    runs-on: ubuntu-latest
    permissions:
      contents: read
    outputs:
      final_message: ${{ steps.run_codex.outputs.final-message }}
    steps:
      - uses: actions/checkout@v5
        with:
          ref: refs/pull/${{ github.event.pull_request.number }}/merge
          fetch-depth: 0
          persist-credentials: false

      - name: Run Codex
        id: run_codex
        uses: openai/codex-action@v1
        with:
          openai-api-key: ${{ secrets.OPENAI_API_KEY }}
          prompt-file: .github/codex/prompts/review.md
          output-file: codex-output.md

  post_feedback:
    runs-on: ubuntu-latest
    needs: codex
    if: needs.codex.outputs.final_message != ''
    permissions:
      issues: write
      pull-requests: write
    steps:
      - name: Post Codex feedback
        uses: actions/github-script@v7
        with:
          github-token: ${{ github.token }}
          script: |
            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.payload.pull_request.number,
              body: process.env.CODEX_FINAL_MESSAGE,
            });
        env:
          CODEX_FINAL_MESSAGE: ${{ needs.codex.outputs.final_message }}
```

## Configure `codex exec`

Key action inputs that map to `codex exec` options:

- `prompt` or `prompt-file`: Inline instructions or repository path to Markdown/text
- `codex-args`: Extra CLI flags as JSON array or shell string
- `model` and `effort`: Agent configuration settings
- `sandbox`: Sandbox mode (`workspace-write`, `read-only`, `danger-full-access`)
- `output-file`: Save final message to disk
- `codex-version`: Pin specific CLI release
- `codex-home`: Shared Codex home directory for configuration reuse

## Manage Privileges

Control Codex access with these inputs:

- `safety-strategy` (default `drop-sudo`): Removes `sudo` before running Codex
- `unprivileged-user`: Runs Codex as specific account with `safety-strategy: unprivileged-user`
- `read-only`: Prevents file changes and network access
- `sandbox`: Limits filesystem and network access within Codex
- `allow-users` and `allow-bots`: Restrict workflow triggers

## Capture Outputs

The action emits the last Codex message through `final-message` output. Combine `output-file` with artifact uploads for full transcripts, or use `--output-schema` for structured JSON data.

## Security Checklist

- Limit who can start the workflow using trusted events or explicit approvals
- Sanitize prompt inputs from pull requests and commit messages to avoid injection
- Protect `OPENAI_API_KEY` by keeping safety-strategy on `drop-sudo` or using unprivileged user
- Run Codex as the last step so later steps don't inherit unexpected changes
- Rotate keys immediately if secret exposure is suspected

## Troubleshooting

- Both prompt and prompt-file set: Remove the duplicate input
- responses-api-proxy didn't write server info: Confirm API key validity
- Unexpected `sudo` behavior: Verify runner OS is Linux/macOS
- Permission errors after `drop-sudo`: Grant write access before action runs
- Unauthorized trigger blocked: Adjust `allow-users` or `allow-bots` inputs
