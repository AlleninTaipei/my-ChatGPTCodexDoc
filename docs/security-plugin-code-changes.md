# Review code changes for security

Review pull requests and local changes for security regressions manually or in CI/CD.

## Overview

Use a security change review when you need evidence about regressions introduced by one Git-backed change set. The workflow reviews every changed source-like file and directly supporting code without turning the task into a general repository audit.

If you want to scan a full repository instead of a specific change, see [Run a security scan](/codex/security/plugin/scans).

## Run a manual review

For uncommitted changes, send:

```
Use $codex-security:security-diff-scan to review my current uncommitted changes for security regressions.
```

For a commit or branch range, identify both ends when needed:

```
Use $codex-security:security-diff-scan to review the changes from origin/main to HEAD for security regressions. Focus on authentication, authorization, input handling, filesystem access, network requests, and secrets.
```

You can also name a pull request when its base and head revisions are available in the local checkout.

## Confirm the change in setup

1. Confirm **Scan type** is `Changes`.
2. Confirm the checked-out **Codebase**, **Current branch**, and **Last commit**.
3. Under **Changes to review**, choose:
   - `Uncommitted changes` for the current working tree.
   - The latest commit for a single-commit review.
   - A base and head revision for a branch or pull-request range.
4. Confirm that the summary describes the change you intended to review.
5. Select **Start scan**.

The workflow doesn't check out another branch or change the selected working tree. If a requested revision isn't available locally, fetch it before the review or provide a locally available base and head.

## Act on findings

After reviewing the results, fix and verify an accepted finding or export and track findings.

## Automate reviews in CI/CD

Run the same `$codex-security:security-diff-scan` skill from CI when the runner can invoke the Codex CLI without interaction. First install the CLI and plugin without exposing the scan credential:

```bash
npm install --global @openai/codex
codex plugin add codex-security@openai-curated
```

Then expose an OpenAI API key from your CI secret store as `CODEX_SECURITY_API_KEY` only for the scan:

```bash
CODEX_API_KEY="$CODEX_SECURITY_API_KEY" codex exec \
  --sandbox workspace-write \
  "Use \$codex-security:security-diff-scan to review changes from $BASE_REVISION to $HEAD_REVISION for security regressions. Do not modify the checkout."
```

### Output files

The scan writes its output to `$TMPDIR/codex-security-scans/<repository>/<scan-id>/`:

| File | Contents |
|------|----------|
| `findings.json` | Findings with stable identifiers, severity, confidence, source locations, and remediation. Use it to create pull-request comments or feed downstream tools. |
| `scan-manifest.json` | Sealed scan receipt with the reviewed target, revisions, and artifact hashes. |
| `coverage.json` | Reviewed and deferred surfaces, exclusions, and coverage completeness. |

### Findings schema

The `findings.json` schema defines the complete structure with these key fields:

| Field | Type | Description |
|-------|------|-------------|
| `documentType` | String | Identifies the document as `codex-security.findings`. |
| `schemaVersion` | String | Identifies the findings schema version. |
| `scanId` | String | Identifies the scan that produced the findings. |
| `findings` | Array | Contains zero or more finding objects. |
| `findings[].findingId` | String | Stable finding identifier derived from the finding fingerprint. |
| `findings[].occurrenceId` | String | Identifies this occurrence of the finding in a specific scan. |
| `findings[].ruleId` | String | Identifies the vulnerability family. |
| `findings[].identity` | Object | Contains the semantic anchor and optional sibling-instance identifier. |
| `findings[].fingerprints` | Object | Contains the fingerprint algorithm and primary fingerprint. |
| `findings[].title` | String | Provides the short finding title. |
| `findings[].summary` | String | Summarizes the vulnerability and its impact. |
| `findings[].severity` | Object | Contains the severity level and optional scoring details. |
| `findings[].confidence` | Object | Contains the confidence level and rationale. |
| `findings[].taxonomy` | Object | Contains the vulnerability category and CWE identifiers. |
| `findings[].locations` | Array | Lists affected files, line numbers, and location roles. |
| `findings[].remediation` | String | Describes the recommended fix. |
| `findings[].provenance` | Object | Identifies the source of the finding. |

### Processing findings

For example, this command prints one tab-separated row per finding:

```bash
jq -r '
  .findings[] |
  [.findingId, .severity.level, .confidence.level, .locations[0].path, .locations[0].startLine, .title] |
  @tsv
' findings.json
```

### CI/CD pipeline examples

The documentation provides examples for GitHub Actions, GitLab CI/CD, Azure Pipelines, and Jenkins. These examples assume a trusted Linux runner with Node.js, npm, Git, Python 3, jq, and provider command-line tools.

For GitHub Actions, the pipeline checks out the pull request head, installs Codex Security, runs the scan, and comments on the pull request with findings.

For GitLab CI/CD, create masked `CODEX_SECURITY_API_KEY` and `GITLAB_TOKEN` variables with the GitLab token needing API access.

For Azure Pipelines, configure a **Build validation** branch policy to run the pipeline on pull requests.

For Jenkins, use credentials and conditional stages to run the scan only on pull requests from within the repository.

All examples skip forked pull requests and recommend archiving scan artifacts, starting with advisory results, and reviewing coverage before making the job a required check.
