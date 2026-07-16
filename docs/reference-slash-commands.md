# Slash commands

Reference for slash commands in the ChatGPT desktop app

Slash commands enable you to perform actions directly within the task composer without switching contexts. The specific commands available depend on your setup and permissions.

## How to use a slash command

1. In the task composer, type `/`
2. Select from the displayed list or continue typing to narrow down options (e.g., `/status`)

You can also activate skills explicitly by typing `$` in the composer. Visit Skills & Plugins (/codex/skills-and-plugins) for more details.

Active skills and custom prompts (displayed as `/prompts:<name>`) appear within the slash command list.

## Complete slash command reference

| Command | Function |
|---------|----------|
| `/approve` | Authorize a single retry after an automatic-review block |
| `/cloud` | Execute the task in cloud when available |
| `/cloud-environment` | Select your cloud execution environment |
| `/compact` | Compress current task context |
| `/fast` | Toggle Fast service tier availability |
| `/feedback` | Launch feedback interface with optional log inclusion |
| `/fork` | Duplicate a task locally or across worktrees |
| `/goal` | Establish an enduring objective for ChatGPT to pursue |
| `/ide-context` | Toggle IDE context sharing |
| `/init` | Build an `AGENTS.md` template for your project |
| `/local` | Execute in your selected local project |
| `/mcp` | Display MCP server connections |
| `/memories` | Configure memory access and generation settings |
| `/model` | Select the task's operating model |
| `/pet` | Activate or deactivate the desktop companion |
| `/personality` | Select ChatGPT's communication style |
| `/plan` | Activate multi-step planning mode |
| `/project` | Select project for upcoming tasks |
| `/reasoning` | Adjust reasoning depth for current task |
| `/review` | Initiate code inspection for uncommitted work |
| `/side` | Begin isolated conversation without affecting main task |
| `/status` | Display task identifier, context consumption, and limits |
| `/task` | Initiate a standalone task |
| `/worktree` | Run in a fresh Git worktree |

## Using `/goal` for persistent objectives

Invoke `/goal` in the app composer to activate Goal mode, a framework for ChatGPT to maintain focus on a defined objective throughout execution. Consider using `/plan` beforehand to develop your goal statement collaboratively with ChatGPT.

When active, the progress displays above the composer with controls to pause/resume, modify, or abandon the goal without additional commands. You can provide ongoing guidance through messages while the objective progresses.

For best practices on crafting effective goals, consult Goal mode (/codex/prompting#goal-mode).
