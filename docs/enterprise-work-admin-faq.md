# ChatGPT Work Admin FAQ

## Overview

ChatGPT Work enables users to delegate longer, multi-step tasks to the AI system. It gathers information from conversations, files, workspace resources, and connected systems; uses approved tools; and creates review-ready artifacts. Work launches July 9, 2026, with web and mobile access starting disabled by default for Enterprise and Edu during a two-week preview period.

## Core Administrative Controls

Administrators manage Work through multiple control layers:

- **Workspace access** via identity controls including SSO, domain verification, and SCIM provisioning
- **Work feature access** using top-level controls and role-based access control (RBAC)
- **Group membership** synchronized through identity providers for automatic access updates
- **Workspace and member roles** determining admin privileges and end-user capabilities
- **Plugin and app governance** through policy, action controls, and approval behavior
- **Source-system permissions** limiting access to authorized content in native applications
- **Approval restrictions** for apps supporting action controls
- **Credits** with per-user monthly limits for eligible Enterprise and Edu workspaces
- **Analytics and reporting** via the Global Admin Console and Compliance API

## Access, Data, Systems, and User Actions

Work respects existing ChatGPT workspace identity, access, and permission controls. Access can synchronize with identity providers through SCIM and group synchronization, enabling centralized management as employees' roles change.

Work accesses data from conversations, uploaded files, workspace resources, and connected systems through plugins. Underlying source systems enforce their own access controls, Work doesn't bypass existing permissions or grant new access in connected applications.

Action risk varies by type. Higher-impact actions (writing, sharing, executing) should be restricted to trusted, reviewed use through roles, narrow permissions, and supported approvals. Lower-impact actions include reading or drafting.

## Compliance

Work uses privacy, security, and data commitments applicable to the customer's ChatGPT workspace. Coverage for data residency, FedRAMP, HIPAA, or BAA varies by plan and configuration and should be confirmed.

Data retention and deletion follow ChatGPT workspace policies and connected application requirements. Connected systems manage their own data lifecycle policies. The Compliance API covers prompts and responses but doesn't track files, actions, or tool calls, with 30 day retention requiring continuous export for longer periods.

## Observability

Workspace analytics show adoption and credit use by user, product, and model. The Compliance API provides user prompts and agent responses. Analytics can identify unusual behavior through signals including unexpected usage spikes, irregular user activity, operational errors, and administrative events.

For Codex activity, analytics and the Analytics API provide adoption metrics. Organizations using local clients can enable OpenTelemetry exports for API requests, errors, and tool decisions (prompt contents are redacted by default).

## Governance

Governance spans three layers: Work access controls determine who can use the personal Work agent; Workspace Agent controls govern who builds and publishes reusable agents; and Codex managed configuration constrains local runtime behavior.

Access can be scoped by workspace roles, identity groups, and administrator defined permissions. Organizations should assign capabilities to groups based on business need rather than providing identical access to all users.

Runtime boundaries depend on the task context. Work on web and mobile is governed separately from desktop and Codex tasks, which follow their own permissions, managed configuration, and approval controls.

## Usage and Cost

ChatGPT Work and Codex share pricing, credits, and usage limits. Consumption varies with model, capability, context size, task duration, tool use, and output size. High variance patterns include frequently run workflows, large information retrieval, multiple app calls, and Codex tasks processing repositories.

Eligible Enterprise and Edu workspaces can set monthly per-user limits through workspace defaults, group defaults, and individual overrides. Users can request increases when enabled. Workspace credit alerts and overage limits provide additional controls.

## Incident and Revocation Controls

Administrators can revoke access through:

- Removing workspace or group access (or via identity provider for SCIM users)
- Disabling or restricting relevant apps
- Revoking shared connections, bots, or service accounts
- Removing Workspace Agents from publication
- Disabling schedules or triggers
- Revoking Codex access tokens, repository connections, and cloud environment access

## Recommended Admin Actions

- Decide rollout scope (pilot vs. broad deployment)
- Confirm RBAC settings match business need
- Review enabled apps and data source permissions
- Set expectations for appropriate use cases
- Review credit controls and usage guidance
- Identify initial high value workflows
- Prepare champions and support teams
- Communicate review and approval expectations
- Monitor adoption and adjust accordingly
