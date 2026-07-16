# Sandbox

## How sandboxing works across ChatGPT and Codex clients

The sandbox creates boundaries enabling autonomous agent operation without unrestricted machine access. When running in ChatGPT desktop, Codex CLI, or IDE extensions, commands execute in constrained environments rather than with full default permissions.

This environment defines agent capabilities, which files it can modify and network access permissions. Tasks staying within boundaries proceed autonomously; crossing them triggers approval flows.

## What the sandbox does

The sandbox applies to spawned commands, not just file operations. Tools like `git`, package managers, and test runners inherit sandbox boundaries.

Codex employs platform-native enforcement across macOS, Linux, WSL2, and Windows, implementing the same principle differently per OS: providing bounded workspaces for routine autonomous task execution within clear limits.

## Why it matters

Sandboxing reduces approval fatigue by allowing routine file reading, editing, and command execution within pre-approved boundaries without constant confirmation requests.

It clarifies your trust model: you trust both agent intentions and enforced operational limits, enabling independent work while knowing escalation points.

## Getting started

Default permissions modes apply sandboxing automatically.

### Prerequisites

macOS: Sandboxing works natively via Seatbelt framework.

Windows: Uses native Windows sandbox in PowerShell; Linux sandbox in WSL2.

Linux/WSL2: Install `bubblewrap`:

```bash
sudo apt install bubblewrap
```

or

```bash
sudo dnf install bubblewrap
```

Codex locates the first `bwrap` executable on `PATH`. Without it, Codex falls back to bundled helpers requiring unprivileged user namespace support.

Ubuntu AppArmor note: On Ubuntu 25.04, installing `bubblewrap` from Ubuntu's repository works without additional setup. For Ubuntu 24.04:

```bash
sudo apt update
sudo apt install apparmor-profiles apparmor-utils
sudo install -m 0644 \
  /usr/share/apparmor/extra-profiles/bwrap-userns-restrict \
  /etc/apparmor.d/bwrap-userns-restrict
sudo apparmor_parser -r /etc/apparmor.d/bwrap-userns-restrict
```

Or disable the restriction:

```bash
sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=0
```

## How permissions work

Use surface-specific permissions controls to adjust Codex handling of local actions.

Approvals determine when pauses occur; sandboxes determine file and network resource access. Choose narrowest approval scopes needed for task continuation.

ChatGPT Work executes code in managed, isolated environments with workspace policy and tool-specific controls determining capability availability. Use Settings > Data controls > Work network access to manage network permissions. Enable Allow public internet access for public internet reach; disabled restricts to managed allowlists.

Web search, apps/connectors, and remote browsers have separate controls. Changes take effect after current execution completes and environment refresh.

ChatGPT desktop app uses permissions controls beneath the composer, potentially including Ask for approval, Approve for me, Full access, and custom profiles.

CLI: Enter `/permissions` to access the permissions picker.

IDE extension: Uses permissions control beneath the composer.

### Permissions profiles

Ask for approval: Agent reads/edits workspace files and runs routine commands; requests approval for internet or workspace boundary crossing.

- Sandbox: `workspace-write`
- Approvals policy: `on-request`
- Reviewer: `user`

## Configure defaults

Set defaults in `config.toml` using sandbox modes:

- `read-only`: Agent inspects files; can't edit without approval
- `workspace-write`: Agent reads, edits within workspace, runs routine local commands (default)
- `danger-full-access`: No sandbox restrictions

Common approval policies:

- `untrusted`: Agent asks before running non-trusted commands
- `on-request`: Agent works inside sandbox; asks when exceeding boundaries (default)
- `never`: No approval stops

When interactive, choose reviewers via `approvals_reviewer`:

- `user`: Prompts surface to user (default)
- `auto_review`: Eligible prompts go to reviewer agent

Full access combines `sandbox_mode = "danger-full-access"` with `approval_policy = "never"`. Lower-risk automation uses `sandbox_mode = "workspace-write"` with `approval_policy = "on-request"`.

Extend writable locations via `writable_roots` without removing sandboxing entirely. Use rules (/codex/agent-configuration/rules) for specific exceptions.

Automatic review doesn't alter sandbox boundaries; it's one `approvals_reviewer` option for requests like sandbox escalations or blocked network access.
