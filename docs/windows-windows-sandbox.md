# Windows sandbox

Configure and troubleshoot the native Codex sandbox on Windows

## Overview

Use Codex on Windows with the native ChatGPT desktop app, the CLI, or the IDE extension. The ChatGPT desktop app supports core workflows including parallel tasks, worktrees, scheduled tasks, Git functionality, the built-in browser, file previews, plugins, and skills.

The app can run natively in PowerShell with a Windows sandbox instead of requiring WSL or a virtual machine. "This keeps Codex in Windows-native workflows while enforcing bounded filesystem and network permissions."

## Native Windows Sandbox Modes

The native Windows sandbox has two modes:

- natively on Windows with the stronger `elevated` sandbox
- natively on Windows with the fallback `unelevated` sandbox

## Configure the Windows Sandbox

When running Codex natively on Windows, agent mode uses a Windows sandbox to block filesystem writes outside the working folder and prevent network access without explicit approval.

Configure in `config.toml`:

```toml
[windows]
sandbox = "elevated" # or "unelevated"
```

**Elevated Mode**: The preferred native Windows sandbox using dedicated lower-privilege sandbox users, filesystem permission boundaries, firewall rules, and local policy changes for sandboxed commands.

**Unelevated Mode**: The fallback sandbox running commands with a restricted Windows token, applying ACL-based filesystem boundaries and environment-level offline controls. Weaker than elevated but useful when administrator setup is blocked.

Enterprise administrators can constrain sandbox implementations through `requirements.toml`:

```toml
[windows]
allowed_sandbox_implementations = ["elevated"]
```

By default, both modes use a private desktop for stronger UI isolation. Set `windows.sandbox_private_desktop = false` only for compatibility with older `Winsta0\\Default` behavior.

### Sandbox Permissions

"Running Codex in full access mode means Codex is not limited to your project directory and might perform unintentional destructive actions." For safer automation, maintain sandbox boundaries and use rules for specific exceptions.

### Windows Version Matrix

| Windows version | Support level | Notes |
|---|---|---|
| Windows 11 | Recommended | Best baseline for Codex on Windows |
| Recent, fully updated Windows 10 | Best effort | Can work but less reliable; version 1809+ required |
| Older Windows 10 builds | Not recommended | More likely to fail |

Environment assumptions:

- `winget` should be available
- The recommended native sandbox depends on administrator-approved setup
- Some enterprise-managed devices block required setup steps

### Grant Sandbox Read Access

When a command fails because the Windows sandbox cannot read a directory:

```
/sandbox-add-read-dir C:\absolute\directory\path
```

The path must be an existing absolute directory. After success, later commands can read that directory during the current session.

Use the native Windows sandbox by default. Choose WSL when you need Linux-native tooling, your workflow already lives in WSL2, or neither native Windows sandbox mode meets your needs.

## Troubleshooting and FAQ

### My native sandbox setup failed

Common causes:

- the Windows UAC or administrator prompt was declined
- the machine does not allow local user or group creation
- the machine does not allow firewall rule changes
- the machine blocks logon rights needed by sandbox users
- enterprise policy blocks part of the setup flow

**What to try:**

1. Try the `elevated` sandbox setup again and approve the administrator prompt
2. If blocked, ask your IT team about administrator-approved setup for local user/group creation and firewall configuration
3. If default setup fails, use the `unelevated` sandbox to continue working

### Codex switched me to the unelevated sandbox

This means Codex could not finish the stronger `elevated` sandbox setup.

- Codex still runs in sandboxed mode
- It applies ACL-based filesystem boundaries but without the separate sandbox-user boundary
- This is a useful fallback, not the preferred long-term enterprise configuration

The best long-term fix is usually getting the `elevated` sandbox working with IT team help.

### I see Windows error 1385

If sandboxed commands fail with error 1385, Windows is denying the logon type the sandbox user needs.

**What to do:**

1. Ask your IT team whether device policy grants required logon rights to Codex-created sandbox users
2. Compare group policy or OU differences if the issue affects only some machines
3. If you need to keep working, use the `unelevated` sandbox while investigating
4. Send `CODEX_HOME/.sandbox/sandbox.log` with Windows version and failure description

### Codex warns that some folders are writable by Everyone

Windows permissions on those folders are too broad for sandbox protection.

**What to do:**

1. Review the folders Codex lists in the warning
2. Remove `Everyone` write access from those folders if appropriate
3. Restart Codex or re-run sandbox setup after correcting permissions

### Sandboxed commands cannot reach the network

Some Codex tasks intentionally run without outbound network access depending on the permissions mode.

**If a task fails:**

1. Check whether the task was supposed to run with network disabled
2. If you expected network access, restart Codex and try again
3. If the issue persists, collect the sandbox log

### Sandboxing worked before and then stopped

This can happen after moving a repo, changing machine permissions, changing Windows policies, or other system configuration changes.

**What to try:**

1. Restart Codex
2. Try the `elevated` sandbox setup again
3. If that does not fix it, use the `unelevated` sandbox as a temporary fallback
4. Collect the sandbox log for review

### I need to send diagnostics to OpenAI

Send:

- `CODEX_HOME/.sandbox/sandbox.log`

Also helpful:

- short description of what you were trying to do
- whether the `elevated` sandbox failed or `unelevated` was used
- any error message shown in the app
- whether you saw 1385 or another Windows/PowerShell error
- whether you are on Windows 11 or Windows 10

Do not send:

- the contents of `CODEX_HOME/.sandbox-secrets/`

### The IDE extension is installed but unresponsive

Your system may be missing C++ development tools, which some native dependencies require:

- Visual Studio Build Tools (C++ workload)
- Microsoft Visual C++ Redistributable (x64)
- With `winget`, run: `winget install --id Microsoft.VisualStudio.2022.BuildTools -e`

Then fully restart VS Code after installation.
</content>
