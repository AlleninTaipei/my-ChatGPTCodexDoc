# Analytics API

Understand the purpose and administration boundary of the Codex Analytics API

## Overview

The Codex Analytics API provides aggregated Codex usage and activity metrics for a ChatGPT workspace.

The authenticated Codex Analytics API reference is the source of truth for current access requirements, routes, request and response schemas, metrics, time semantics, and pagination.

## When to use the Analytics API

The Analytics API is appropriate when you need to:

- Automate recurring Codex reporting.
- Join aggregated Codex metrics with internal organizational data.
- Build a controlled reporting layer for approved audiences.
- Avoid coupling an integration to an interactive dashboard.

It's not a raw audit-log interface. Use the Compliance API when the workflow requires auditable activity records.

## Confirm the administration boundaries

Analytics API results are scoped to a ChatGPT workspace, but requests authenticate with a Platform organization API key. The key's organization must match the organization associated with the workspace.

The authenticated reference owns current key provisioning, scope requirements, routes, schemas, fields, time semantics, and pagination behavior. This page doesn't duplicate that contract.

## Related docs

- [Workspace analytics](/codex/enterprise/workspace-analytics)
- [Admin rollout guide](/codex/enterprise/admin-setup)
- [Governance](/codex/enterprise/governance)
- [Compliance API](/codex/enterprise/compliance-api)
