# Admin Rollout Guide

Plan, configure, and verify a ChatGPT Enterprise rollout across workspace and developer surfaces.

## Overview

This guide addresses administration boundaries including workspace access, local runtime policy for covered capabilities in the ChatGPT desktop app, Codex CLI, and IDE extension, Codex cloud, Platform API access, plugins and apps, and permissions in connected systems.

Note: "Codex Local" in workspace settings is a grouping label for certain local access and access-token controls, not a separate product.

## Step 1: Assign Owners and Choose a Rollout

Assign an owner for each part:

- **Workspace access:** Membership, seats, roles, and supported features
- **Local runtime policy:** Approvals, permission profiles, filesystem and network access
- **Codex cloud:** Hosted environments, repository connections, and cloud runtime policy
- **Connected systems:** Provider-side installation, accounts, and permissions
- **Reporting and compliance:** Analytics access, audit exports, and data handling

Decide which audiences need covered local capabilities in desktop app, Codex CLI, IDE extension, Codex cloud, or combinations.

## Step 2: Configure Workspace Access and Identity

Use ChatGPT workspace membership, seats, groups, and RBAC permissions to grant intended audiences supported features. Verify local client and Codex cloud access against current workspace guidance.

Key resources:
- Manage members, seat types, roles, and access
- Configure role-based access control
- Manage workspace settings
- Groups and provisioning documentation
- Authentication documentation

Test sign-in and feature access with representative members before expanding.

## Step 3: Configure Local Runtime Requirements

Local requirements constrain runtime behavior when users start supported local runs. Deliver `requirements.toml` through supported cloud, device, or system channels.

Example configuration using permission profiles:

```toml
default_permissions = ":workspace"

[allowed_permission_profiles]
":read-only" = true
":workspace" = true
```

To disable Computer Use across browser and desktop surfaces:

```toml
[features]
browser_use = false
browser_use_full_cdp_access = false
browser_use_external = false
in_app_browser = false
computer_use = false
```

## Step 4: Standardize Repository Configuration

Use repository-scoped configuration to share project defaults, rules, and skills:

| Type | Source | Purpose |
|------|--------|---------|
| Configuration | Config basics | Set repository defaults for supported local clients |
| Rules | Rules documentation | Control commands requiring approval outside sandbox |
| Skills | Build skills | Make repository workflows available to clients |

Repository configuration supplies defaults and reusable workflows but cannot grant workspace, model, Platform API, or connected-system access.

## Step 5: Configure Codex Cloud

Plan each boundary:

1. Grant intended audience Codex cloud access through supported workspace controls
2. Install and configure the supported source-system integration
3. Limit repository access in the source system
4. Configure cloud environments, secrets, and internet access
5. Configure optional hosted workflows such as code review
6. Test with representative users having intended workspace and repository permissions

Codex cloud respects repository permissions and protections exposed by connected source systems. Workspace access doesn't bypass those controls.

## Step 6: Configure Plugins and Connected Capabilities

Review plugin installation, bundled skills, app-backed capabilities, app actions, and source-system authorization as separate decisions.

Before including a plugin or skill:

1. Confirm source, accountable owner, intended audience, and review date
2. Review bundled skills, apps, MCP servers, hooks, and data/actions each requires
3. Test with non-sensitive data and least required access
4. Record ownership of re-review and retirement

Plugins are available in ChatGPT Work on the web, ChatGPT Work and Codex in the desktop app, and through Codex CLI plugin browser. They're unavailable in Chat, the IDE extension, or mobile.

## Step 7: Set Up Governance and Observability

Choose the reporting surface matching your question:

- **Workspace analytics:** Interactive ChatGPT workspace and Codex analytics
- **Analytics API:** Programmatic, aggregated reporting through Codex Analytics API
- **Compliance API:** Audit and investigation records
- **Usage limits and spend controls:** When plan-dependent Codex activity consumes ChatGPT workspace credits

Protect the integration boundary:

- Store API keys in the organization's secret-management system
- Limit downstream system access to approved audiences
- Protect exported records according to sensitivity and retention policy

## Step 8: Verify and Maintain the Rollout

Verify every applicable boundary with representative identities:

- ChatGPT workspace membership, seat, and supported role permissions
- Covered local capabilities in desktop app, Codex CLI, and IDE extension
- Codex cloud access, environment configuration, and repository permissions
- Platform API organization and project access for API-key workflows
- Plugin installation, bundled skills, app access, and supported actions
- Connected-system authorization and data access
- Analytics and compliance access for responsible administrators

Record the owner and current procedural source for each control. After initial rollout, review access, connected capabilities, credit use, support feedback, and actual workflows. Adjust scope and guidance when signals change.
