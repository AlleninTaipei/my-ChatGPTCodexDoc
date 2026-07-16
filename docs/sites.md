# Sites

Build and share hosted sites in ChatGPT

## Overview

"Sites lets ChatGPT create, host, refine, and share websites, web apps, and games." Use Sites to transform prompts or existing projects into hosted experiences without separate deployment workflows.

Access Sites through the ChatGPT desktop app or web version at chatgpt.com/sites.

## Get started with Sites

1. **Describe the Site** - Outline audience, purpose, required behavior, and information needed
2. **Review the Site** - Verify generated content and data handling
3. **Refine the Site** - Request changes with relevant context
4. **Manage and share** - Return to Sites to reopen, refine, and share with chosen audience

## Prompt Sites for common tasks

For dashboards or internal tools, include audience and core requirements:

```
Build a project request dashboard for my operations team. Let team members
submit requests, see who owns each one, update the status, and filter the list.
Require people to sign in with their workspace account, and keep the request
data saved between visits.
```

For existing projects requesting deployment:

```
Deploy this project with Sites. Check whether it is compatible, make any
required changes, and give me the deployment URL.
```

## Understand projects, versions, and deployments

A Site represents persistent hosted output accessible from ChatGPT. Sites projects link local source to hosting, storing configuration in `.openai/hosting.json`. Example structure:

```json
{
  "project_id": "<project-id>",
  "d1": "DB",
  "r2": null
}
```

Publishing involves two stages: saving a deployable version and deploying it to production.

## Choose a supported site shape

| Need | Request |
|------|---------|
| Content-led website | Site with no persistent state unless required |
| Saved records/progress | D1 relational database |
| File uploads | R2 object storage |
| Files with metadata | D1 and R2 combined |
| Workspace authentication | Workspace-authenticated identity |
| External sign-in | Authentication-enabled Site |

## Control access and secrets

Sites aren't automatically public. Sharing options may include workspace-only, invite-only, or link-sharing access based on account settings.

Configure environment variables in Settings at chatgpt.com/sites, avoiding secrets in prompts or source files.

## Review before you share

Before sharing a Site:
- Review content and behavior in preview
- Verify proper information and file handling
- Confirm no confidential information exposure
- Choose narrowest appropriate sharing option
- Test access with intended audience
