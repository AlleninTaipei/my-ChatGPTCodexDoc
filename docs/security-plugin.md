# Codex Security plugin quickstart

Codex Security is a security-review plugin for Codex that scans your code for vulnerabilities, validates plausible findings, and presents evidence and remediation guidance in a reviewable workspace. Use it to find security issues in code you own or have authorization to assess before they reach production.

This quickstart takes you through one recommended first run: an ordinary, read-only scan of a local repository in Codex.

This page covers the plugin that runs in a local Codex task. To scan a connected GitHub repository in Codex cloud, see [Codex Security cloud setup](/codex/security/setup).

## Install the plugin

1. Open the repository you want to assess in Codex in the ChatGPT desktop app.

2. Go to **Plugins** and search for **Codex Security**, or select the button below:

   [Install the Codex Security plugin](codex://plugins/install/codex-security?marketplace=openai-curated)

3. Start a new task in Codex for that repository (don't continue in a task that was already open).

Alternatively, using the Codex CLI:

1. In your terminal, go to the repository you want to assess and start Codex:

   ```
   codex
   ```

2. Enter `/plugins`, search for **Codex Security**, and select **Install plugin**.

3. Enter `/new` to start a new task for the repository.

## Run your first scan

For the best scan quality, use `gpt-5.6` with `high` or `xhigh` reasoning effort.

1. Ask for an ordinary scan

   Send this prompt in the new task:

   ```
   Run a Codex Security scan on this repository.
   ```

2. Confirm the setup

   Codex opens a setup workspace before it starts. For your first run, use these settings:

   - **Scan type:** `Codebase`
   - **Deep scan:** Off
   - **Scan area:** `Entire codebase`
   - **Threat model scoping guidance:** Leave blank unless you already know a specific attack vector or application area that deserves priority.

   Confirm that **Codebase**, **Current branch**, and **Last commit** identify the repository you intended to scan. Then select **Start scan**.

3. Let the scan finish

   The scan can take time. Keep the task running until the workspace reports completion. If Codex identifies a configuration limitation, review the exact limitation and proposed change before allowing it to update your configuration.

4. Review the result

   Use the UI to browse findings or open the generated report for a complete, portable review.

## What the scan creates

Every completed scan opens a findings workspace. Use it to review findings and coverage without inspecting raw artifacts. The scan also creates the files below:

- `report.md`, a complete portable report for sharing or archiving.
- Structured scan data in `scan-manifest.json`, `findings.json`, and `coverage.json` for automation and integrations. You normally don't need to open these files yourself.

## Choose your next workflow

- [Run a standard or scoped scan](/codex/security/plugin/scans) when you want to scan a repository or one folder with the default workflow.
- [Run a deep scan](/codex/security/plugin/deep-scans) when you need a more comprehensive scan and can wait longer for it to finish.
- [Review code changes](/codex/security/plugin/code-changes) when the target is a pull request, commit, branch range, or working-tree patch.
- [Triage a backlog](/codex/security/plugin/triage-backlog) when you have existing security findings to review.
- [Fix and verify a finding](/codex/security/plugin/fix-findings) after you accept one finding for remediation.
- [Export or track findings](/codex/security/plugin/export-findings) when you need JSON, CSV, SARIF, an approval-gated Linear, GitHub, or Jira issue, or a private draft GitHub Security Advisory.
