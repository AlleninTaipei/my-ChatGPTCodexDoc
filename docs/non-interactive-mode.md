# Non-interactive mode

## Use `codex exec` to run Codex in scripts and CI

Non-interactive mode enables running Codex from scripts without opening the interactive TUI through the `codex exec` command.

## When to use `codex exec`

Use `codex exec` when you need Codex to:

- Run as part of a pipeline (CI, pre-merge checks, scheduled jobs)
- Produce output that pipes into other tools (for generating release notes or summaries)
- Fit naturally into CLI workflows that chain command output into Codex
- Run with explicit, pre-set sandbox and approval settings

## Basic usage

Pass a task prompt as a single argument:

```
codex exec "summarize the repository structure and list the top 5 risky areas"
```

Codex streams progress to `stderr` and prints only the final agent message to `stdout`, enabling straightforward redirection or piping:

```
codex exec "generate release notes for the last 10 commits" | tee release-notes.md
```

Use `--ephemeral` to avoid persisting session rollout files to disk:

```
codex exec --ephemeral "triage this repository and suggest next steps"
```

When stdin is piped with a prompt argument, Codex treats the prompt as instruction and piped content as additional context:

```
curl -s https://jsonplaceholder.typicode.com/comments \
  | codex exec "format the top 20 items into a markdown table" \
  > table.md
```

## Permissions and safety

By default, `codex exec` runs in a read-only sandbox. Set the least permissions needed:

- Allow edits: `codex exec --sandbox workspace-write "<task>"`
- Allow broader access: `codex exec --sandbox danger-full-access "<task>"`

Use `danger-full-access` only in controlled environments like isolated CI runners or containers.

The `--full-auto` flag is deprecated; prefer explicit `--sandbox workspace-write` in new scripts.

Use `--ignore-user-config` when a run shouldn't load `$CODEX_HOME/config.toml`, and `--ignore-rules` to skip user and project execpolicy `.rules` files for controlled automation.

If an MCP server is configured with `required = true` and fails to initialize, `codex exec` exits with an error.

## Make output machine-readable

To consume Codex output in scripts, use JSON Lines output:

```
codex exec --json "summarize the repo structure" | jq
```

When `--json` is enabled, `stdout` becomes a JSON Lines stream capturing every event Codex emits. Event types include `thread.started`, `turn.started`, `turn.completed`, `turn.failed`, `item.*`, and `error`.

Sample JSON stream (each line is a JSON object):

```
{"type":"thread.started","thread_id":"0199a213-81c0-7800-8aa1-bbab2a035a53"}
{"type":"turn.started"}
{"type":"item.started","item":{"id":"item_1","type":"command_execution","command":"bash -lc ls","status":"in_progress"}}
{"type":"item.completed","item":{"id":"item_3","type":"agent_message","text":"Repo contains docs, sdk, and examples directories."}}
{"type":"turn.completed","usage":{"input_tokens":24763,"cached_input_tokens":24448,"output_tokens":122,"reasoning_output_tokens":0}}
```

For only the final message, use `-o <path>`/`--output-last-message <path>` to write to a file while printing to `stdout`.

## Create structured outputs with a schema

Use `--output-schema` to request final responses conforming to a JSON Schema. This supports automated workflows needing stable fields like job summaries, risk reports, or release metadata.

`schema.json`:

```json
{
  "type": "object",
  "properties": {
    "project_name": { "type": "string" },
    "programming_languages": {
      "type": "array",
      "items": { "type": "string" }
    }
  },
  "required": ["project_name", "programming_languages"],
  "additionalProperties": false
}
```

Run Codex with the schema and write the final JSON response to disk:

```
codex exec "Extract project metadata" \
  --output-schema ./schema.json \
  -o ./project-metadata.json
```

Example final output (stdout):

```json
{
  "project_name": "Codex CLI",
  "programming_languages": ["Rust", "TypeScript", "Shell"]
}
```

## Authenticate in automation

`codex exec` reuses saved CLI authentication by default. In CI, provide credentials explicitly.

### Use API key auth

For GitHub Actions, use the Codex GitHub Action instead of installing and authenticating the CLI directly. This reduces API key exposure by installing Codex, starting a Responses API proxy, and running Codex with configurable safety strategy.

Do not set `OPENAI_API_KEY` or `CODEX_API_KEY` as job-level environment variables in workflows checking out or running repository-controlled code. Build scripts, tests, dependency lifecycle hooks, or compromised actions can read environment variables.

For other automation environments, set `CODEX_API_KEY` only for the single `codex exec` invocation:

```
CODEX_API_KEY=<api-key> codex exec --json "triage open bug reports"
```

`CODEX_API_KEY` is only supported in `codex exec`.

### Use ChatGPT-managed auth in CI/CD (advanced)

API keys are the default for automation. Use ChatGPT-managed auth only when running CI/CD jobs with a Codex user account instead of an API key.

Treat `~/.codex/auth.json` as sensitive: it contains access tokens. Don't commit, share, or expose it.

## Resume a non-interactive session

Continue previous runs using the `resume` subcommand:

```
codex exec "review the change for race conditions"
codex exec resume --last "fix the race conditions you found"
```

Target specific session IDs with `codex exec resume <SESSION_ID>`.

## Git repository required

Codex requires running inside a Git repository to prevent destructive changes. Override with `codex exec --skip-git-repo-check` if the environment is safe.

## Common automation patterns

### Example: Autofix CI failures in GitHub Actions

For GitHub Actions, use `openai/codex-action` (https://github.com/openai/codex-action) instead of installing Codex and passing the API key directly. The action secures the API key with a proxy.

This pattern automatically proposes fixes when CI fails:

1. Trigger a follow-up workflow when main CI completes with error
2. Check out the failing commit with read-only repository access
3. Run setup commands before Codex without exposing the API key
4. Run the Codex GitHub Action
5. Save Codex's local changes as a patch artifact
6. In a separate job, apply the patch and open a pull request

The Codex job below has only `contents: read`. After Codex runs, it serializes only the diff as an artifact. The `open_pr` job receives repository write permissions but not `OPENAI_API_KEY`.

The example assumes a Node.js project; adjust setup and test commands for your stack.

```yaml
name: Codex auto-fix on CI failure

on:
  workflow_run:
    workflows: ["CI"]
    types: [completed]

jobs:
  generate_fix:
    if: ${{ github.event.workflow_run.conclusion == 'failure' }}
    runs-on: ubuntu-latest
    permissions:
      contents: read
    outputs:
      has_patch: ${{ steps.diff.outputs.has_patch }}
    steps:
      - uses: actions/checkout@v5
        with:
          ref: ${{ github.event.workflow_run.head_sha }}
          fetch-depth: 0
          persist-credentials: false

      - uses: actions/setup-node@v4
        with:
          node-version: "20"

      - name: Install dependencies
        run: |
          if [ -f package-lock.json ]; then npm ci; fi

      - name: Run Codex
        uses: openai/codex-action@v1
        with:
          openai-api-key: ${{ secrets.OPENAI_API_KEY }}
          prompt: |
            The CI workflow "${{ github.event.workflow_run.name }}" failed for commit
            ${{ github.event.workflow_run.head_sha }}.

            Run `npm test --silent` to reproduce the failure. Identify the minimal
            change needed to make the tests pass, implement only that change, and
            run `npm test --silent` again.

            Do not refactor unrelated files.

      - name: Create patch artifact
        id: diff
        run: |
          git add -N .
          git diff --binary HEAD > codex.patch
          if [ -s codex.patch ]; then
            echo "has_patch=true" >> "$GITHUB_OUTPUT"
          else
            echo "has_patch=false" >> "$GITHUB_OUTPUT"
          fi

      - name: Upload patch artifact
        if: steps.diff.outputs.has_patch == 'true'
        uses: actions/upload-artifact@v4
        with:
          name: codex-fix-patch
          path: codex.patch
          if-no-files-found: error

  open_pr:
    runs-on: ubuntu-latest
    needs: generate_fix
    if: needs.generate_fix.outputs.has_patch == 'true'
    permissions:
      contents: write
      pull-requests: write
    steps:
      - uses: actions/checkout@v5
        with:
          ref: ${{ github.event.workflow_run.head_sha }}
          fetch-depth: 0

      - uses: actions/download-artifact@v4
        with:
          name: codex-fix-patch

      - name: Apply Codex patch
        run: git apply --index codex.patch

      - name: Open pull request
        env:
          GH_TOKEN: ${{ github.token }}
          FAILED_HEAD_BRANCH: ${{ github.event.workflow_run.head_branch }}
          FAILED_HEAD_SHA: ${{ github.event.workflow_run.head_sha }}
          RUN_ID: ${{ github.event.workflow_run.run_id }}
        run: |
          branch="codex/auto-fix-$RUN_ID"

          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git switch -c "$branch"
          git commit -m "Auto-fix failing CI via Codex"
          git push origin "$branch"

          {
            echo "Codex generated this patch after CI failed for \`$FAILED_HEAD_SHA\`."
            echo
            echo "Review the changes before merging."
          } > pr-body.md

          gh pr create \
            --base "$FAILED_HEAD_BRANCH" \
            --head "$branch" \
            --title "Auto-fix failing CI via Codex" \
            --body-file pr-body.md
```

## Advanced stdin piping

Choose the stdin pattern based on where the instruction comes from. Use prompt-plus-stdin when you already know the instruction and want piped output as context. Use `codex exec -` when stdin should become the full prompt.

### Use prompt-plus-stdin

This pattern is useful when another command produces data for Codex to inspect. Write the instruction and pipe output as context, fitting naturally into CLI workflows with command output, logs, and generated data.

```
npm test 2>&1 \
  | codex exec "summarize the failing tests and propose the smallest likely fix" \
  | tee test-summary.md
```

More prompt-plus-stdin examples:

Summarize logs:

```
tail -n 200 app.log \
  | codex exec "identify the likely root cause, cite the most important errors, and suggest the next three debugging steps" \
  > log-triage.md
```

Inspect TLS or HTTP issues:

```
curl -vv https://api.example.com/health 2>&1 \
  | codex exec "explain the TLS or HTTP failure and suggest the most likely fix" \
  > tls-debug.md
```

Prepare a Slack-ready update:

```
gh run view 123456 --log \
  | codex exec "write a concise Slack-ready update on the CI failure, including the likely cause and next step" \
  | pbcopy
```

Draft a pull request comment from CI logs:

```
gh run view 123456 --log \
  | codex exec "summarize the failure in 5 bullets for the pull request thread" \
  | gh pr comment 789 --body-file -
```

### Use `codex exec -` when stdin is the prompt

Omit the prompt argument to read from stdin. Use `codex exec -` to force this behavior explicitly.

The `-` sentinel is useful when another command generates the entire prompt dynamically, good for storing prompts in files, assembling them with shell scripts, or combining live command output with instructions before sending to Codex.

```
cat prompt.txt | codex exec -

printf "Summarize this error log in 3 bullets:\n\n%s\n" "$(tail -n 200 app.log)" \
  | codex exec -

generate_prompt.sh | codex exec - --json > result.jsonl
```
