# HIPAA Configuration Guide for Codex Local

## Overview

This guide helps organizations configure Codex Local (the app, IDE extension, and CLI) for workflows potentially involving protected health information (PHI). OpenAI supports HIPAA compliance through applicable Business Associate Agreements when using ChatGPT for Healthcare, ChatGPT for Clinicians, or Regulated workspaces.

## Key Responsibilities

**Shared Model**: OpenAI handles PHI received from Codex Local consistent with the BAA, while organizations bear responsibility for local workstation security, source repositories, local retention, MCP servers, Browser Use, Computer Use activity, and third-party service integrations.

**Your Obligations**: Organizations must secure local workstations, review HIPAA policies against final configurations, manage endpoint controls, and document data flows. This guidance is not legal advice.

## OpenAI's Security Framework

OpenAI maintains enterprise risk management with board-level governance, secure development practices including peer review and automated testing, vulnerability management programs, and comprehensive data protection controls including encryption in transit and at rest.

## Sign-In Configuration

Codex supports two authentication methods:

- **ChatGPT sign-in**: Follows workspace permissions and ChatGPT Enterprise settings
- **API key sign-in**: Follows OpenAI API organization settings (suitable for CI/CD workflows)

The BAA covers both methods when properly configured.

## Critical Configuration Controls

### Core Settings Table

| Control | Setting | Purpose |
|---------|---------|---------|
| Sign-in | ChatGPT for workspace PHI; API keys for approved workflows | Determines governance authority |
| Workspace pinning | `forced_chatgpt_workspace_id` | Keeps PHI workflows in approved workspace |
| Approvals | `allowed_approval_policies = ["on-request", "untrusted"]` | Requires review of higher-risk actions |
| Permissions | `default_permissions = ":workspace"` | Restricts device access |
| Web search | `allowed_web_search_modes = ["cached"]` | Limits external data exposure |
| Features | Disable `computer_use`, `browser_use`, `in_app_browser` | Reduces PHI exposure to external apps |
| MCP servers | Empty by default; allowlist approved servers | Controls third-party tool access |
| History | `persistence = "none"` when required | Manages local transcript retention |

## Starter Configuration

Organizations receive a starter `requirements.toml` that establishes baseline controls:

```toml
allowed_approval_policies = ["on-request", "untrusted"]
allowed_approvals_reviewers = ["user"]
allowed_web_search_modes = ["cached"]
default_permissions = ":workspace"

[allowed_permission_profiles]
":read-only" = true
":workspace" = true

[features]
computer_use = false
browser_use = false
browser_use_full_cdp_access = false
in_app_browser = false

[mcp_servers]
# None allowed by default
```

## Configuration Examples

### Google Drive App Integration

Organizations can enable Google Drive after confirming OAuth scopes, access controls, and third-party BAA status:

```toml
[features]
apps = true

[apps.google_drive]
enabled = true
destructive_enabled = false
default_tools_approval_mode = "prompt"

[apps.google_drive.tools."files/delete"]
enabled = false
```

### Local GitHub Usage

For repository operations, implement review checkpoints:

```toml
[rules]
prefix_rules = [
  { pattern = [{ token = "git" }, { any_of = ["push", "commit"] }], 
    decision = "prompt", justification = "Require review before changing repository history." }
]
```

## Managed Configuration Hierarchy

Codex applies settings in this precedence order:

1. Cloud-managed requirements (highest priority)
2. macOS MDM requirements
3. System `requirements.toml`
4. User `config.toml` (lowest priority)

Managed defaults allow user changes within a session but reset at startup. Use requirements for non-bypassable compliance controls.

## Rollout Steps

1. Select ChatGPT, API key, or hybrid sign-in method
2. Confirm BAA coverage with OpenAI for chosen sign-in paths
3. Enable Codex Local and establish RBAC groups
4. Deploy admin-enforced `requirements.toml` and managed defaults
5. Train users on approval workflows and sandbox boundaries
6. Review third-party apps for PHI compatibility and BAA status
7. Monitor through Compliance API exports, workspace analytics, and audit logs

## Third-Party Service Considerations

Organizations remain responsible for confirming that third-party destinations (Google Drive, GitHub, Microsoft SharePoint, etc.) have appropriate BAAs before PHI exchange. OpenAI's BAA covers OpenAI's handling only, not third-party recipients.

## Key References

- Codex authentication and managed configuration documentation
- Agent approvals and security guidelines
- Codex Model Context Protocol (MCP) setup
- ChatGPT Healthcare and Regulated Workspace functionality
- Business Associate Agreement terms for API services
