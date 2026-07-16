# Build skills

## Overview

Give Codex new capabilities and expertise by using agent skills to extend it with task-specific capabilities. "A skill packages instructions, resources, and optional scripts so Codex can follow a workflow reliably."

Skills build on the open agent skills standard and serve as the authoring format for reusable workflows. They can be packaged as plugins for distribution to ChatGPT Work and the ChatGPT desktop application.

## How Codex Uses Skills

Codex activates skills in two ways:

1. **Explicit invocation:** Include the skill directly in your prompt using `/skills` or `$` to mention it
2. **Implicit invocation:** Codex selects a skill when your task matches the skill description

Effective descriptions should be concise, front-load key use cases, and include trigger words for successful matching.

## Skill Structure

A skill directory contains:
- `SKILL.md` (required) - instructions and metadata
- `scripts/` (optional) - executable code
- `references/` (optional) - documentation
- `assets/` (optional) - templates and resources
- `agents/openai.yaml` (optional) - appearance and dependencies

## Creating Skills

Use the built-in skill creator with `$skill-creator`, which guides you through defining what the skill does and when it should trigger. Alternatively, create a folder with a `SKILL.md` file containing frontmatter with name and description fields.

## Skill Locations

Skills are discovered from multiple scopes:
- Repository level (`.agents/skills` at various directory levels)
- User level (`$HOME/.agents/skills`)
- Admin level (`/etc/codex/skills`)
- System level (bundled with Codex)

## Distribution

For local use, skills exist as direct folders. For broader distribution, "package them as a plugin" to bundle multiple skills with connectors and presentation assets.

## Best Practices

- Keep each skill focused on one specific task
- Prefer instructions over scripts unless deterministic behavior is needed
- Write imperative steps with explicit inputs and outputs
- Test prompts against skill descriptions to verify correct triggering
