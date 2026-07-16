# Hooks

Run deterministic scripts during the Codex lifecycle

Hooks represent an extensibility framework for Codex that allows developers to inject custom scripts into the agentic loop. This enables functionality like sending conversations to custom logging systems, scanning for accidentally pasted API keys, automatically creating persistent memories from conversations, running validation checks at turn completion, and customizing prompts based on directory context.

## Runtime Behavior

Key operational characteristics include:

- Multiple matching hooks from different files all execute
- Command hooks for the same event launch concurrently, preventing one hook from blocking another
- Non-managed command hooks require review and trust before execution
- Turn-scoped hooks (`PreToolUse`, `PermissionRequest`, `PostToolUse`, `PreCompact`, `PostCompact`, `UserPromptSubmit`, `SubagentStop`, `Stop`) operate at turn scope
- Session-scoped hooks (`SessionStart`, `SubagentStart`) operate at thread or subagent-start scope

## Hook Discovery Locations

Codex discovers hooks in these locations:

- `~/.codex/hooks.json`
- `~/.codex/config.toml`
- `<repo>/.codex/hooks.json`
- `<repo>/.codex/config.toml`

Hooks can also be bundled with plugins. Multiple hook sources load together without replacement. Project-local hooks only load when the project's `.codex/` layer is trusted.

## Trust and Review

Before non-managed command hooks execute, Codex requires explicit review and trust. The system records trust against the hook's hash, so modified hooks require re-review. Use the `/hooks` CLI command to inspect sources, review changes, trust hooks, or disable individual hooks.

Managed hooks from system, MDM, cloud, or `requirements.toml` sources are automatically trusted by policy and cannot be disabled.

## Configuration Structure

Hooks organize into three levels:

1. Hook event (e.g., `PreToolUse`, `PostToolUse`, `Stop`)
2. Matcher group determining when the event matches
3. One or more hook handlers executing when matched

### JSON Example

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "startup|resume",
        "hooks": [
          {
            "type": "command",
            "command": "python3 ~/.codex/hooks/session_start.py",
            "statusMessage": "Loading session notes"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "/usr/bin/python3 \"$(git rev-parse --show-toplevel)/.codex/hooks/pre_tool_use_policy.py\"",
            "statusMessage": "Checking Bash command"
          }
        ]
      }
    ]
  }
}
```

### TOML Equivalent

```toml
[[hooks.PreToolUse]]
matcher = "^Bash$"

[[hooks.PreToolUse.hooks]]
type = "command"
command = '/usr/bin/python3 "$(git rev-parse --show-toplevel)/.codex/hooks/pre_tool_use_policy.py"'
timeout = 30
statusMessage = "Checking Bash command"
```

## Configuration Notes

- `timeout` is measured in seconds, defaulting to 600 if omitted
- `statusMessage` is optional
- `commandWindows` provides Windows-specific command overrides
- Only `type: "command"` handlers currently execute
- Commands run with the session's working directory as their context
- Repository-local hooks should resolve from git root for stability

## Disabling Hooks

To disable hooks in `config.toml`:

```toml
[features]
hooks = false
```

Administrators can enforce this setting through `requirements.toml`.

## Managed Hooks in requirements.toml

Enterprise administrators can define hooks within `requirements.toml` configuration:

```toml
allow_managed_hooks_only = true

[features]
hooks = true

[hooks]
managed_dir = "/enterprise/hooks"
windows_managed_dir = 'C:\enterprise\hooks'

[[hooks.PreToolUse]]
matcher = "^Bash$"

[[hooks.PreToolUse.hooks]]
type = "command"
command = "python3 /enterprise/hooks/pre_tool_use_policy.py"
command_windows = 'py -3 C:\enterprise\hooks\pre_tool_use_policy.py'
timeout = 30
statusMessage = "Checking managed Bash command"
```

Notes for managed hooks:

- `managed_dir` applies to macOS and Linux
- `windows_managed_dir` applies to Windows
- Codex doesn't distribute scripts; enterprise tooling must install and update them
- Managed commands should use absolute paths within configured directories
- Setting `allow_managed_hooks_only = true` skips non-managed hooks but loads managed ones

## Plugin-Bundled Hooks

When enabled, plugins can load lifecycle hooks alongside user and project hooks. By default, Codex searches for `hooks/hooks.json` in the plugin root. Plugin manifests can override this with a `hooks` entry in `.codex-plugin/plugin.json`:

```json
{
  "name": "repo-policy",
  "hooks": "./hooks/hooks.json"
}
```

Manifest paths must be relative and remain within the plugin root. Plugin hook commands receive environment variables including `PLUGIN_ROOT` and `PLUGIN_DATA`, plus `CLAUDE_PLUGIN_ROOT` and `CLAUDE_PLUGIN_DATA` for compatibility.

Plugin hooks use the same event schema as other hooks and require trust review before execution.

## Matcher Patterns

The `matcher` field uses regex to filter when hooks activate. Use `"*"`, `""`, or omit `matcher` to match all occurrences.

Supported events and their matcher targets:

| Event | Matcher Filters | Notes |
|-------|-----------------|-------|
| `PermissionRequest` | tool name | Includes `Bash`, `apply_patch`, MCP tools |
| `PostToolUse` | tool name | Includes `Bash`, `apply_patch`, MCP tools |
| `PostCompact` | compaction trigger | Values: `manual`, `auto` |
| `PreCompact` | compaction trigger | Values: `manual`, `auto` |
| `PreToolUse` | tool name | Includes `Bash`, `apply_patch`, MCP tools |
| `SessionStart` | start source | Values: `startup`, `resume`, `clear`, `compact` |
| `SubagentStart` | subagent type | Depends on subagent |
| `SubagentStop` | subagent type | Depends on subagent |
| `UserPromptSubmit` | not supported | Matcher ignored |
| `Stop` | not supported | Matcher ignored |

Example matcher patterns:
- `Bash`
- `^apply_patch$`
- `Edit|Write`
- `mcp__filesystem__read_file`
- `mcp__filesystem__.*`
- `startup|resume|clear|compact`

## Common Input Fields

Every command hook receives a JSON object on stdin with these shared fields:

| Field | Type | Meaning |
|-------|------|---------|
| `session_id` | string | Current Codex session identifier |
| `transcript_path` | string \| null | Path to session transcript, if available |
| `cwd` | string | Working directory for the session |
| `hook_event_name` | string | Current hook event identifier |
| `model` | string | Active model slug |
| `permission_mode` | string | Current mode: `default`, `acceptEdits`, `plan`, `dontAsk`, `bypassPermissions` |

Turn-scoped hooks additionally include `turn_id`. The `transcript_path` provides convenience access but isn't a stable interface for future versions.

## Common Output Fields

These events support standard JSON output: `SessionStart`, `PreCompact`, `PostCompact`, `UserPromptSubmit`, `SubagentStop`, `Stop`:

```json
{
  "continue": true,
  "stopReason": "optional",
  "systemMessage": "optional",
  "suppressOutput": false
}
```

| Field | Effect |
|-------|--------|
| `continue` | Setting to `false` marks the hook run as stopped |
| `stopReason` | Recorded as the reason for stopping |
| `systemMessage` | Surfaced as UI warning or event stream warning |
| `suppressOutput` | Parsed but not yet implemented |

Exiting with code `0` and no output indicates success.

## Hook Events

### SessionStart

Runs when a session begins. The `matcher` filters by `source` (startup, resume, clear, compact).

Input fields beyond common ones:

| Field | Type | Meaning |
|-------|------|---------|
| `source` | string | Session start mode |

Plain text output adds developer context. JSON output supports common fields plus:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "SessionStart",
    "additionalContext": "Load the workspace conventions before editing."
  }
}
```

### SubagentStart

Runs when a subagent begins. The `matcher` filters by `agent_type`.

Input fields:

| Field | Type | Meaning |
|-------|------|---------|
| `turn_id` | string | Active Codex turn identifier |
| `agent_id` | string | Subagent identifier |
| `agent_type` | string | Subagent type or profile |
| `permission_mode` | string | Current permission mode |

Plain text output adds subagent developer context. JSON supports:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "SubagentStart",
    "additionalContext": "Review the repository test conventions first."
  }
}
```

### PreToolUse

Intercepts Bash, file edits through `apply_patch`, and MCP tool calls before execution. The `matcher` filters by `tool_name`.

Input fields:

| Field | Type | Meaning |
|-------|------|---------|
| `turn_id` | string | Active Codex turn identifier |
| `tool_name` | string | Tool identifier |
| `tool_use_id` | string | Tool invocation identifier |
| `tool_input` | JSON value | Tool-specific parameters |

To deny a tool call, return:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "deny",
    "permissionDecisionReason": "Destructive command blocked by hook."
  }
}
```

To rewrite a tool call, return:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "updatedInput": {
      "command": "echo rewritten"
    }
  }
}
```

To add context without blocking:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "additionalContext": "The pending command touches generated files."
  }
}
```

### PermissionRequest

Runs when Codex needs approval (shell escalation, managed-network access). The `matcher` filters by `tool_name`.

Input fields:

| Field | Type | Meaning |
|-------|------|---------|
| `turn_id` | string | Active Codex turn identifier |
| `tool_name` | string | Tool identifier |
| `tool_input` | JSON value | Tool parameters |
| `tool_input.description` | string \| null | Human-readable approval reason |

To approve:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PermissionRequest",
    "decision": {
      "behavior": "allow"
    }
  }
}
```

To deny:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PermissionRequest",
    "decision": {
      "behavior": "deny",
      "message": "Blocked by repository policy."
    }
  }
}
```

If multiple hooks decide, any `deny` takes precedence. If no hook decides, normal approval flow continues.

### PostToolUse

Runs after Bash, `apply_patch`, and MCP tools produce output. The `matcher` filters by `tool_name`.

Input fields:

| Field | Type | Meaning |
|-------|------|---------|
| `turn_id` | string | Active Codex turn identifier |
| `tool_name` | string | Tool identifier |
| `tool_use_id` | string | Tool invocation identifier |
| `tool_input` | JSON value | Tool parameters |
| `tool_response` | JSON value | Tool output |

To provide feedback while blocking further processing:

```json
{
  "decision": "block",
  "reason": "The Bash output needs review before continuing.",
  "hookSpecificOutput": {
    "hookEventName": "PostToolUse",
    "additionalContext": "The command updated generated files."
  }
}
```

### PreCompact

Runs before conversation compaction. The `matcher` filters by `trigger` (manual, auto).

Input fields:

| Field | Type | Meaning |
|-------|------|---------|
| `turn_id` | string | Active Codex turn identifier |
| `trigger` | string | Compaction source |

JSON output supports common fields. Returning `continue: false` prevents compaction.

### PostCompact

Runs after conversation compaction. The `matcher` filters by `trigger` (manual, auto).

Input fields:

| Field | Type | Meaning |
|-------|------|---------|
| `turn_id` | string | Active Codex turn identifier |
| `trigger` | string | Compaction source |

JSON output supports common fields. Returning `continue: false` stops after compaction.

### UserPromptSubmit

Runs when a user submits a prompt. The `matcher` isn't used.

Input fields:

| Field | Type | Meaning |
|-------|------|---------|
| `turn_id` | string | Active Codex turn identifier |
| `prompt` | string | User prompt text |

Plain text output adds developer context. JSON supports common fields plus:

```json
{
  "hookSpecificOutput": {
    "hookEventName": "UserPromptSubmit",
    "additionalContext": "Ask for a clearer reproduction before editing files."
  }
}
```

To block:

```json
{
  "decision": "block",
  "reason": "Ask for confirmation before doing that."
}
```

### SubagentStop

Runs when a subagent stops. The `matcher` filters by `agent_type`.

Input fields:

| Field | Type | Meaning |
|-------|------|---------|
| `turn_id` | string | Active Codex turn identifier |
| `agent_id` | string | Subagent identifier |
| `agent_type` | string | Subagent type or profile |
| `agent_transcript_path` | string \| null | Path to subagent transcript |
| `stop_hook_active` | boolean | Whether already continued |
| `last_assistant_message` | string \| null | Latest subagent message |

Expects JSON output:

```json
{
  "decision": "block",
  "reason": "Run one more focused pass inside the subagent."
}
```

Any matching hook returning `continue: false` prevents other hooks from continuing the subagent.

### Stop

Runs when a turn completes. The `matcher` isn't used.

Input fields:

| Field | Type | Meaning |
|-------|------|---------|
| `turn_id` | string | Active Codex turn identifier |
| `stop_hook_active` | boolean | Whether already continued |
| `last_assistant_message` | string \| null | Latest message text |

Expects JSON output:

```json
{
  "decision": "block",
  "reason": "Run one more pass over the failing tests."
}
```

For this event, `decision: "block"` continues execution using the reason as an automatic continuation prompt. Any matching hook returning `continue: false` takes precedence.

## Schemas

The linked schemas on the main branch may include fields not in the current release. For exact current wire format, see generated schemas in the Codex GitHub repository.
