# Compliance API and audit events

Use the Compliance API for security, legal, governance, and investigation workflows that require auditable records. Use analytics, not compliance records, to measure adoption and trends.

The authenticated Admin API reference is the source of truth for current access requirements, event coverage, routes, schemas, filters, retention, and request behavior.

For an overview of the available compliance surfaces and common integration patterns, see the Compliance Platform guide.

## When to use the Compliance API

The Compliance API is appropriate when you need to:

- Export supported records into an audit or investigation system.
- Apply organizational retention and legal-hold processes.
- Correlate Codex activity with other security or identity data.
- Support approved security, legal, or governance investigations.

It's not a productivity dashboard. Don't use it to infer code quality or individual performance. Use Workspace analytics or the Analytics API for adoption reporting.

## Get started

1. Open the Admin API reference and confirm that your administrator role can access the compliance resources you need.
2. Use the append-only compliance log stream for ongoing collection. Check the authenticated reference for the currently supported resources and retrieval patterns.
3. Test ingestion into a non-production security information and event management (SIEM) system or data lake.
4. Schedule continuous collection and apply your organization's access, retention, and legal-hold controls to exported records. Don't assume the source retention window replaces your organization's retention policy.

## Confirm the administration boundaries

Compliance coverage follows the ChatGPT workspace and the products represented in the current authenticated reference. Platform API organization data follows its own API data and administration controls.

The authenticated reference owns the current routes, event coverage, schemas, filters, retention behavior, permission requirements, and request mechanics.

## Related docs

- Workspace analytics
- Admin rollout guide
- Governance
- Analytics API
