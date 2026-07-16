# Permissions

Configure beta Codex permission profiles for filesystem and network access

## Overview

Permission profiles are under active development and may change. Permission profiles do not compose with older sandbox settings, configure either `default_permissions` and `[permissions]`, or `sandbox_mode`/`sandbox_workspace_write`, but not both.

Permission profiles let you apply least-privilege boundaries to local commands Codex runs on your behalf. A profile combines filesystem rules (defining read/write access) with network rules (defining destination access).

## Built-in Profiles

Codex includes three built-in permission profiles:

- `:read-only` keeps local command execution read-only
- `:workspace` allows writes inside active workspace roots and system temp directories
- `:danger-full-access` removes local sandbox restrictions

## Define and Select a Profile

Create named profiles under `[permissions.<name>]`, then set `default_permissions` to that profile name. Example:

```toml
default_permissions = "project-edit"

[permissions.project-edit.workspace_roots]
"~/code/app" = true
"~/code/shared-lib" = true

[permissions.project-edit.filesystem]
":minimal" = "read"

[permissions.project-edit.filesystem.":workspace_roots"]
"." = "write"
".devcontainer" = "read"
"**/*.env" = "deny"

[permissions.project-edit.network]
enabled = true

[permissions.project-edit.network.domains]
"api.openai.com" = "allow"
"objects.githubusercontent.com" = "allow"
"*.github.com" = "allow"
"tracking.example.com" = "deny"
```

## Extend a Profile

Use `extends` when a profile resembles a built-in or another named profile:

```toml
default_permissions = "project-edit"

[permissions.project-edit]
description = "Project editing with OpenAI API access."
extends = ":workspace"

[permissions.project-edit.filesystem.":workspace_roots"]
"**/*.env" = "deny"

[permissions.project-edit.network]
enabled = true

[permissions.project-edit.network.domains]
"api.openai.com" = "allow"
```

A profile cannot extend `:danger-full-access`; Codex rejects unknown parents and inheritance cycles.

## Configuration Specification

| Entry | Type/Values | Default | Details |
|-------|------------|---------|---------|
| `default_permissions` | String profile name | None | Names the profile Codex applies by default |
| `[permissions.<name>]` | Table | None | Defines a named profile |
| `permissions.<name>.description` | String | None | Human-readable profile description |
| `permissions.<name>.extends` | String profile name | None | Starts from another named or built-in profile |
| `[permissions.<name>.workspace_roots]` | Table | None | Adds profile-defined workspace roots |
| `permissions.<name>.workspace_roots."<path>"` | Boolean | false | Adds path to workspace root set when true |
| `[permissions.<name>.filesystem]` | Table | None | Maps filesystem paths to access values |
| `permissions.<name>.filesystem.glob_scan_max_depth` | Number | None | Limits deny-read glob expansion |
| `[permissions.<name>.filesystem]."<path>"` | read/write/deny | None | Grants direct access for supported paths |
| `[permissions.<name>.network]` | Table | None | Configures network sandbox proxy and policy |
| `permissions.<name>.network.enabled` | Boolean | false | Enables network access for sandboxed commands |
| `[permissions.<name>.network.domains]` | Table | None | Maps host patterns to allow or deny |
| `permissions.<name>.network.proxy_url` | URL string | http://127.0.0.1:3128 | HTTP proxy listener address |
| `permissions.<name>.network.enable_socks5` | Boolean | true | Enables SOCKS5 listener |
| `permissions.<name>.network.socks_url` | URL string | http://127.0.0.1:8081 | SOCKS5 listener address |
| `permissions.<name>.network.allow_local_binding` | Boolean | false | Allows access to local/private-network targets |

## Filesystem Permissions

Access levels:

- read: Allows reading files and listing directories; prevents creation, modification, deletion
- write: Allows reading and modifying files, including creation and deletion
- deny: Denies both reads and writes

Specificity rules: More specific entries override broader entries. When entries target the same path, `deny` takes precedence over `write`, and `write` over `read`.

Example showing precedence:

```toml
[permissions.project-edit.filesystem]
":minimal" = "read"

[permissions.project-edit.filesystem.":workspace_roots"]
"." = "write"
".devcontainer" = "read"
"**/*.env" = "deny"
```

### Supported Path Forms

| Path | Meaning |
|------|---------|
| `:root` | Filesystem root |
| `:minimal` | Platform and runtime paths needed by common tools |
| `:workspace_roots` | Current session workspace roots plus profile-defined roots |
| `:tmpdir` | `$TMPDIR` location |
| `:slash_tmp` | `/tmp` folder |
| `/absolute/path` | Platform absolute path |
| `~/path` | Home directory relative path |

### Deny Reads with Globs

Use `deny` for sensitive files even when broader rules grant access:

```toml
[permissions.project-edit.filesystem]
glob_scan_max_depth = 3

[permissions.project-edit.filesystem.":workspace_roots"]
"**/*.env" = "deny"
```

On Linux, WSL, and native Windows, an unbounded `**` deny-read pattern may need bounded pre-expansion before the sandbox starts.

## Network Permissions

Enable network access with domain allowlisting:

```toml
[permissions.project-edit.network]
enabled = true

[permissions.project-edit.network.domains]
"example.com" = "allow"
"*.example.com" = "allow"
"**.example.com" = "allow"
"ads.example.com" = "deny"
```

### Local and Private Networks

Codex applies a local/private-network guard by default as a defense against DNS rebinding and accidental access to local services.

Allowlist exact local targets:

```toml
[permissions.project-edit.network.domains]
"localhost" = "allow"
"127.0.0.1" = "allow"
```

Set `allow_local_binding = true` when profiles must reach allowlisted hostnames resolving to local addresses.

### Unix Sockets

For tools like Docker:

```toml
[permissions.project-edit.network.unix_sockets]
"/var/run/docker.sock" = "allow"
"/tmp/old.sock" = "deny"
```

## Migrate from Older Sandbox Settings

Permission profiles replace the older `sandbox_mode` and `sandbox_workspace_write` combination. Use one system or the other, not both.

Suggested starting points:

- Read-only: Use `:read-only` or custom profile with read access only
- Workspace editing: Use `:workspace` or custom profile writing through `:workspace_roots`
- Unrestricted execution: Use `:danger-full-access` only when intentional

## Scope and Enforcement

### What Profiles Control

- Local command execution: Governs sandboxed commands running on your machine
- Filesystem writes: Write-capable profiles enable persistent changes
- Outbound destinations: Network domain rules constrain sandboxed command traffic destinations
- Local services: Local/private targets blocked by default

### How Enforcement Works

- macOS: Uses Seatbelt sandbox profiles; refuses commands if policies cannot be enforced
- Linux/WSL: Uses bubblewrap and seccomp with Landlock compatibility fallback
- Native Windows: Elevated sandboxing uses privilege boundaries and firewall rules; unelevated is weaker

## Common Profiles

### Read-only with Network Allowlist

```toml
default_permissions = "readonly-net"

[permissions.readonly-net.filesystem]
":minimal" = "read"

[permissions.readonly-net.filesystem.":workspace_roots"]
"." = "read"

[permissions.readonly-net.network]
enabled = true

[permissions.readonly-net.network.domains]
"api.openai.com" = "allow"
```

### File Access Limited to Workspace

```toml
default_permissions = "workspace-only"

[permissions.workspace-only]
extends = ":workspace"

[permissions.workspace-only.filesystem]
":root" = "deny"
":minimal" = "read"
":tmpdir" = "deny"
":slash_tmp" = "deny"
```

### Workspace Write Without Network

```toml
default_permissions = "project-edit"

[permissions.project-edit.filesystem]
":minimal" = "read"

[permissions.project-edit.filesystem.":workspace_roots"]
"." = "write"

[permissions.project-edit.network]
enabled = false
```

### Workspace Write with Public Web Access

```toml
default_permissions = "workspace-net"

[permissions.workspace-net.filesystem]
":minimal" = "read"

[permissions.workspace-net.filesystem.":workspace_roots"]
"." = "write"

[permissions.workspace-net.network]
enabled = true

[permissions.workspace-net.network.domains]
"*" = "allow"
```
