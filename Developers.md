# Developers

## Code Review

Review code changes across ChatGPT and Codex clients

### Start a review

In ChatGPT Work, upload code for review or make it available through an installed source plugin. In your prompt, identify the pull request, branch, commit, files, and review criteria.

#### Review in the app

Open the review pane to understand what changed, give line-specific feedback, and decide what to stage, revert, commit, or push.

To ask Codex to review changes, type `/review` in the composer. Choose **Review against a base branch** or **Review uncommitted changes**. Codex reports prioritized findings without changing your working tree.

The review pane requires a project inside a Git repository. If your project isn't a Git repository yet, the app prompts you to create one.

Type `/review` to open the CLI review presets. Codex starts a dedicated reviewer that reads the selected diff and reports prioritized, actionable findings without changing your working tree.

Type `/review` in the IDE extension composer. Choose **Review against a base branch** or **Review uncommitted changes**. Codex reports prioritized findings without changing your working tree.

The `/review` command appears only when the open project is inside a Git repository.

### Choose a review scope

Name the pull request, branch, commit, or files to inspect in your prompt. To review local files that aren't available through an installed source plugin, upload them to the conversation.

#### What changes it shows

The review pane reflects the state of your Git repository, not just what Codex edited. It includes changes made by Codex, changes you made yourself, and any other uncommitted changes in the repository.

By default, the review pane shows **Unstaged** changes. Use **Staged** for the Git index, **Commit** for a selected commit, **Branch** for the diff against your base branch, or **Last turn** for the most recent assistant turn.

Choose one of these `/review` scopes:

- **Review against a base branch** finds the merge base and reviews your branch diff.
- **Review uncommitted changes** includes staged, unstaged, and untracked files.
- **Review a commit** reviews the exact change set for a selected commit.
- **Custom review instructions** focuses the review on criteria you provide.

Choose one of these `/review` scopes:

- **Review against a base branch** compares your current branch with a branch you select.
- **Review uncommitted changes** reviews the changes in your working tree.

### Work with review results

Review findings appear in the web conversation. Ask for evidence, request a narrower follow-up review, or ask ChatGPT to prepare revised files.

#### Code review results

Review findings appear as inline comments in the review pane.

Reviews run in the current task by default. Under **Settings** > **General** > **Code review**, choose **Detached** to start a separate review task.

The review appears as a turn in the transcript. Set `review_model` in `config.toml` when you want reviews to use a different model from the current session.

By default, the review runs in the current task. Set `chatgpt.reviewDelivery` to `detached` when you want `/review` to start a separate review task.

If you ask ChatGPT to prepare revised files, the tools and workspace permissions available to the conversation still apply.

If you ask Codex to apply the fixes it finds, your normal sandbox and approval settings apply.

### Navigating the review pane

- Clicking a file name typically opens that file in your chosen editor. You can choose the default editor in developer settings.
- Clicking the file name background expands or collapses the diff.
- Clicking a single line while holding Cmd pressed opens the line in your chosen editor.
- If you're happy with a change, you can stage it or revert changes you don't want.

### Inline comments for feedback

Inline comments let you attach feedback directly to specific lines in the diff. This is often the fastest way to guide Codex to the right fix.

To leave an inline comment:

1. Open the review pane.
2. Hover over the line you want to comment on.
3. Select the **+** button that appears.
4. Write your feedback and submit it.
5. After you finish leaving feedback, send a message back to the task.

Because comments are line-specific, Codex can respond more precisely than with a general instruction.

Codex treats inline comments as review guidance. After leaving comments, send a follow-up message that makes your intent explicit, for example, "Address the inline comments and keep the scope minimal."

### Pull request reviews

When Codex has GitHub access for your repository and the current project is on the pull request branch, the ChatGPT desktop app can help you work through pull request feedback without leaving the app. The sidebar shows pull request context and feedback from reviewers, and the review pane shows comments alongside the diff so you can ask Codex to address issues in the same task.

Install the GitHub CLI (`gh`) and authenticate it with `gh auth login` so Codex can load pull request context, review comments, and changed files. If `gh` is missing or unauthenticated, pull request details may not appear in the sidebar or review pane.

Use this flow when you want to keep the full fix loop in one place:

1. Open the review pane on the pull request branch.
2. Review the pull request context, comments, and changed files.
3. Ask Codex to fix the specific comments you want handled.
4. Inspect the resulting diff in the review pane.
5. Stage, commit, and push the changes to the pull request branch when you're ready.

For GitHub-triggered reviews, see Use Codex in GitHub.

### Staging and reverting files

The review pane includes Git actions so you can shape the diff before you commit.

You can stage, unstage, or revert changes at these levels:

- **Entire diff**: Use the action buttons in the review header, such as **Stage all** or **Revert all**.
- **Per file**: Stage, unstage, or revert an individual file.
- **Per hunk**: Stage, unstage, or revert a single hunk.

Use staging when you want to accept part of the work, and revert when you want to discard it.

#### Staged and unstaged states

Git can represent both staged and unstaged changes in the same file. When that happens, the pane can show the same file in both views. That's normal Git behavior.

---

## Integrated Terminal

Run commands and inspect output without leaving your task

Each task in the ChatGPT desktop app includes a terminal scoped to its current project or worktree. Open it from the terminal icon in the top-right corner of the app, or press Ctrl+\`.

### Run and validate your project

Use the terminal to validate changes, run scripts, and perform Git operations without switching apps. ChatGPT can read the current terminal output, so it can check a running development server or refer to a failed build while it works with you.

Common commands include:

- `git status`
- `git pull --rebase`
- `pnpm test` or `npm test`
- `pnpm run lint` or another project-specific check

### Create reusable actions

If you run a command regularly, define an action in your local environment. Actions appear as shortcuts in the ChatGPT desktop app and run in the integrated terminal.

Cmd+K opens the app command palette; it doesn't clear the terminal. To clear the terminal, press Ctrl+L.

---

## Build skills

### Overview

Give Codex new capabilities and expertise by using agent skills to extend it with task-specific capabilities. "A skill packages instructions, resources, and optional scripts so Codex can follow a workflow reliably."

Skills build on the open agent skills standard and serve as the authoring format for reusable workflows. They can be packaged as plugins for distribution to ChatGPT Work and the ChatGPT desktop application.

### How Codex Uses Skills

Codex activates skills in two ways:

1. **Explicit invocation:** Include the skill directly in your prompt using `/skills` or `$` to mention it
2. **Implicit invocation:** Codex selects a skill when your task matches the skill description

Effective descriptions should be concise, front-load key use cases, and include trigger words for successful matching.

### Skill Structure

A skill directory contains:
- `SKILL.md` (required) - instructions and metadata
- `scripts/` (optional) - executable code
- `references/` (optional) - documentation
- `assets/` (optional) - templates and resources
- `agents/openai.yaml` (optional) - appearance and dependencies

### Creating Skills

Use the built-in skill creator with `$skill-creator`, which guides you through defining what the skill does and when it should trigger. Alternatively, create a folder with a `SKILL.md` file containing frontmatter with name and description fields.

### Skill Locations

Skills are discovered from multiple scopes:
- Repository level (`.agents/skills` at various directory levels)
- User level (`$HOME/.agents/skills`)
- Admin level (`/etc/codex/skills`)
- System level (bundled with Codex)

### Distribution

For local use, skills exist as direct folders. For broader distribution, "package them as a plugin" to bundle multiple skills with connectors and presentation assets.

### Best Practices

- Keep each skill focused on one specific task
- Prefer instructions over scripts unless deterministic behavior is needed
- Write imperative steps with explicit inputs and outputs
- Test prompts against skill descriptions to verify correct triggering

---

## Build plugins

Create, test, and distribute plugins for ChatGPT

### Overview

This documentation targets plugin authors. Users seeking to browse, install, and use plugins should visit the [Plugins section](/codex/plugins). For single-repository or personal workflows, start with local skills. Build a plugin when sharing workflows across teams, bundling connectors or MCP configuration, packaging lifecycle hooks, or publishing stable packages.

A plugin can include skills, an MCP-backed app, or both. For service connections or MCP server tool exposure, see [Build an app](/codex/build-app).

Public examples are available in [Figma](https://github.com/openai/plugins/tree/main/plugins/figma), [Notion](https://github.com/openai/plugins/tree/main/plugins/notion), and [Build web apps](https://github.com/openai/plugins/tree/main/plugins/build-web-apps) repositories.

### Create a plugin with `@plugin-creator`

The built-in `@plugin-creator` skill provides the fastest setup path.

It scaffolds the required `.codex-plugin/plugin.json` manifest and generates local marketplace entries for testing. Existing plugin folders can still use `@plugin-creator` to integrate into local marketplaces.

#### Create and test a plugin locally with MCP-server-backed dev-mode app

You can use `@plugin-creator` to test plugins locally with MCP-server-backed apps. The plugin requires a local folder and manifest, but the app launches in ChatGPT developer mode.

**Enable developer mode:**

1. Open [ChatGPT](https://chatgpt.com)
2. Access **Settings**
3. Select **Security and login**
4. Enable **Developer mode**

**Create the app in developer mode:**

1. Open **Settings → Plugins** or [the Plugins page](https://chatgpt.com/plugins)
2. Select the plus button
3. Complete the modal to create a developer-mode app for your MCP server
4. Copy the app ID from the browser URL (starts with `plugin_asdk_app`)

Provide that `plugin_asdk_app...` ID to `@plugin-creator`. Example prompt:

```
$plugin-creator create a Codex plugin for my ChatGPT app. Use plugin_asdk_app_6a4c0062f3b88191855c0a80eac5d53d and name it Acme Support. Include a personal marketplace entry so I can test it locally.
```

`@plugin-creator` will create the plugin folder, required `.codex-plugin/plugin.json`, and app wiring.

**After creation:**

1. Review `.app.json` to confirm it points at the correct `plugin_asdk_app...` ID
2. Review `.codex-plugin/plugin.json` to ensure its `apps` field references `./.app.json`
3. Add bundled skills under `skills/` if needed
4. If a personal marketplace entry was created, refresh ChatGPT and install from your local plugin directory to test

### Build your own curated plugin list

A marketplace is a JSON plugin catalog. `@plugin-creator` generates one for a single plugin; you can expand it into a curated list by adding entries.

In Work or Codex, marketplaces appear as selectable sources in the plugin directory. Use `$REPO_ROOT/.agents/plugins/marketplace.json` for repository scope or `~/.agents/plugins/marketplace.json` for personal scope. Add entries under `plugins[]`, point each `source.path` at the plugin folder with `./`-prefixed relative paths, and set `interface.displayName` for the marketplace label shown in the picker. Restart the ChatGPT desktop app afterward.

One marketplace can expose a single testing plugin or grow into a larger curated catalog as you add plugins.

#### Add a marketplace from the CLI

Use `codex plugin marketplace` commands to add and manage marketplace sources:

```bash
codex plugin marketplace add owner/repo
codex plugin marketplace add owner/repo --ref main
codex plugin marketplace add https://github.com/example/plugins.git --sparse .agents/plugins
codex plugin marketplace add ./local-marketplace-root
```

Marketplace sources accept GitHub shorthand (`owner/repo` or `owner/repo@ref`), HTTP/HTTPS Git URLs, SSH Git URLs, or local directories. Use `--ref` to pin Git references and `--sparse PATH` (for Git sources only) to enable sparse checkout.

**Inspect, refresh, or remove configured marketplaces:**

```bash
codex plugin marketplace list
codex plugin marketplace upgrade
codex plugin marketplace upgrade marketplace-name
codex plugin marketplace remove marketplace-name
```

The `list` command prints each configured marketplace and its resolved root path.

#### Create a plugin manually

Start with a minimal plugin packaging one skill.

**Step 1: Create plugin folder structure**

```bash
mkdir -p my-first-plugin/.codex-plugin
```

**Step 2: Create manifest**

File: `my-first-plugin/.codex-plugin/plugin.json`

```json
{
  "name": "my-first-plugin",
  "version": "1.0.0",
  "description": "Reusable greeting workflow",
  "skills": "./skills/"
}
```

Use a stable plugin `name` in kebab-case as the plugin identifier and component namespace.

**Step 3: Add skill**

```bash
mkdir -p my-first-plugin/skills/hello
```

File: `my-first-plugin/skills/hello/SKILL.md`

```markdown
---
name: hello
description: Greet the user with a friendly message.
---

Greet the user warmly and ask how you can help.
```

**Step 4: Add to marketplace**

Use `@plugin-creator` to generate a marketplace, or manually wire the plugin into Codex following the curated plugin list instructions.

#### Install a local plugin manually

Choose between repo marketplace (team-scoped) or personal marketplace (individual):

**Repo marketplace:**

Step 1: Copy plugin folder to `$REPO_ROOT/plugins/my-plugin`:
```bash
mkdir -p ./plugins
cp -R /absolute/path/to/my-plugin ./plugins/my-plugin
```

Step 2: Add or update `$REPO_ROOT/.agents/plugins/marketplace.json`:
```json
{
  "name": "local-repo",
  "plugins": [
    {
      "name": "my-plugin",
      "source": {
        "source": "local",
        "path": "./plugins/my-plugin"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
```

Step 3: Restart the ChatGPT desktop app and verify plugin appears.

**Personal marketplace:**

Step 1: Copy plugin folder to `~/.codex/plugins/my-plugin`:
```bash
mkdir -p ~/.codex/plugins
cp -R /absolute/path/to/my-plugin ~/.codex/plugins/my-plugin
```

Step 2: Add or update `~/.agents/plugins/marketplace.json` with the plugin entry's `source.path` pointing to that directory.

Step 3: Restart the ChatGPT desktop app and verify plugin appears.

The marketplace file points to plugin locations; these are example paths rather than fixed requirements. Codex resolves `source.path` relative to the marketplace root, not the `.agents/plugins/` folder. See [Marketplace metadata](#marketplace-metadata) for file format details.

After plugin changes, update the directory your marketplace entry references and restart the ChatGPT desktop app.

#### Share a local plugin with your workspace

After creating and installing a plugin in Work or Codex, share it with workspace members from the ChatGPT desktop app:

1. Open **Plugins** in the ChatGPT desktop app
2. Go to **Created by you** and open the plugin details page
3. Select **Share**
4. Add workspace members, groups, or copy a share link
5. Choose access level and send invitation or link

Recipients find the plugin under **Shared with you** in the plugin directory. Workspace sharing doesn't publish to the public Plugins Directory; plugins remain within workspace boundaries. Unsigned-in accounts cannot access them. Use groups for team/role access, marketplaces for repo/CLI distribution, and workspace sharing for teammate access via the desktop app.

**Workspace admins** can disable plugin sharing by adding to `requirements.toml`:

```toml
features.plugin_sharing = false
```

### Marketplace metadata

Define repo marketplaces at `$REPO_ROOT/.agents/plugins/marketplace.json` or personal marketplaces at `~/.agents/plugins/marketplace.json`. Marketplace files control plugin ordering and install policies in the ChatGPT desktop app. They can represent a single testing plugin or a curated catalog. Before adding plugins, ensure `version`, publisher metadata, and install-surface copy are ready for developer visibility.

**Example marketplace file:**

```json
{
  "name": "local-example-plugins",
  "interface": {
    "displayName": "Local Example Plugins"
  },
  "plugins": [
    {
      "name": "my-plugin",
      "source": {
        "source": "local",
        "path": "./plugins/my-plugin"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    },
    {
      "name": "research-helper",
      "source": {
        "source": "local",
        "path": "./plugins/research-helper"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
```

**Marketplace field guidance:**

- Top-level `name` identifies the marketplace
- `interface.displayName` shows the marketplace title in the ChatGPT desktop app
- `plugins` array contains one object per plugin
- Each plugin's `source.path` points to its directory, relative to marketplace root
- Start paths with `./` and keep them inside the marketplace root
- Include `policy.installation`, `policy.authentication`, and `category` on each entry
- Use `policy.installation` values like `AVAILABLE`, `INSTALLED_BY_DEFAULT`, or `NOT_AVAILABLE`
- Use `policy.authentication` to determine whether auth happens at install or first use

The marketplace controls load source. Local `source.path` entries can point elsewhere if plugins exist outside example directories. One marketplace file can live in the plugin development repo or a separate marketplace repo, and can point to one or many plugins.

**Git-backed marketplace entries** can use `"source": "url"` (repository root) or `"source": "git-subdir"` (subdirectory):

```json
{
  "name": "remote-helper",
  "source": {
    "source": "git-subdir",
    "url": "https://github.com/example/codex-plugins.git",
    "path": "./plugins/remote-helper",
    "ref": "main"
  },
  "policy": {
    "installation": "AVAILABLE",
    "authentication": "ON_INSTALL"
  },
  "category": "Productivity"
}
```

Git entries may use `ref` or `sha` selectors. If Codex cannot resolve an entry source, it skips that plugin entry rather than failing the entire marketplace.

**NPM package registry entries:**

```json
{
  "name": "npm-helper",
  "source": {
    "source": "npm",
    "package": "@example/codex-plugin",
    "version": "^1.2.0",
    "registry": "https://registry.npmjs.org"
  },
  "policy": {
    "installation": "AVAILABLE",
    "authentication": "ON_INSTALL"
  },
  "category": "Productivity"
}
```

- `package` is required and supports registry scopes
- `version` is optional; accepts versions, distribution tags, and ranges (not paths or URLs)
- `registry` is optional; must be HTTPS URL without credentials, queries, or fragments
- Codex downloads without running lifecycle scripts
- npm CLI must be installed; registry authentication comes from its configuration

#### How the ChatGPT desktop app uses marketplaces

The ChatGPT desktop app can read marketplace files from:

- Official curated Plugins Directory marketplace
- Repo marketplace at `$REPO_ROOT/.agents/plugins/marketplace.json`
- Legacy-compatible marketplace at `$REPO_ROOT/.claude-plugin/marketplace.json`
- Personal marketplace at `~/.agents/plugins/marketplace.json`

Any marketplace-exposed plugin is installable. The app installs plugins to `~/.codex/plugins/cache/$MARKETPLACE_NAME/$PLUGIN_NAME/$VERSION/`. For local plugins, `$VERSION` is `local`, and the app loads from that cache path rather than directly from the marketplace entry.

Plugins can be individually enabled or disabled. The app stores each plugin's state in `~/.codex/config.toml`.

### Package and distribute plugins

#### Plugin structure

Every plugin requires `.codex-plugin/plugin.json`. Plugins may also include `skills/`, `hooks/`, `.app.json`, `.mcp.json`, and `assets/`:

```
my-plugin/
├── .codex-plugin/
│   └── plugin.json          (Required)
├── skills/
│   └── my-skill/
│       └── SKILL.md         (Optional)
├── hooks/
│   └── hooks.json           (Optional)
├── .app.json                (Optional)
├── .mcp.json                (Optional)
└── assets/                  (Optional)
```

Only `plugin.json` belongs in `.codex-plugin/`. Keep `skills/`, `hooks/`, `assets/`, `.mcp.json`, and `.app.json` at the plugin root.

Published plugins use richer manifests than quick-start scaffolds. The manifest serves three purposes:

- Identify the plugin
- Point to bundled components (skills, connectors, MCP servers, hooks)
- Provide install-surface metadata (descriptions, icons, legal links)

**Complete manifest example:**

```json
{
  "name": "my-plugin",
  "version": "0.1.0",
  "description": "Bundle reusable skills and connectors.",
  "author": {
    "name": "Your team",
    "email": "team@example.com",
    "url": "https://example.com"
  },
  "homepage": "https://example.com/plugins/my-plugin",
  "repository": "https://github.com/example/my-plugin",
  "license": "MIT",
  "keywords": ["research", "crm"],
  "skills": "./skills/",
  "mcpServers": "./.mcp.json",
  "apps": "./.app.json",
  "hooks": "./hooks/hooks.json",
  "interface": {
    "displayName": "My Plugin",
    "shortDescription": "Reusable skills and connectors",
    "longDescription": "Distribute skills and connectors together.",
    "developerName": "Your team",
    "category": "Productivity",
    "capabilities": ["Read", "Write"],
    "websiteURL": "https://example.com",
    "privacyPolicyURL": "https://example.com/privacy",
    "termsOfServiceURL": "https://example.com/terms",
    "defaultPrompt": [
      "Use My Plugin to summarize new CRM notes.",
      "Use My Plugin to triage new customer follow-ups."
    ],
    "brandColor": "#10A37F",
    "composerIcon": "./assets/icon.png",
    "logo": "./assets/logo.png",
    "screenshots": ["./assets/screenshot-1.png"]
  }
}
```

#### Manifest fields

**Top-level fields** define package metadata and point to bundled components:

- `name`, `version`, `description`: identify the plugin
- `author`, `homepage`, `repository`, `license`, `keywords`: publisher and discovery metadata
- `skills`, `mcpServers`, `apps`, `hooks`: point to bundled components relative to plugin root
- `interface`: controls install-surface presentation

**Interface object fields** control install-surface metadata:

- `displayName`, `shortDescription`, `longDescription`: title and descriptive copy
- `developerName`, `category`, `capabilities`: publisher and capability metadata
- `websiteURL`, `privacyPolicyURL`, `termsOfServiceURL`: external links
- `defaultPrompt`, `brandColor`, `composerIcon`, `logo`, `screenshots`: starter prompts and visuals

#### Path rules

- Keep manifest paths relative to plugin root, starting with `./`
- Store visual assets (`composerIcon`, `logo`, `screenshots`) under `./assets/` when possible
- Use `skills` for skill folders, `apps` for `.app.json`, `mcpServers` for `.mcp.json`, `hooks` for lifecycle hooks
- Enabled plugins can include lifecycle hooks alongside skills, MCP servers, and connectors
- If hooks are stored at `./hooks/hooks.json`, no `hooks` manifest entry is required; Codex checks this default automatically

#### Bundled MCP servers and lifecycle hooks

`mcpServers` points to `.mcp.json` containing a direct server map or wrapped `mcp_servers` object.

**Direct server map:**

```json
{
  "docs": {
    "command": "docs-mcp",
    "args": ["--stdio"]
  }
}
```

**Wrapped server map:**

```json
{
  "mcp_servers": {
    "docs": {
      "command": "docs-mcp",
      "args": ["--stdio"]
    }
  }
}
```

After installation, users can enable/disable bundled MCP servers and tune tool approval policy from Codex config without editing the plugin. Use `plugins.<plugin>.mcp_servers.<server>` for plugin-scoped MCP server policy:

```toml
[plugins."my-plugin".mcp_servers.docs]
enabled = true
default_tools_approval_mode = "prompt"
enabled_tools = ["search"]

[plugins."my-plugin".mcp_servers.docs.tools.search]
approval_mode = "approve"
```

When a plugin is enabled, Codex can load lifecycle hooks from it alongside user, project, and managed hooks.

Installing or enabling a plugin doesn't automatically trust its hooks. Plugin-bundled hooks are non-managed, so Codex skips them until the user reviews and trusts the current definition.

**Default plugin hook file** is `hooks/hooks.json`:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 ${PLUGIN_ROOT}/hooks/session_start.py",
            "statusMessage": "Loading plugin context"
          }
        ]
      }
    ]
  }
}
```

If `hooks` is defined in `.codex-plugin/plugin.json`, Codex uses that manifest entry instead of the default. The field can be a single path, array of paths, inline hooks object, or array of inline hooks objects:

```json
{
  "name": "repo-policy",
  "hooks": ["./hooks/session.json", "./hooks/tools.json"]
}
```

Hook paths follow standard manifest rules: start with `./`, resolve relative to plugin root, stay inside the root.

Plugin hook commands receive `PLUGIN_ROOT` and `PLUGIN_DATA` environment variables. `PLUGIN_ROOT` points to the installed plugin root; `PLUGIN_DATA` points to the plugin's writable data directory. Codex also sets `CLAUDE_PLUGIN_ROOT` and `CLAUDE_PLUGIN_DATA` for compatibility with existing plugin hooks.

Plugin hooks use the same event schema as regular hooks. See [Hooks](/codex/hooks) for supported events, inputs, outputs, trust review, and current limitations.

#### Publish official public plugins

To publish plugins for public use, submit through the plugin submission portal. See [Submit plugins](/codex/submit-plugins) for the complete review and publishing process.

---

## Build an app

Build the MCP-backed app portion of a plugin, with optional UI from the Apps SDK

### Overview

Apps are one part of the plugin model. A plugin is the package users discover, install, submit, and publish. An app is the MCP-backed capability inside that package.

The Apps SDK is the ChatGPT app development framework for MCP-backed apps. It builds on MCP: your server exposes tools and returns structured data, and, when you need UI, Apps SDK conventions let you register MCP UI resources and connect them to those tools inside ChatGPT.

Build an app when your plugin needs to connect to a service, expose tools, authenticate users, or take action through an MCP server.

### App building model

An app can include:

- **An MCP server:** the server defines tools, handles authentication, returns structured data, and enforces the integration's real behavior. See Build your MCP server.
- **Tool metadata and annotations:** required for reliable model behavior, discovery, and review. Tool names, descriptions, schemas, `readOnlyHint`, `openWorldHint`, and `destructiveHint` should match what the tool actually does. See the Apps SDK reference for the descriptor fields and annotation details.
- **Optional MCP UI:** useful when users need to inspect, compare, edit, confirm, or navigate structured information inside ChatGPT. If your app can complete its job through tool calls and model responses, you do not need custom UI. Build this with Apps SDK UI resources when tool calls and model responses are not enough.

### Build MCP-first

Define the app's capabilities before designing UI:

1. Identify the user workflows the app should support.
2. Define the MCP tools needed for those workflows.
3. Write clear tool names, descriptions, input schemas, and output schemas. Use the metadata optimization guide to improve discovery and model selection.
4. Mark every tool with accurate safety annotations, then review the security and privacy guidance for write actions, data handling, and network access.
5. Add authentication only for data or actions that require it.
6. Test the app from ChatGPT developer mode before packaging it into a plugin.

### Add UI if it materially improves the app experience

The Apps SDK quickstart shows how to build a simple MCP-backed app with an optional UI component. The Apps SDK helps you build MCP UI for an MCP-backed app. It is optional: use it when the app needs an embedded component, modal, fullscreen view, or other custom interaction in ChatGPT. See Build your MCP UI for UI-specific patterns.

Do not add UI just to show a banner ad or brand placement. The UI should materially improve the user's workflow by making the app easier to inspect, edit, compare, confirm, or navigate.

Even when you add UI, keep the tools decoupled from the rendering layer. Tools should still return useful structured data and model-readable results, while UI components focus on presentation and interaction. See Separate data processing from UI rendering.

### Package the app as a plugin

After the app works in developer mode, package it into a plugin so users can install it.

1. Create or scaffold the plugin folder. See Build plugins.
2. Add the app reference to the plugin manifest.
3. Add bundled skills if ChatGPT should follow repeatable workflows alongside the app.
4. Test the plugin locally.
5. Review the Apps SDK app guidelines, then submit it for review as part of a plugin when it is ready for public distribution. See Submit plugins.

---

## Submit plugins

Submit a plugin for review through the OpenAI Platform

### Overview

Use the plugin submission portal to submit a plugin for review when you're ready to publish it for public use.

A plugin is an app, skills, or both. You can submit:

- A skills-only plugin that packages reusable workflows.
- An app-only plugin backed by an MCP server. Custom UI is optional.
- An app-plus-skills plugin that combines an MCP-backed app with bundled skills.

The submission form collects listing information, MCP server details, bundled skills, starter prompts, test cases, country availability, and policy attestations. Which fields you complete depends on whether the plugin includes skills, an app, or both.

### Before you submit

#### Submit the MCP server, not an existing app reference

You will not be able to submit a plugin that references an existing, already-published ChatGPT app. If your plugin includes an app that already exists in ChatGPT, submit that app's MCP server from scratch through the portal as a new MCP-backed plugin submission. The portal scans that MCP server, validates the tool metadata, and uses the submitted server details during review.

#### Get plugin submission access

You need an organization role with app management write access before you can create or submit plugin drafts.

1. Open [OpenAI Platform roles settings](https://platform.openai.com/settings/organization/people/roles).
2. Select the organization that owns the plugin.
3. Open the role assigned to the submitter, or create a new role.
4. In the role permissions, set **Apps Management** to **Write**.
5. Save the role and assign it to each person who needs to create, edit, or submit plugin drafts.
6. Reload the [plugin submission portal](https://platform.openai.com/plugins).

Organization owners already have app management permissions. Non-owner submitters need write access to create or submit drafts, and read access to view drafts and review status.

#### Verify your developer or business identity

Every public submission must use a verified developer or business identity in the OpenAI Platform. Reviewers use this identity to confirm the submission matches the name, website, support contact, privacy policy, and terms in your public listing.

To verify an identity:

1. Sign in to the [OpenAI Platform](https://platform.openai.com).
2. Select the organization that will publish the plugin.
3. Open [organization settings](https://platform.openai.com/settings/organization/general).
4. Complete **individual verification** if you will publish under your own name, or **business verification** if you will publish under a company name.
5. Return to the plugin submission form and select the verified identity in the **Developer Identity** field.

Reviewers may reject submissions that use an unverified or mismatched publisher identity. If the Platform shows that the developer or business identity is verified but the plugin submission form does not recognize it, check that you are submitting from the same organization and project where the identity was verified. The submitter also needs **Apps Management** write access for that organization. Ask an organization owner or admin to update the submitter's role, then reload the plugin submission portal.

#### Prepare required materials

Before opening the form, collect:

| Material | What to prepare |
|----------|-----------------|
| Listing details | Plugin name, short description, long description, logo, category, website, support URL, privacy policy URL, and terms URL. |
| Developer identity | Verified individual or business identity in the OpenAI Platform. |
| MCP server | For plugins that contain apps: public MCP server URL, domain verification access, authentication details, demo credentials if needed, content security policy, and accurate tool metadata. |
| Tool annotations | For plugins that contain apps: `readOnlyHint`, `openWorldHint`, and `destructiveHint` values for every MCP tool. |
| Skills | For skills plugins: skill bundle or ZIP with the final skill file tree. |
| Prompts | Starter prompts that show useful, realistic workflows. |
| Test cases | Five positive test cases and three negative test cases with clear expected behavior. |
| Availability | Countries or regions where the plugin should be available. |
| Release notes | A short summary of what you are submitting and what changed since any prior version. |

### Create a plugin submission

1. Open the [plugin submission portal](https://platform.openai.com/plugins).
2. Select **Create plugin**.
3. Choose the submission type:
   - **Skills only** for a plugin that only packages skills.
   - **With MCP** for an app-only plugin backed by an MCP server.
   - **With MCP** for an app-plus-skills plugin that combines an MCP-backed app with bundled skills.

The portal saves the submission as a draft while you complete the form.

### Complete the form

#### Info

Complete the public listing and publisher fields:

- **Plugin name:** Use the customer-facing product or workflow name.
- **Descriptions:** Explain what the plugin helps users do. Keep the short description concise and use the long description for workflow details.
- **Developer Identity:** Select the verified individual or business identity for the publisher.
- **Logo and category:** Use production-ready brand assets.
- **Website, support, privacy, and terms URLs:** Use public URLs that match the publisher and disclose relevant data handling.

Review your MCP responses against your privacy policy before you submit. Remove unnecessary personal data, auth secrets, debug payloads, internal identifiers, and undisclosed user-related fields from tool responses.

#### MCP

For app or MCP submissions:

1. Enter the production MCP server URL.
2. Configure authentication and provide reviewer-ready demo credentials if the server requires sign-in.
3. Define a content security policy that allows the exact domains your app fetches from.
4. Complete domain verification if the portal shows a **Domain not verified** challenge. Use an HTTPS origin on the MCP hostname or a parent hostname, and host the exact token at `/.well-known/openai-apps-challenge`.
5. Select **Scan Tools**.
6. Review the discovered tools, domains, validation output, and tool metadata.
7. Fix server or metadata issues, deploy the fix, then scan again.

Do not enter a ChatGPT app ID or try to point the portal at an existing, published ChatGPT app. The submission must provide the MCP server URL and review materials directly, even when that server backs an app that is already published in ChatGPT.

##### Domain verification

Plugins that contain apps must verify control of the domain that hosts the app's MCP server. When the portal shows a domain verification challenge, place the exact verification token at the generated well-known URL:

```
https://<challenge-base-host>/.well-known/openai-apps-challenge
```

The challenge endpoint must return only that plugin's verification token. Do not return JSON, a list of tokens, or multiple tokens from the same URL.

The **Challenge Base URL** is an optional HTTPS origin that tells the portal where to check the token. It must be the MCP hostname or a parent hostname. Paths are ignored. For example, if the MCP server URL is `https://api.example.com/mcp`, the default challenge URL is `https://api.example.com/.well-known/openai-apps-challenge`, and `https://example.com` can be used as a parent-origin challenge base if you can host the token there.

If two plugins that contain apps share the same MCP hostname but differ only by path, they also share the same default challenge URL. You cannot verify them separately by putting different tenant paths in the Challenge Base URL, because the path is ignored. Use a parent origin that can host the new token, give the app's MCP server a distinct hostname, or work with OpenAI support if neither hosting option is possible.

If another plugin that contains an app already uses the same MCP hostname, do not replace its existing challenge token unless that plugin no longer needs it. Use an allowed parent-origin Challenge Base URL or a distinct MCP hostname for the new submission.

Every tool should have clear names, descriptions, schemas, and output structure. Add output schemas when they help reviewers and models understand what the tool returns.

Set tool annotations to match each tool's real behavior:

| Annotation | Use it when |
|-----------|------------|
| `readOnlyHint` | Set to `true` only when the tool fetches, looks up, lists, retrieves, previews, or computes information and doesn't change anything. Set to `false` if the tool can create, update, delete, send, enqueue, run jobs, start workflows, write logs, or otherwise change state. |
| `openWorldHint` | For write tools, set to `true` if the tool can change publicly visible internet state, such as posting online, sending external messages, publishing content, pushing code, or submitting forms to third parties. Set to `false` only if the tool operates entirely within closed or private systems and can't change publicly visible internet state. |
| `destructiveHint` | For write tools, set to `true` if the tool can delete, overwrite, revoke access, send messages or transactions that can't be undone, or cause another irreversible side effect. Otherwise, set it to `false`. |

#### Skills

Upload the final skill bundle for skills-only or app-plus-skills submissions. Use the same file tree and instructions you tested locally.

Each skill should include:

- A clear `SKILL.md` with trigger conditions and task instructions.
- Any referenced scripts, templates, or assets.
- Minimal, scoped instructions that fit the plugin's purpose.

Uploaded skills are scanned for policy compliance and security risks, including sensitive information, unnecessary access requests, and instructions that may conflict with safe or expected plugin behavior. Skills must follow the same standards as the rest of the plugin and may block submission or require remediation if they fail automated scanning.

#### Prompts

Add starter prompts that show the plugin's highest-value workflows. Good prompts are specific enough to show when to use the plugin, but general enough that users can adapt them.

Examples:

- "Investigate checkout errors from the last release and summarize likely root causes."
- "Create a P1 incident brief from the latest support tickets and related deploys."
- "Review unsuccessful deployment logs and recommend the next debugging step."

#### Testing

Submit exactly five positive test cases and three negative test cases.

For each positive test case, include:

- User prompt.
- Expected tool, skill, or workflow behavior.
- Expected result shape.
- Test account or fixture data required to reproduce it.

For each negative test case, include:

- User prompt or scenario.
- Expected refusal, clarification, or safe fallback behavior.
- Why the plugin shouldn't complete the requested action.

Use test cases that reviewers can run without internal context. If your plugin requires authentication, make sure the provided demo credentials can complete each test without MFA, SMS, email confirmation, or private-network access.

#### Global

Choose the countries or regions where the plugin should be available. Only select locations where the publisher, product, support process, and legal terms are ready for users.

#### Submit

Review the full draft before submitting.

In the release notes, summarize:

- What the plugin does.
- Whether this is an initial submission or an update.
- What changed since the prior submitted version, if any.
- Anything reviewers should know about test credentials, expected data, or setup.

Complete the policy attestations only after confirming the listing, server, skills, prompts, tests, and availability are accurate. Then select **Submit for Review**.

### Public publishing flow

Submitting a plugin starts review; it doesn't publish the plugin immediately. For public availability, the flow is:

1. Submit the plugin through the plugin submission portal.
2. OpenAI reviews the submission. Review timelines may vary as OpenAI builds and scales the review process.
3. After OpenAI approves the plugin, the developer chooses when to publish it and publishes it from the portal.
4. After publication, the plugin appears in the universal plugin directory available in both ChatGPT and Codex.

No separate apps directory exists. Published apps, skills-only plugins, and app-plus-skills plugins all appear in the plugin directory.

### Final checklist

Before submitting, confirm:

- The submitter has **Apps Management** write access.
- The publisher has a verified developer or business identity.
- The MCP server uses a public, production URL.
- Plugins that contain apps define a content security policy for the exact domains the app fetches from.
- Reviewer credentials work without MFA, email confirmation, SMS confirmation, or private-network access.
- Tool names, descriptions, schemas, and annotations match actual behavior.
- Every tool has accurate `readOnlyHint`, `openWorldHint`, and `destructiveHint` values.
- Tool responses don't include unnecessary personal data, auth secrets, debug payloads, internal identifiers, or undisclosed user-related fields.
- You tested the skills locally with the final file tree.
- Starter prompts show realistic user workflows.
- The submission includes five positive and three negative test cases.
- Privacy policy, terms, support, and website URLs are public and match the publisher identity.

---

## Hooks

Run deterministic scripts during the Codex lifecycle

Hooks represent an extensibility framework for Codex that allows developers to inject custom scripts into the agentic loop. This enables functionality like sending conversations to custom logging systems, scanning for accidentally pasted API keys, automatically creating persistent memories from conversations, running validation checks at turn completion, and customizing prompts based on directory context.

### Runtime Behavior

Key operational characteristics include:

- Multiple matching hooks from different files all execute
- Command hooks for the same event launch concurrently, preventing one hook from blocking another
- Non-managed command hooks require review and trust before execution
- Turn-scoped hooks (`PreToolUse`, `PermissionRequest`, `PostToolUse`, `PreCompact`, `PostCompact`, `UserPromptSubmit`, `SubagentStop`, `Stop`) operate at turn scope
- Session-scoped hooks (`SessionStart`, `SubagentStart`) operate at thread or subagent-start scope

### Hook Discovery Locations

Codex discovers hooks in these locations:

- `~/.codex/hooks.json`
- `~/.codex/config.toml`
- `<repo>/.codex/hooks.json`
- `<repo>/.codex/config.toml`

Hooks can also be bundled with plugins. Multiple hook sources load together without replacement. Project-local hooks only load when the project's `.codex/` layer is trusted.

### Trust and Review

Before non-managed command hooks execute, Codex requires explicit review and trust. The system records trust against the hook's hash, so modified hooks require re-review. Use the `/hooks` CLI command to inspect sources, review changes, trust hooks, or disable individual hooks.

Managed hooks from system, MDM, cloud, or `requirements.toml` sources are automatically trusted by policy and cannot be disabled.

### Configuration Structure

Hooks organize into three levels:

1. Hook event (e.g., `PreToolUse`, `PostToolUse`, `Stop`)
2. Matcher group determining when the event matches
3. One or more hook handlers executing when matched

#### JSON Example

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

#### TOML Equivalent

```toml
[[hooks.PreToolUse]]
matcher = "^Bash$"

[[hooks.PreToolUse.hooks]]
type = "command"
command = '/usr/bin/python3 "$(git rev-parse --show-toplevel)/.codex/hooks/pre_tool_use_policy.py"'
timeout = 30
statusMessage = "Checking Bash command"
```

### Configuration Notes

- `timeout` is measured in seconds, defaulting to 600 if omitted
- `statusMessage` is optional
- `commandWindows` provides Windows-specific command overrides
- Only `type: "command"` handlers currently execute
- Commands run with the session's working directory as their context
- Repository-local hooks should resolve from git root for stability

### Disabling Hooks

To disable hooks in `config.toml`:

```toml
[features]
hooks = false
```

Administrators can enforce this setting through `requirements.toml`.

### Managed Hooks in requirements.toml

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

### Plugin-Bundled Hooks

When enabled, plugins can load lifecycle hooks alongside user and project hooks. By default, Codex searches for `hooks/hooks.json` in the plugin root. Plugin manifests can override this with a `hooks` entry in `.codex-plugin/plugin.json`:

```json
{
  "name": "repo-policy",
  "hooks": "./hooks/hooks.json"
}
```

Manifest paths must be relative and remain within the plugin root. Plugin hook commands receive environment variables including `PLUGIN_ROOT` and `PLUGIN_DATA`, plus `CLAUDE_PLUGIN_ROOT` and `CLAUDE_PLUGIN_DATA` for compatibility.

Plugin hooks use the same event schema as other hooks and require trust review before execution.

### Matcher Patterns

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

### Common Input Fields

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

### Common Output Fields

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

### Hook Events

#### SessionStart

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

#### SubagentStart

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

#### PreToolUse

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

#### PermissionRequest

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

#### PostToolUse

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

#### PreCompact

Runs before conversation compaction. The `matcher` filters by `trigger` (manual, auto).

Input fields:

| Field | Type | Meaning |
|-------|------|---------|
| `turn_id` | string | Active Codex turn identifier |
| `trigger` | string | Compaction source |

JSON output supports common fields. Returning `continue: false` prevents compaction.

#### PostCompact

Runs after conversation compaction. The `matcher` filters by `trigger` (manual, auto).

Input fields:

| Field | Type | Meaning |
|-------|------|---------|
| `turn_id` | string | Active Codex turn identifier |
| `trigger` | string | Compaction source |

JSON output supports common fields. Returning `continue: false` stops after compaction.

#### UserPromptSubmit

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

#### SubagentStop

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

#### Stop

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

### Schemas

The linked schemas on the main branch may include fields not in the current release. For exact current wire format, see generated schemas in the Codex GitHub repository.

---

## Codex environments

### Main Content

Choose where a Codex task runs and how its files stay isolated

In the ChatGPT desktop app, open the ChatGPT dropdown and select **Codex**. When starting a Codex task, choose where it runs:

- **Local**: work directly in your current project directory.
- **Worktree**: isolate changes in a Git worktree. [Learn more](/codex/environments/git-worktrees).
- **Cloud**: run remotely in a configured cloud environment.

Both **Local** and **Worktree** tasks run on your computer.

For the full glossary and concepts, explore the [concepts section](/codex/prompting).

---

### Navigation Note

The page includes a "Next" link directing to "Local environments" documentation at `/codex/environments/local-environment`, as well as a "Docs agent" feature for asking AI questions about the documentation.

---

## Local environments

Configure common actions and setup scripts for worktrees

Local environments enable you to set up initialization steps for worktrees and define frequently-used actions within a project.

**Availability:** Local environments are exclusive to Codex in the ChatGPT desktop app. Access Codex before configuring or using a local environment.

You can manage your local environments via the ChatGPT desktop app settings pane. This configuration can be version-controlled in your project's Git repository for team sharing.

Codex maintains this setup in the `.codex` folder at your project root. For multi-project repositories, navigate to the project directory containing the shared `.codex` folder.

### Setup scripts

Worktrees operate in distinct directories from your local tasks, which may leave your project incomplete with missing dependencies or untracked files. "Setup scripts run automatically when Codex creates a new worktree at the start of a new task."

Execute commands necessary for environment configuration, including dependency installation or build processes.

**Example for TypeScript projects:**

```
npm install
npm run build
```

For platform-specific setups, create distinct scripts for macOS, Windows, or Linux to replace the default.

### Actions

Define frequent operations like launching your development server or executing tests. These appear in the ChatGPT desktop app toolbar for convenient access and run within the app's integrated terminal.

Actions eliminate repetitive typing for common tasks such as project builds or development server startup.

**Example for Node.js projects:**

```
npm start
```

For platform-specific commands, establish separate scripts for macOS, Windows, and Linux.

Each action requires an associated icon for identification.

### Use built-in Git tools

Codex provides standard Git operations alongside local projects and worktrees in the ChatGPT desktop app. The diff view displays current changes and enables inline comments for Codex to resolve. You can stage/revert chunks or files, commit, push branches, or open pull requests without leaving the app.

Use the integrated terminal for Git operations unavailable in the app interface. Start tasks in worktrees to separate concurrent modifications from your main checkout.

---

## Cloud environments

Customize dependencies and tools for Codex

Use environments to control what Codex installs and runs during cloud tasks. For example, you can add dependencies, install tools like linters and formatters, and set environment variables.

Configure environments in Codex settings.

### How Codex cloud tasks run

Here's what happens when you submit a task:

1. Codex creates a container and checks out your repo at the selected branch or commit SHA.
2. Codex runs your setup script, plus an optional maintenance script when a cached container is resumed.
3. Codex applies your internet access settings. Setup scripts run with internet access. Agent internet access is off by default, but you can enable limited or unrestricted access if needed.
4. The agent runs terminal commands in a loop. It edits code, runs checks, and tries to validate its work. If your repo includes `AGENTS.md`, the agent uses it to find project-specific lint and test commands.
5. When the agent finishes, it shows its answer and a diff of any files it changed. You can open a PR or ask follow-up questions.

### Default universal image

The Codex agent runs in a default container image called `universal`, which comes pre-installed with common languages, packages, and tools.

In environment settings, select **Set package versions** to pin versions of Python, Node.js, and other runtimes.

For details on what's installed, see openai/codex-universal for a reference Dockerfile and an image that can be pulled and tested locally.

While `codex-universal` comes with languages pre-installed for speed and convenience, you can also install additional packages to the container using setup scripts.

### Environment variables and secrets

**Environment variables** are set for the full duration of the task (including setup scripts and the agent phase).

**Secrets** are similar to environment variables, except:

- They are stored with an additional layer of encryption and are only decrypted for task execution.
- They are only available to setup scripts. For security reasons, secrets are removed before the agent phase starts.

### Automatic setup

For projects using common package managers (`npm`, `yarn`, `pnpm`, `pip`, `pipenv`, and `poetry`), Codex can automatically install dependencies and tools.

### Manual setup

If your development setup is more complex, you can also provide a custom setup script. For example:

```bash
# Install type checker
pip install pyright

# Install dependencies
poetry install --with test
pnpm install
```

Setup scripts run in a separate Bash session from the agent, so commands like `export` do not persist into the agent phase. To persist environment variables, add them to `~/.bashrc` or configure them in environment settings.

### Container caching

Codex caches container state for up to 12 hours to speed up new tasks and follow-ups.

When an environment is cached:

- Codex clones the repository and checks out the default branch.
- Codex runs the setup script and caches the resulting container state.

When a cached container is resumed:

- Codex checks out the branch specified for the task.
- Codex runs the maintenance script (optional). This is useful when the setup script ran on an older commit and dependencies need to be updated.

Codex automatically invalidates the cache if you change the setup script, maintenance script, environment variables, or secrets. If your repo changes in a way that makes the cached state incompatible, select **Reset cache** on the environment page.

For Business and Enterprise users, caches are shared across all users who have access to the environment. Invalidating the cache will affect all users of the environment in your workspace.

### Internet access and network proxy

Internet access is available during the setup script phase to install dependencies. During the agent phase, internet access is off by default, but you can configure limited or unrestricted access.

Environments run behind an HTTP/HTTPS network proxy for security and abuse prevention purposes. All outbound internet traffic passes through this proxy.

---

## Worktrees

Worktrees enable Codex to "run multiple independent tasks in the same project without interfering with each other" in the ChatGPT desktop app, particularly useful for Git repositories where scheduled tasks operate on dedicated background worktrees.

### What's a worktree

Built on Git's worktree functionality, this feature creates a secondary repository copy while sharing Git metadata. Each maintains its own files but references the same commit history and branches.

### Key Terminology

- **Local checkout**: Your original repository
- **Worktree**: A Git worktree created from your local checkout within the app
- **Handoff**: The process moving tasks between Local and Worktree environments

### Primary Benefits

1. Work in parallel without disrupting your current Local setup
2. Queue background tasks while maintaining foreground focus
3. Transition tasks to Local for inspection or collaboration

### Getting Started

Tasks require a Git repository. The process involves selecting "Worktree," choosing a starting branch, submitting your prompt, then deciding whether to continue on the worktree or hand off to Local.

### Working Between Environments

Since Git restricts simultaneous branch checkouts across locations, Handoff manages safe transitions by handling required Git operations. Two common workflows exist: working exclusively on the worktree or handing tasks off to Local for IDE inspection or testing.

### Advanced Details

Codex maintains managed worktrees in `$CODEX_HOME/worktrees`, typically dedicated to single tasks and retained in detached HEAD states. A `.worktreeinclude` file can specify ignored files for copying into new worktrees, such as environment configuration files.

The system automatically retains 15 recent Codex-managed worktrees by default, with automatic deletion during archiving or when storage limits require cleanup.

---

## Codex SDK

Programmatically control local Codex agents

If you use Codex through Codex CLI, the IDE extension, or Codex cloud, you can also control it programmatically.

Use the SDK when you need to:

- Control Codex as part of your CI/CD pipeline
- Create your own agent that can engage with Codex to perform complex engineering tasks
- Build Codex into your own internal tools and workflows
- Integrate Codex within your own application

Use the Codex SDK for coding-focused Codex threads. If Codex is one specialist inside a broader orchestrated workflow, run Codex CLI as an MCP server and orchestrate it with the Agents SDK.

### TypeScript library

The TypeScript library provides a way to control Codex from within your application that's more comprehensive and flexible than non-interactive mode.

Use the library server-side; it requires Node.js 18 or later.

#### Installation

To get started, install the Codex SDK using `npm`:

```
npm install @openai/codex-sdk
```

#### Usage

Start a thread with Codex and run it with your prompt.

```typescript
import { Codex } from "@openai/codex-sdk";

const codex = new Codex();
const thread = codex.startThread();
const result = await thread.run(
  "Make a plan to diagnose and fix the CI failures"
);

console.log(result.finalResponse);
```

Call `run()` again to continue on the same thread, or resume a past thread by providing a thread ID.

```typescript
// running the same thread
const result = await thread.run("Implement the plan");

console.log(result.finalResponse);

// resuming past thread

const threadId = "<thread-id>";
const thread2 = codex.resumeThread(threadId);
const result2 = await thread2.run("Pick up where you left off");

console.log(result2.finalResponse);
```

For more details, check out the TypeScript repo.

### Python library

The Python SDK controls the local Codex app-server over JSON-RPC. It requires Python 3.10 or later. Published SDK builds include a pinned Codex CLI runtime dependency.

#### Installation

To install the SDK run:

```
pip install openai-codex
```

Published SDK builds automatically use their pinned runtime. Pass `CodexConfig(codex_bin=...)` only when you intentionally want to run against a specific local Codex executable.

While the Python SDK is in beta, `pip install openai-codex` selects the latest published beta build. After a stable SDK release exists, use `pip install --pre openai-codex` to opt in to newer prerelease builds.

#### Usage

Start Codex, create a thread, and run a prompt:

```python
from openai_codex import Codex, Sandbox

with Codex() as codex:
    thread = codex.thread_start(
        model="gpt-5.4",
        sandbox=Sandbox.workspace_write,
    )
    result = thread.run("Make a plan to diagnose and fix the CI failures")
    print(result.final_response)
```

Use `AsyncCodex` when your application is already asynchronous:

```python
import asyncio

from openai_codex import AsyncCodex


async def main() -> None:
    async with AsyncCodex() as codex:
        thread = await codex.thread_start(model="gpt-5.4")
        result = await thread.run("Implement the plan")
        print(result.final_response)


asyncio.run(main())
```

#### Sandbox presets

Use the same `Sandbox` presets when creating a thread or changing its filesystem access for a later turn:

```python
from openai_codex import Codex, Sandbox

with Codex() as codex:
    thread = codex.thread_start(sandbox=Sandbox.workspace_write)
    thread.run("Make the requested change.")
    review = thread.run("Review the diff only.", sandbox=Sandbox.read_only)
```

Available presets:

- `Sandbox.read_only`: Read files without allowing writes.
- `Sandbox.workspace_write`: Read files and write inside the workspace and configured writable roots.
- `Sandbox.full_access`: Run without filesystem access restrictions.

When you omit `sandbox=`, app-server uses its configured default. A sandbox passed to `run(...)` or `turn(...)` applies to that turn and later turns on the thread.

For more details, check out the Python repo.

---

## Codex App Server

Embed Codex into your product with the app-server protocol

Codex app-server is the interface Codex uses to power rich clients (for example, the Codex VS Code extension). Use it when you want a deep integration inside your own product: authentication, conversation history, approvals, and streamed agent events. The app-server implementation is open source in the Codex GitHub repository ([openai/codex/codex-rs/app-server](https://github.com/openai/codex/tree/main/codex-rs/app-server)).

### Connect the CLI terminal UI

Remote terminal UI mode lets you run app-server on one machine and connect the Codex CLI terminal interface from another. Start a WebSocket listener:

```bash
codex app-server --listen ws://127.0.0.1:4500
```

Then connect the terminal UI:

```bash
codex --remote ws://127.0.0.1:4500
```

For a non-local connection, configure WebSocket authentication and put the connection behind TLS. Store the bearer token in an environment variable and pass its name instead of putting the token on the command line:

```bash
export CODEX_REMOTE_TOKEN="$(cat "$HOME/.codex/app-server-token")"
codex --remote wss://remote-host:4500 \
  --remote-auth-token-env CODEX_REMOTE_TOKEN
```

The `--remote` option accepts `ws://`, `wss://`, `unix://`, and `unix://PATH` endpoints. Use plain WebSockets only for localhost or an SSH port-forwarded connection.

### Protocol

Like MCP, `codex app-server` supports bidirectional communication using JSON-RPC 2.0 messages (with the `"jsonrpc":"2.0"` header omitted on the wire).

#### Supported transports:

- `stdio` (`--listen stdio://`, default): newline-delimited JSON (JSONL).
- `websocket` (`--listen ws://IP:PORT`, experimental and unsupported): one JSON-RPC message per WebSocket text frame.
- Unix socket (`--listen unix://` or `--listen unix://PATH`): WebSocket connections over Codex's default app-server control socket or a custom Unix socket path, using the standard HTTP Upgrade handshake.
- `off` (`--listen off`): don't expose a local transport.

When you run with `--listen ws://IP:PORT`, the same listener also serves basic HTTP health probes:

- `GET /readyz` returns `200 OK` once the listener accepts new connections.
- `GET /healthz` returns `200 OK` when the request doesn't include an `Origin` header.
- Requests with an `Origin` header are rejected with `403 Forbidden`.

WebSocket transport is experimental and unsupported. Local listeners such as `ws://127.0.0.1:PORT` are appropriate for localhost and SSH port-forwarding workflows. Non-loopback WebSocket listeners currently allow unauthenticated connections by default during rollout, so configure WebSocket auth before exposing one remotely.

#### Supported WebSocket auth flags:

- `--ws-auth capability-token --ws-token-file /absolute/path`
- `--ws-auth capability-token --ws-token-sha256 HEX`
- `--ws-auth signed-bearer-token --ws-shared-secret-file /absolute/path`

For signed bearer tokens, you can also set `--ws-issuer`, `--ws-audience`, and `--ws-max-clock-skew-seconds`. Clients present the credential as `Authorization: Bearer <token>` during the WebSocket handshake, and app-server enforces auth before JSON-RPC `initialize`.

Prefer `--ws-token-file` over passing raw bearer tokens on the command line. Use `--ws-token-sha256` only when the client keeps the raw high-entropy token in a separate local secret store; the hash is only a verifier, and clients still need the original token.

In WebSocket mode, app-server uses bounded queues. When request ingress is full, the server rejects new requests with JSON-RPC error code `-32001` and message `"Server overloaded; retry later."` Clients should retry with an exponentially increasing delay and jitter.

### Message schema

Requests include `method`, `params`, and `id`:

```json
{ "method": "thread/start", "id": 10, "params": { "model": "gpt-5.4" } }
```

Responses echo the `id` with either `result` or `error`:

```json
{ "id": 10, "result": { "thread": { "id": "thr_123" } } }
```

```json
{ "id": 10, "error": { "code": 123, "message": "Something went wrong" } }
```

Notifications omit `id` and use only `method` and `params`:

```json
{ "method": "turn/started", "params": { "turn": { "id": "turn_456" } } }
```

You can generate a TypeScript schema or a JSON Schema bundle from the CLI. Each output is specific to the Codex version you ran, so the generated artifacts match that version exactly:

```bash
codex app-server generate-ts --out ./schemas
codex app-server generate-json-schema --out ./schemas
```

### Getting started

1. Start the server with `codex app-server` (default stdio transport), `codex app-server --listen ws://127.0.0.1:4500` (TCP WebSocket), or `codex app-server --listen unix://` (default Unix socket).
2. Connect a client over the selected transport, then send `initialize` followed by the `initialized` notification.
3. Start a thread and a turn, then keep reading notifications from the active transport stream.

Example (Node.js / TypeScript):

```typescript
import { spawn } from "node:child_process";
import readline from "node:readline";

const proc = spawn("codex", ["app-server"], {
  stdio: ["pipe", "pipe", "inherit"],
});
const rl = readline.createInterface({ input: proc.stdout });

const send = (message: unknown) => {
  proc.stdin.write(`${JSON.stringify(message)}\n`);
};

let threadId: string | null = null;

rl.on("line", (line) => {
  const msg = JSON.parse(line) as any;
  console.log("server:", msg);

  if (msg.id === 1 && msg.result?.thread?.id && !threadId) {
    threadId = msg.result.thread.id;
    send({
      method: "turn/start",
      id: 2,
      params: {
        threadId,
        input: [{ type: "text", text: "Summarize this repo." }],
      },
    });
  }
});

send({
  method: "initialize",
  id: 0,
  params: {
    clientInfo: {
      name: "my_product",
      title: "My Product",
      version: "0.1.0",
    },
  },
});
send({ method: "initialized", params: {} });
send({ method: "thread/start", id: 1, params: { model: "gpt-5.4" } });
```

### Core primitives

- Thread: A conversation between a user and the Codex agent. Threads contain turns.
- Turn: A single user request and the agent work that follows. Turns contain items and stream incremental updates.
- Item: A unit of input or output (user message, agent message, command runs, file change, tool call, and more).

Use the thread APIs to create, list, or archive conversations. Drive a conversation with turn APIs and stream progress via turn notifications.

### Lifecycle overview

- Initialize once per connection: Immediately after opening a transport connection, send an `initialize` request with your client metadata, then emit `initialized`. The server rejects any request on that connection before this handshake.
- Start (or resume) a thread: Call `thread/start` for a new conversation, `thread/resume` to continue an existing one, or `thread/fork` to branch history into a new thread id.
- Begin a turn: Call `turn/start` with the target `threadId` and user input. Optional fields override model, personality, `cwd`, sandbox policy, and more.
- Steer an active turn: Call `turn/steer` to append user input to the currently in-flight turn without creating a new turn.
- Stream events: After `turn/start`, keep reading notifications on stdout: `thread/archived`, `thread/unarchived`, `item/started`, `item/completed`, `item/agentMessage/delta`, tool progress, and other updates.
- Finish the turn: The server emits `turn/completed` with final status when the model finishes or after a `turn/interrupt` cancellation.

### Initialization

Clients must send a single `initialize` request per transport connection before invoking any other method on that connection, then acknowledge with an `initialized` notification. Requests sent before initialization receive a `Not initialized` error, and repeated `initialize` calls on the same connection return `Already initialized`.

The server returns the user agent string it will present to upstream services plus `platformFamily` and `platformOs` values that describe the runtime target. Set `clientInfo` to identify your integration.

`initialize.params.capabilities` also supports these client capabilities:

- `optOutNotificationMethods`: exact notification method names to suppress for this connection. Matching is exact (no wildcards or prefixes); unknown names are accepted and ignored.
- `requestAttestation`: opt into the server-initiated `attestation/generate` request. Desktop hosts that provide upstream attestation respond with an opaque `{ "token": "..." }` value.
- `mcpServerOpenaiFormElicitation`: allow downstream MCP servers to send the OpenAI extended-form variant of `mcpServer/elicitation/request`.

Important: Use `clientInfo.name` to identify your client for the OpenAI Compliance Logs Platform. If you are developing a new Codex integration intended for enterprise use, please contact OpenAI to get it added to a known clients list.

Example (from the Codex VS Code extension):

```json
{
  "method": "initialize",
  "id": 0,
  "params": {
    "clientInfo": {
      "name": "codex_vscode",
      "title": "Codex VS Code Extension",
      "version": "0.1.0"
    }
  }
}
```

Example with notification opt-out:

```json
{
  "method": "initialize",
  "id": 1,
  "params": {
    "clientInfo": {
      "name": "my_client",
      "title": "My Client",
      "version": "0.1.0"
    },
    "capabilities": {
      "experimentalApi": true,
      "optOutNotificationMethods": ["thread/started", "item/agentMessage/delta"]
    }
  }
}
```

### Experimental API opt-in

Some app-server methods and fields are intentionally gated behind `experimentalApi` capability.

- Omit `capabilities` (or set `experimentalApi` to `false`) to stay on the stable API surface, and the server rejects experimental methods/fields.
- Set `capabilities.experimentalApi` to `true` to enable experimental methods and fields.

```json
{
  "method": "initialize",
  "id": 1,
  "params": {
    "clientInfo": {
      "name": "my_client",
      "title": "My Client",
      "version": "0.1.0"
    },
    "capabilities": {
      "experimentalApi": true
    }
  }
}
```

If a client sends an experimental method or field without opting in, app-server rejects it with:

`<descriptor> requires experimentalApi capability`

### API overview

- `thread/start`: create a new thread; emits `thread/started` and automatically subscribes you to turn/item events for that thread.
- `thread/resume`: reopen an existing thread by id so later `turn/start` calls append to it.
- `thread/fork`: fork a thread into a new thread id by copying stored history. Pass `lastTurnId` to copy history through that turn and omit later turns. Emits `thread/started` for the new thread; returned threads include `forkedFromId` when available.
- `thread/read`: read a stored thread by id without resuming it; set `includeTurns` to return full turn history. Returned `thread` objects include runtime `status`.
- `thread/list`: page through stored thread logs; supports cursor-based pagination plus `modelProviders`, `sourceKinds`, `archived`, `cwd`, `useStateDbOnly`, `searchTerm`, and experimental `parentThreadId` or `ancestorThreadId` filters. Returned `thread` objects include runtime `status`.
- `thread/turns/list`: experimental; page through a stored thread's turn history without resuming it. `itemsView` controls whether turn items are omitted, summarized, or fully loaded.
- `thread/items/list`: experimental; page through persisted thread items, optionally restricted to one `turnId`. The active thread store must support item pagination.
- `thread/loaded/list`: list the thread ids currently loaded in memory.
- `thread/name/set`: set or update a thread's user-facing name for a loaded thread or a persisted rollout; emits `thread/name/updated`.
- `thread/goal/set`: set the goal for a thread; emits `thread/goal/updated`.
- `thread/goal/get`: read the current goal for a thread.
- `thread/goal/clear`: clear the goal for a thread; emits `thread/goal/cleared`.
- `thread/metadata/update`: patch SQLite-backed stored thread metadata; currently supports persisted `gitInfo`.
- `thread/archive`: move a thread's log file into the archived directory and attempt to archive spawned descendant thread logs that aren't already archived; returns `{}` on success and emits `thread/archived` for each archived thread.
- `thread/delete`: permanently delete a persisted active or archived thread and any spawned descendant threads; returns `{}` on success and emits `thread/deleted` for each deleted thread.
- `thread/unsubscribe`: unsubscribe this connection from thread turn/item events. If this was the last subscriber, the server unloads the thread after a no-subscriber inactivity grace period and emits `thread/closed`.
- `thread/unarchive`: restore an archived thread rollout back into the active sessions directory; returns the restored `thread` and emits `thread/unarchived`.
- `thread/status/changed`: notification emitted when a loaded thread's runtime `status` changes.
- `thread/compact/start`: trigger conversation history compaction for a thread; returns `{}` immediately while progress streams via `turn/*` and `item/*` notifications.
- `thread/shellCommand`: run a user-initiated shell command against a thread. This runs outside the sandbox with full access and doesn't inherit the thread sandbox policy.
- `thread/backgroundTerminals/clean`: stop all running background terminals for a thread (experimental; requires `capabilities.experimentalApi`).
- `thread/backgroundTerminals/list`: list running background terminals for a loaded thread (experimental; requires `capabilities.experimentalApi`).
- `thread/backgroundTerminals/terminate`: terminate one running background terminal by app-server `processId` (experimental; requires `capabilities.experimentalApi`).
- `thread/rollback`: deprecated; drop the last N turns from the in-memory context and persist a rollback marker; returns the updated `thread`.
- `turn/start`: add user input to a thread and begin Codex generation; responds with the initial `turn` and streams events. For `collaborationMode`, `settings.developer_instructions: null` means "use built-in instructions for the selected mode."
- `thread/inject_items`: append raw Responses API items to a loaded thread's model-visible history without starting a user turn.
- `turn/steer`: append user input to the active in-flight turn for a thread; returns the accepted `turnId`.
- `turn/interrupt`: request cancellation of an in-flight turn; success is `{}` and the turn ends with `status: "interrupted"`.
- `review/start`: kick off the Codex reviewer for a thread; emits `enteredReviewMode` and `exitedReviewMode` items.
- `command/exec`: run a single command under the server sandbox without starting a thread/turn.
- `command/exec/write`: write `stdin` bytes to a running `command/exec` session or close `stdin`.
- `command/exec/resize`: resize a running PTY-backed `command/exec` session.
- `command/exec/terminate`: stop a running `command/exec` session.
- `command/exec/outputDelta` (notify): emitted for base64-encoded stdout/stderr chunks from a streaming `command/exec` session.
- `process/spawn`: start an explicit process session outside Codex's sandbox (experimental; requires `capabilities.experimentalApi`).
- `process/writeStdin`: write stdin bytes to a running `process/spawn` session or close stdin (experimental).
- `process/resizePty`: resize a running PTY-backed process session (experimental).
- `process/kill`: terminate a running process session (experimental).
- `process/outputDelta` and `process/exited` (notify): emitted for streaming process output and process exit status (experimental).
- `model/list`: list available models (set `includeHidden: true` to include entries with `hidden: true`) with effort options, optional `upgrade`, and `inputModalities`.
- `modelProvider/capabilities/read`: read provider capability bounds for model/provider combinations.
- `experimentalFeature/list`: list feature flags with lifecycle stage metadata and cursor pagination.
- `experimentalFeature/enablement/set`: patch in-memory runtime settings for supported feature keys such as `apps` and `plugins`.
- `environment/info`: experimental; connect to a configured execution environment and return its shell plus default working directory.
- `permissionProfile/list`: list beta permission profiles and whether effective requirements allow them, with cursor pagination.
- `collaborationMode/list`: list collaboration mode presets (experimental, no pagination).
- `skills/list`: list skills for one or more `cwd` values (supports `forceReload` and optional `perCwdExtraUserRoots`).
- `skills/extraRoots/set`: replace the process-level extra roots used to discover standalone skills without persisting them.
- `skills/changed` (notify): emitted when watched local skill files change.
- `hooks/list`: list discovered lifecycle hooks for one or more `cwd` values.
- `marketplace/add`: add a remote plugin marketplace and persist it into the user's marketplace config.
- `marketplace/remove`: remove a configured marketplace and its installed marketplace root when present.
- `marketplace/upgrade`: refresh a configured Git marketplace, or all configured Git marketplaces when you omit the marketplace name.
- `plugin/list`: under development; list discovered plugin marketplaces and plugin state, including install/auth policy metadata, marketplace load errors, featured plugin ids, and local, Git, package-registry, or remote plugin source metadata. Summaries can include remote `version`, local `localVersion`, structured light/dark icons, and `installPolicySource`, which can be `null`, `WORKSPACE_SETTING`, or `IMPLICIT_CANONICAL_APP` for current remote rows. Don't call this method from production clients yet.
- `plugin/read`: under development; read one plugin by marketplace path or remote marketplace name and plugin name, including bundled skills, apps, MCP server names, and a remote plugin `shareUrl` when the remote catalog provides one. Don't call this method from production clients yet.
- `plugin/install`: under development; install a plugin from a marketplace path or remote marketplace name. Don't call this method from production clients yet.
- `plugin/uninstall`: under development; uninstall an installed plugin. Don't call this method from production clients yet.
- `plugin/skill/read`: read remote plugin skill Markdown on demand by remote marketplace, plugin id, and skill name.
- `app/list`: list available apps (connectors) with pagination plus accessibility/enabled metadata.
- `skills/config/write`: enable or disable skills by path.
- `mcpServer/oauth/login`: start an OAuth login for a configured MCP server; returns an authorization URL and emits `mcpServer/oauthLogin/completed` on completion.
- `tool/requestUserInput`: prompt the user with 1 to 3 short questions for a tool call (experimental); questions can set `isOther` for a free-form option.
- `mcpServer/elicitation/request` (server request): ask the client for structured form input or confirmation of a URL flow requested by an MCP server.
- `item/permissions/requestApproval` (server request): ask the client to grant a subset of network or filesystem permissions requested by the built-in `request_permissions` tool.
- `config/mcpServer/reload`: reload MCP server configuration from disk and queue a refresh for loaded threads.
- `mcpServerStatus/list`: list MCP servers, tools, resources, and auth status (cursor + limit pagination). Use `detail: "full"` for full data or `detail: "toolsAndAuthOnly"` to omit resources.
- `mcpServer/resource/read`: read a single MCP resource through an initialized MCP server.
- `mcpServer/tool/call`: call a tool on a thread's configured MCP server.
- `mcpServer/startupStatus/updated` (notify): emitted when a configured MCP server's startup status changes for a loaded thread.
- `windowsSandbox/setupStart`: start Windows sandbox setup for `elevated` or `unelevated` mode; returns quickly and later emits `windowsSandbox/setupCompleted`.
- `feedback/upload`: submit a feedback report (classification plus optional reason/logs and conversation id, plus optional `extraLogFiles` attachments).
- `config/read`: fetch the effective configuration on disk after resolving configuration layering.
- `externalAgentConfig/detect`: detect external-agent artifacts that can be migrated with `includeHome` and optional `cwds`; each detected item includes `cwd` (`null` for home).
- `externalAgentConfig/import`: apply selected external-agent migration items by passing explicit `migrationItems` with `cwd` (`null` for home). Supported item types include config, skills, `AGENTS.md`, plugins, MCP server config, subagents, hooks, commands, and sessions; non-empty imports emit `externalAgentConfig/import/progress` and `externalAgentConfig/import/completed` as work finishes. Plugin and session imports can complete asynchronously.
- `config/value/write`: write a single configuration key/value to the user's `config.toml` on disk.
- `config/batchWrite`: apply configuration edits atomically to the user's `config.toml` on disk.
- `configRequirements/read`: fetch requirements from `requirements.toml` and/or MDM, including allow-lists, pinned `featureRequirements`, and residency/network requirements (or `null` if you haven't set any up).
- `fs/readFile`, `fs/writeFile`, `fs/createDirectory`, `fs/getMetadata`, `fs/readDirectory`, `fs/remove`, `fs/copy`, `fs/watch`, `fs/unwatch`, and `fs/changed` (notify): operate on absolute filesystem paths through the app-server v2 filesystem API.

Plugin summaries include a `source` union. Local plugins return `{ "type": "local", "path": ... }`, Git-backed marketplace entries return `{ "type": "git", "url": ..., "path": ..., "refName": ..., "sha": ... }`, package-registry entries return `{ "type": "npm", "package": ..., "version": ..., "registry": ... }`, and remote catalog entries return `{ "type": "remote" }`. For remote-only catalog entries, `PluginMarketplaceEntry.path` can be `null`; pass `remoteMarketplaceName` instead of `marketplacePath` when reading or installing those plugins.

### Models

#### List models (`model/list`)

Call `model/list` to discover available models and their capabilities before rendering model or personality selectors.

```json
{ "method": "model/list", "id": 6, "params": { "limit": 20, "includeHidden": false } }
```

```json
{ "id": 6, "result": {
  "data": [{
    "id": "gpt-5.4",
    "model": "gpt-5.4",
    "displayName": "GPT-5.4",
    "hidden": false,
    "defaultReasoningEffort": "medium",
    "supportedReasoningEfforts": [{
      "reasoningEffort": "low",
      "description": "Lower latency"
    }],
    "inputModalities": ["text", "image"],
    "supportsPersonality": true,
    "isDefault": true
  }],
  "nextCursor": null
} }
```

Each model entry can include:

- `supportedReasoningEfforts`: supported effort options for the model.
- `defaultReasoningEffort`: suggested default effort for clients.
- `upgrade`: optional recommended upgrade model id for migration prompts in clients.
- `upgradeInfo`: optional upgrade metadata for migration prompts in clients.
- `hidden`: whether the model is hidden from the default picker list.
- `inputModalities`: supported input types for the model (for example `text`, `image`).
- `supportsPersonality`: whether the model supports personality-specific instructions such as `/personality`.
- `isDefault`: whether the model is the recommended default.

By default, `model/list` returns picker-visible models only. Set `includeHidden: true` if you need the full list and want to filter on the client side using `hidden`.

When `inputModalities` is missing (older model catalogs), treat it as `["text", "image"]` for backward compatibility.

#### List experimental features (`experimentalFeature/list`)

Use this endpoint to discover feature flags with metadata and lifecycle stage:

```json
{ "method": "experimentalFeature/list", "id": 7, "params": { "limit": 20 } }
```

```json
{ "id": 7, "result": {
  "data": [{
    "name": "unified_exec",
    "stage": "beta",
    "displayName": "Unified exec",
    "description": "Use the unified PTY-backed execution tool.",
    "announcement": "Beta rollout for improved command execution reliability.",
    "enabled": false,
    "defaultEnabled": false
  }],
  "nextCursor": null
} }
```

`stage` can be `beta`, `underDevelopment`, `stable`, `deprecated`, or `removed`. For non-beta flags, `displayName`, `description`, and `announcement` may be `null`.

#### Inspect an execution environment (experimental)

Use `environment/info` to inspect a configured remote environment before starting work there. The method requires `capabilities.experimentalApi = true`.

```json
{ "method": "environment/info", "id": 8, "params": { "environmentId": "devbox" } }
```

```json
{ "id": 8, "result": {
  "shell": { "name": "zsh", "path": "/bin/zsh" },
  "cwd": "file:///workspace/project"
} }
```

`cwd` can be `null`. When present, it's a canonical `file:` URI that uses the environment's native path syntax. Unknown environment IDs and connection or protocol failures return request errors.

### Threads

- `thread/read` reads a stored thread without subscribing to it; set `includeTurns` to include turns.
- `thread/turns/list` is experimental and pages through a stored thread's turn history without resuming it. Use `itemsView` to choose whether turn items are omitted, summarized, or fully loaded.
- `thread/items/list` is experimental and pages through persisted thread items, optionally restricted to one turn.
- `thread/list` supports cursor pagination plus `modelProviders`, `sourceKinds`, `archived`, `cwd`, `useStateDbOnly`, `searchTerm`, and experimental `parentThreadId` or `ancestorThreadId` filtering.
- `thread/loaded/list` returns the thread IDs currently in memory.
- `thread/archive` moves the thread's persisted JSONL log into the archived directory and attempts to archive spawned descendant thread logs that aren't already archived.
- `thread/delete` permanently deletes a persisted active or archived thread and its spawned descendant threads.
- `thread/metadata/update` patches stored thread metadata, currently including persisted `gitInfo`.
- `thread/unsubscribe` unsubscribes the current connection from a loaded thread and can trigger `thread/closed` after an inactivity grace period.
- `thread/unarchive` restores an archived thread rollout back into the active sessions directory.
- `thread/compact/start` triggers compaction and returns `{}` immediately.
- `thread/rollback` is deprecated. It drops the last N turns from the in-memory context and records a rollback marker in the thread's persisted JSONL log.
- `thread/inject_items` appends raw Responses API items to a loaded thread's model-visible history without starting a user turn.

#### Start or resume a thread

Start a fresh thread when you need a new Codex conversation.

```json
{ "method": "thread/start", "id": 10, "params": {
  "model": "gpt-5.4",
  "cwd": "/Users/me/project",
  "approvalPolicy": "never",
  "sandbox": "workspaceWrite",
  "personality": "friendly",
  "serviceName": "my_app_server_client"
} }
```

```json
{ "id": 10, "result": {
  "thread": {
    "id": "thr_123",
    "sessionId": "thr_123",
    "preview": "",
    "ephemeral": false,
    "modelProvider": "openai",
    "createdAt": 1730910000
  }
} }
```

```json
{ "method": "thread/started", "params": { "thread": { "id": "thr_123" } } }
```

`serviceName` is optional. Set it when you want app-server to tag thread-level metrics with your integration's service name.

`thread/start`, `thread/resume`, and `thread/fork` return `instructionSources`, an array of loaded instruction-file paths. Each path uses its source environment's native absolute syntax, including for remote environments.

Experimental clients can set `historyMode` on `thread/start` to `"legacy"` (the default) or `"paginated"`. Paginated thread creation isn't supported yet and returns JSON-RPC error `-32601`. App-server can list and read summaries for existing paginated records, but full-history reads, turn pagination, and resume fail closed until paginated history is supported.

Beta clients that opt into `capabilities.experimentalApi` can pass a named permission-profile id in `permissions` instead of the legacy `sandbox` field. Don't send `permissions` and `sandbox` together. Use `permissionProfile/list` with the project `cwd` to discover available profiles and whether managed requirements allow each one.

`thread.sessionId` identifies the current live session tree root. Root threads use their own thread id as the session id; forked threads keep the session id of the root they came from. Clients should read the session id from `thread.sessionId` instead of deriving it from the thread id.

To continue a stored session, call `thread/resume` with the `thread.id` you recorded earlier. The response shape matches `thread/start`. You can also pass the same configuration overrides supported by `thread/start`, such as `personality`:

```json
{ "method": "thread/resume", "id": 11, "params": {
  "threadId": "thr_123",
  "personality": "friendly"
} }
```

```json
{ "id": 11, "result": { "thread": { "id": "thr_123", "name": "Bug bash notes", "ephemeral": false } } }
```

Resuming a thread doesn't update `thread.updatedAt` (or the rollout file's modified time) by itself. The timestamp updates when you start a turn.

If you mark an enabled MCP server as `required` in config and that server fails to initialize, `thread/start` and `thread/resume` fail instead of continuing without it.

`dynamicTools` on `thread/start` is an experimental field (requires `capabilities.experimentalApi = true`). Codex persists these dynamic tools in the thread rollout metadata and restores them on `thread/resume` when you don't supply new dynamic tools.

If you resume with a different model than the one recorded in the rollout, Codex emits a warning and applies a one-time model-switch instruction on the next turn.

#### Manage a thread goal

Use `thread/goal/set`, `thread/goal/get`, and `thread/goal/clear` to manage the same persisted goal state surfaced by `/goal` in the TUI.

```json
{ "method": "thread/goal/set", "id": 13, "params": {
  "threadId": "thr_123",
  "objective": "Finish the migration and keep tests green",
  "status": "active",
  "tokenBudget": 40000
} }
```

```json
{ "id": 13, "result": { "goal": {
  "threadId": "thr_123",
  "objective": "Finish the migration and keep tests green",
  "status": "active",
  "tokenBudget": 40000,
  "tokensUsed": 0,
  "timeUsedSeconds": 0
} } }
```

```json
{ "method": "thread/goal/updated", "params": {
  "threadId": "thr_123",
  "goal": {
    "threadId": "thr_123",
    "objective": "Finish the migration and keep tests green",
    "status": "active",
    "tokenBudget": 40000,
    "tokensUsed": 0,
    "timeUsedSeconds": 0
  }
} }
```

Goal objectives must be non-empty and at most 4,000 characters. Supplying a new objective replaces the goal and resets usage accounting. Supplying the current non-terminal objective, or omitting `objective`, updates status or token budget while preserving usage history.

To branch from a stored session, call `thread/fork` with the `thread.id`. This creates a new thread id and emits a `thread/started` notification for it. Pass `lastTurnId` to copy history through that turn, inclusive, and omit later turns:

```json
{ "method": "thread/fork", "id": 12, "params": { "threadId": "thr_123", "lastTurnId": "turn_456" } }
```

```json
{ "id": 12, "result": { "thread": { "id": "thr_456", "sessionId": "thr_123", "forkedFromId": "thr_123" } } }
```

```json
{ "method": "thread/started", "params": { "thread": { "id": "thr_456" } } }
```

App-server rejects an in-progress `lastTurnId`. If you omit the field while the source thread is mid-turn, the fork records an interruption marker instead of retaining an unmarked partial turn.

When a user-facing thread title has been set, app-server hydrates `thread.name` on `thread/list`, `thread/read`, `thread/resume`, `thread/unarchive`, and `thread/rollback` responses. `thread/start` and `thread/fork` may omit `name` (or return `null`) until a title is set later.

#### Read a stored thread (without resuming)

Use `thread/read` when you want stored thread data but don't want to resume the thread or subscribe to its events.

- `includeTurns`: when `true`, the response includes the thread's turns; when `false` or omitted, you get the thread summary only.
- Returned `thread` objects include runtime `status` (`notLoaded`, `idle`, `systemError`, or `active` with `activeFlags`).

```json
{ "method": "thread/read", "id": 19, "params": { "threadId": "thr_123", "includeTurns": true } }
```

```json
{ "id": 19, "result": { "thread": { "id": "thr_123", "name": "Bug bash notes", "ephemeral": false, "status": { "type": "notLoaded" }, "turns": [] } } }
```

Unlike `thread/resume`, `thread/read` doesn't load the thread into memory or emit `thread/started`.

#### List thread turns

`thread/turns/list` is experimental. Use it to page a stored thread's turn history without resuming it. Results default to newest-first so clients can fetch older turns with `nextCursor`. The response also includes `backwardsCursor`; pass it as `cursor` with `sortDirection: "asc"` to fetch turns newer than the first item from the earlier page.

`itemsView` controls how much turn-item data the response includes:

- `notLoaded` omits items.
- `summary` returns summarized item data and is the default when omitted.
- `full` returns full item data.

```json
{ "method": "thread/turns/list", "id": 20, "params": {
  "threadId": "thr_123",
  "limit": 50,
  "sortDirection": "desc",
  "itemsView": "summary"
} }
```

```json
{ "id": 20, "result": {
  "data": [],
  "nextCursor": "older-turns-cursor-or-null",
  "backwardsCursor": "newer-turns-cursor-or-null"
} }
```

`thread/items/list` is also experimental. It pages persisted items without resuming the thread. Pass `turnId` to restrict results to one turn, or omit it to page items across the thread. The active thread store must support item pagination; otherwise, the server returns an unsupported-method error.

#### List threads (with pagination and filters)

`thread/list` lets you render a history UI. Results default to newest-first by `createdAt`. Filters apply before pagination. Pass any combination of:

- `cursor`: opaque string from a prior response; omit for the first page.
- `limit`: server defaults to a reasonable page size if unset.
- `sortKey`: `created_at` (default), `updated_at`, or `recency_at`.
- `sortDirection`: `desc` (default) or `asc`.
- `modelProviders`: restrict results to specific providers; unset, null, or an empty array includes all providers.
- `sourceKinds`: restrict results to specific thread sources. When omitted or `[]`, the server defaults to interactive sources only: `cli` and `vscode`.
- `archived`: when `true`, list archived threads only. When `false` or omitted, list non-archived threads (default).
- `cwd`: restrict results to threads whose session current working directory exactly matches this path, or one of the paths in an array. Relative paths resolve from the app-server process working directory.
- `useStateDbOnly`: when `true`, return state database results without scanning JSONL thread logs to repair metadata. Omit it or pass `false` for the default scan-and-repair behavior.
- `searchTerm`: restrict results to threads whose extracted title contains this case-sensitive text fragment.
- `parentThreadId`: restrict results to direct child threads of the given parent thread. This filter is experimental and requires `capabilities.experimentalApi = true`.
- `ancestorThreadId`: restrict results to spawned descendants of the given thread at any depth. This filter is experimental and requires `capabilities.experimentalApi = true`; don't combine it with `parentThreadId`.

`sourceKinds` accepts the following values:

- `cli`
- `vscode`
- `exec`
- `appServer`
- `subAgent`
- `subAgentReview`
- `subAgentCompact`
- `subAgentThreadSpawn`
- `subAgentOther`
- `unknown`

Example:

```json
{ "method": "thread/list", "id": 20, "params": {
  "cursor": null,
  "limit": 25,
  "sortKey": "created_at"
} }
```

```json
{ "id": 20, "result": {
  "data": [
    { "id": "thr_a", "preview": "Create a TUI", "ephemeral": false, "modelProvider": "openai", "createdAt": 1730831111, "updatedAt": 1730831111, "name": "TUI prototype", "status": { "type": "notLoaded" } },
    { "id": "thr_b", "preview": "Fix tests", "ephemeral": true, "modelProvider": "openai", "createdAt": 1730750000, "updatedAt": 1730750000, "status": { "type": "notLoaded" } }
  ],
  "nextCursor": "opaque-token-or-null"
} }
```

When `nextCursor` is `null`, you have reached the final page.

#### Update stored thread metadata

Use `thread/metadata/update` to patch stored thread metadata without resuming the thread. Today this supports persisted `gitInfo`; omitted fields are left unchanged, and explicit `null` clears a stored value.

```json
{ "method": "thread/metadata/update", "id": 21, "params": {
  "threadId": "thr_123",
  "gitInfo": { "branch": "feature/sidebar-pr" }
} }
```

```json
{ "id": 21, "result": {
  "thread": {
    "id": "thr_123",
    "gitInfo": { "sha": null, "branch": "feature/sidebar-pr", "originUrl": null }
  }
} }
```

#### Track thread status changes

`thread/status/changed` is emitted whenever a loaded thread's runtime status changes. The payload includes `threadId` and the new `status`.

```json
{
  "method": "thread/status/changed",
  "params": {
    "threadId": "thr_123",
    "status": { "type": "active", "activeFlags": ["waitingOnApproval"] }
  }
}
```

#### List loaded threads

`thread/loaded/list` returns thread IDs currently loaded in memory.

```json
{ "method": "thread/loaded/list", "id": 21 }
```

```json
{ "id": 21, "result": { "data": ["thr_123", "thr_456"] } }
```

#### Unsubscribe from a loaded thread

`thread/unsubscribe` removes the current connection's subscription to a thread. The response status is one of:

- `unsubscribed` when the connection was subscribed and is now removed.
- `notSubscribed` when the connection wasn't subscribed to that thread.
- `notLoaded` when the thread isn't loaded.

If this was the last subscriber, the server keeps the thread loaded until it has no subscribers and no thread activity for 30 minutes. When the grace period expires, app-server unloads the thread and emits a `thread/status/changed` transition to `notLoaded` plus `thread/closed`.

```json
{ "method": "thread/unsubscribe", "id": 22, "params": { "threadId": "thr_123" } }
```

```json
{ "id": 22, "result": { "status": "unsubscribed" } }
```

If the thread later expires:

```json
{ "method": "thread/status/changed", "params": {
    "threadId": "thr_123",
    "status": { "type": "notLoaded" }
} }
```

```json
{ "method": "thread/closed", "params": { "threadId": "thr_123" } }
```

#### Archive a thread

Use `thread/archive` to move the persisted thread log (stored as a JSONL file on disk) into the archived sessions directory. Archiving a thread also attempts to archive spawned descendant threads that aren't already archived.

```json
{ "method": "thread/archive", "id": 22, "params": { "threadId": "thr_b" } }
```

```json
{ "id": 22, "result": {} }
```

```json
{ "method": "thread/archived", "params": { "threadId": "thr_b" } }
```

```json
{ "method": "thread/archived", "params": { "threadId": "thr_child" } }
```

Archived threads won't appear in future calls to `thread/list` unless you pass `archived: true`. The server emits one `thread/archived` notification for each thread it actually archives; if a spawned descendant can't be archived, the request can still succeed without an archived notification for that descendant.

#### Delete a thread

Use `thread/delete` to permanently delete a persisted active or archived thread and its spawned descendant threads. The server removes existing rollout files and associated metadata before returning success; missing rollout files are treated as already deleted. Ephemeral root threads can't be deleted.

```json
{ "method": "thread/delete", "id": 23, "params": { "threadId": "thr_b" } }
```

```json
{ "id": 23, "result": {} }
```

```json
{ "method": "thread/deleted", "params": { "threadId": "thr_b" } }
```

```json
{ "method": "thread/deleted", "params": { "threadId": "thr_child" } }
```

#### Unarchive a thread

Use `thread/unarchive` to move an archived thread rollout back into the active sessions directory.

```json
{ "method": "thread/unarchive", "id": 24, "params": { "threadId": "thr_b" } }
```

```json
{ "id": 24, "result": { "thread": { "id": "thr_b", "name": "Bug bash notes" } } }
```

```json
{ "method": "thread/unarchived", "params": { "threadId": "thr_b" } }
```

#### Trigger thread compaction

Use `thread/compact/start` to trigger manual history compaction for a thread. The request returns immediately with `{}`.

App-server emits progress as standard `turn/*` and `item/*` notifications on the same `threadId`, including a `contextCompaction` item lifecycle (`item/started` then `item/completed`).

```json
{ "method": "thread/compact/start", "id": 25, "params": { "threadId": "thr_b" } }
```

```json
{ "id": 25, "result": {} }
```

#### Run a thread shell command

Use `thread/shellCommand` for user-initiated shell commands that belong to a thread. The request returns immediately with `{}` while progress streams through standard `turn/*` and `item/*` notifications.

This API runs outside the sandbox with full access and doesn't inherit the thread sandbox policy. Clients should expose it only for explicit user-initiated commands.

If the thread already has an active turn, the command runs as an auxiliary action on that turn and its formatted output is injected into the turn's message stream. If the thread is idle, app-server starts a standalone turn for the shell command.

```json
{ "method": "thread/shellCommand", "id": 26, "params": { "threadId": "thr_b", "command": "git status --short" } }
```

```json
{ "id": 26, "result": {} }
```

#### Clean background terminals

Use `thread/backgroundTerminals/clean` to stop all running background terminals associated with a thread. This method is experimental and requires `capabilities.experimentalApi = true`.

```json
{ "method": "thread/backgroundTerminals/clean", "id": 27, "params": { "threadId": "thr_b" } }
```

```json
{ "id": 27, "result": {} }
```

Use `thread/backgroundTerminals/list` to inspect running background terminals for a loaded thread. The request supports standard `cursor` and `limit` pagination, and the returned `processId` is the app-server process id. This method is experimental and requires `capabilities.experimentalApi = true`:

```json
{ "method": "thread/backgroundTerminals/list", "id": 28, "params": { "threadId": "thr_b" } }
```

```json
{ "id": 28, "result": { "data": [
  {
    "itemId": "item_456",
    "processId": "42",
    "command": "python3 -m http.server",
    "cwd": "/workspace",
    "osPid": null,
    "cpuPercent": null,
    "rssKb": null
  }
], "nextCursor": null } }
```

Use `thread/backgroundTerminals/terminate` with that `processId` to stop one background terminal. This method is experimental and requires `capabilities.experimentalApi = true`:

```json
{ "method": "thread/backgroundTerminals/terminate", "id": 29, "params": { "threadId": "thr_b", "processId": "42" } }
```

```json
{ "id": 29, "result": { "terminated": true } }
```

#### Roll back recent turns

`thread/rollback` is deprecated and will be removed. It removes the last `numTurns` entries from the in-memory context and persists a rollback marker in the rollout log. The returned `thread` includes `turns` populated after the rollback.

```json
{ "method": "thread/rollback", "id": 30, "params": { "threadId": "thr_b", "numTurns": 1 } }
```

```json
{ "id": 30, "result": { "thread": { "id": "thr_b", "name": "Bug bash notes", "ephemeral": false } } }
```

### Turns

The `input` field accepts a list of items:

- `{ "type": "text", "text": "Explain this diff" }`
- `{ "type": "image", "url": "https://.../design.png" }`
- `{ "type": "localImage", "path": "/tmp/screenshot.png" }`

You can override configuration settings per turn (model, effort, personality, `cwd`, sandbox policy, summary). When specified, these settings become the defaults for later turns on the same thread. `outputSchema` applies only to the current turn. For `sandboxPolicy.type = "externalSandbox"`, set `networkAccess` to `restricted` or `enabled`; for `workspaceWrite`, `networkAccess` remains a boolean.

For `turn/start.collaborationMode`, `settings.developer_instructions: null` means "use built-in instructions for the selected mode" rather than clearing mode instructions.

#### Sandbox read access (`ReadOnlyAccess`)

`sandboxPolicy` supports explicit read-access controls:

- `readOnly`: optional `access` (`{ "type": "fullAccess" }` by default, or restricted roots).
- `workspaceWrite`: optional `readOnlyAccess` (`{ "type": "fullAccess" }` by default, or restricted roots).

Restricted read access shape:

```json
{
  "type": "restricted",
  "includePlatformDefaults": true,
  "readableRoots": ["/Users/me/shared-read-only"]
}
```

On macOS, `includePlatformDefaults: true` appends a curated platform-default Seatbelt policy for restricted-read sessions. This improves tool compatibility without broadly allowing all of `/System`.

Examples:

```json
{ "type": "readOnly", "access": { "type": "fullAccess" } }
```

```json
{
  "type": "workspaceWrite",
  "writableRoots": ["/Users/me/project"],
  "readOnlyAccess": {
    "type": "restricted",
    "includePlatformDefaults": true,
    "readableRoots": ["/Users/me/shared-read-only"]
  },
  "networkAccess": false
}
```

#### Start a turn

```json
{ "method": "turn/start", "id": 30, "params": {
  "threadId": "thr_123",
  "input": [ { "type": "text", "text": "Run tests" } ],
  "cwd": "/Users/me/project",
  "approvalPolicy": "unlessTrusted",
  "sandboxPolicy": {
    "type": "workspaceWrite",
    "writableRoots": ["/Users/me/project"],
    "networkAccess": true
  },
  "model": "gpt-5.4",
  "effort": "medium",
  "summary": "concise",
  "personality": "friendly",
  "outputSchema": {
    "type": "object",
    "properties": { "answer": { "type": "string" } },
    "required": ["answer"],
    "additionalProperties": false
  }
} }
```

#### Inject items into a thread

Use `thread/inject_items` to append prebuilt Responses API items to a loaded thread's prompt history without starting a user turn. These items are persisted to the rollout and included in subsequent model requests.

```json
{ "method": "thread/inject_items", "id": 31, "params": {
  "threadId": "thr_123",
  "items": [
    {
      "type": "message",
      "role": "assistant",
      "content": [{ "type": "output_text", "text": "Previously computed context." }]
    }
  ]
} }
```

```json
{ "id": 31, "result": {} }
```

#### Steer an active turn

Use `turn/steer` to append more user input to the active in-flight turn.

- Include `expectedTurnId`; it must match the active turn id.
- The request fails if there is no active turn on the thread.
- `turn/steer` doesn't emit a new `turn/started` notification.
- `turn/steer` doesn't accept turn-level overrides (`model`, `cwd`, `sandboxPolicy`, or `outputSchema`).

```json
{ "method": "turn/steer", "id": 32, "params": {
  "threadId": "thr_123",
  "input": [ { "type": "text", "text": "Actually focus on failing tests first." } ],
  "expectedTurnId": "turn_456"
} }
```

```json
{ "id": 32, "result": { "turnId": "turn_456" } }
```

#### Start a turn (invoke a skill)

Invoke a skill explicitly by including `$<skill-name>` in the text input and adding a `skill` input item alongside it.

```json
{ "method": "turn/start", "id": 33, "params": {
  "threadId": "thr_123",
  "input": [
    { "type": "text", "text": "$skill-creator Add a new skill for triaging flaky CI and include step-by-step usage." },
    { "type": "skill", "name": "skill-creator", "path": "/Users/me/.codex/skills/skill-creator/SKILL.md" }
  ]
} }
```

```json
{ "id": 33, "result": { "turn": { "id": "turn_457", "status": "inProgress", "items": [], "error": null } } }
```

#### Interrupt a turn

```json
{ "method": "turn/interrupt", "id": 31, "params": { "threadId": "thr_123", "turnId": "turn_456" } }
```

```json
{ "id": 31, "result": {} }
```

On success, the turn finishes with `status: "interrupted"`.

### Review

`review/start` runs the Codex reviewer for a thread and streams review items. Targets include:

- `uncommittedChanges`
- `baseBranch` (diff against a branch)
- `commit` (review a specific commit)
- `custom` (free-form instructions)

Use `delivery: "inline"` (default) to run the review on the existing thread, or `delivery: "detached"` to fork a new review thread.

Example request/response:

```json
{ "method": "review/start", "id": 40, "params": {
  "threadId": "thr_123",
  "delivery": "inline",
  "target": { "type": "commit", "sha": "1234567deadbeef", "title": "Polish tui colors" }
} }
```

```json
{ "id": 40, "result": {
  "turn": {
    "id": "turn_900",
    "status": "inProgress",
    "items": [
      { "type": "userMessage", "id": "turn_900", "content": [ { "type": "text", "text": "Review commit 1234567: Polish tui colors" } ] }
    ],
    "error": null
  },
  "reviewThreadId": "thr_123"
} }
```

For a detached review, use `"delivery": "detached"`. The response is the same shape, but `reviewThreadId` will be the id of the new review thread (different from the original `threadId`). The server also emits a `thread/started` notification for that new thread before streaming the review turn.

Codex streams the usual `turn/started` notification followed by an `item/started` with an `enteredReviewMode` item:

```json
{
  "method": "item/started",
  "params": {
    "item": {
      "type": "enteredReviewMode",
      "id": "turn_900",
      "review": "current changes"
    }
  }
}
```

When the reviewer finishes, the server emits `item/started` and `item/completed` containing an `exitedReviewMode` item with the final review text:

```json
{
  "method": "item/completed",
  "params": {
    "item": {
      "type": "exitedReviewMode",
      "id": "turn_900",
      "review": "Looks solid overall..."
    }
  }
}
```

Use this notification to render the reviewer output in your client.

### Process execution

`process/*` is an experimental, explicit process-control API. It requires `capabilities.experimentalApi = true` and runs outside Codex's sandbox. Use it only when your client intentionally exposes local process control without a sandbox.

Start a process with `process/spawn` and provide a `processHandle`, then use that handle for stdin, resize, and kill requests. Output streams through `process/outputDelta` notifications and completion streams through `process/exited`.

```json
{ "method": "process/spawn", "id": 48, "params": {
  "command": ["python3", "-m", "pytest", "-q"],
  "processHandle": "pytest-1",
  "cwd": "/Users/me/project",
  "tty": true
} }
```

```json
{ "id": 48, "result": {} }
```

```json
{ "method": "process/outputDelta", "params": {
  "processHandle": "pytest-1",
  "stream": "stdout",
  "deltaBase64": "Li4u"
} }
```

```json
{ "method": "process/exited", "params": {
  "processHandle": "pytest-1",
  "exitCode": 0
} }
```

Use `process/writeStdin` with `deltaBase64`, `closeStdin`, or both to send input. Use `process/resizePty` for PTY resize events and `process/kill` to terminate a running process.

### Command execution

`command/exec` runs a single command (`argv` array) under the server sandbox without creating a thread.

```json
{ "method": "command/exec", "id": 50, "params": {
  "command": ["ls", "-la"],
  "cwd": "/Users/me/project",
  "sandboxPolicy": { "type": "workspaceWrite" },
  "timeoutMs": 10000
} }
```

```json
{ "id": 50, "result": { "exitCode": 0, "stdout": "...", "stderr": "" } }
```

Use `sandboxPolicy.type = "externalSandbox"` if you already sandbox the server process and want Codex to skip its own sandbox enforcement. For external sandbox mode, set `networkAccess` to `restricted` (default) or `enabled`. For `readOnly` and `workspaceWrite`, use the same optional `access` / `readOnlyAccess` structure shown above.

Notes:

- The server rejects empty `command` arrays.
- `sandboxPolicy` accepts the same shape used by `turn/start` (for example, `dangerFullAccess`, `readOnly`, `workspaceWrite`, `externalSandbox`).
- When omitted, `timeoutMs` falls back to the server default.
- Set `tty: true` for PTY-backed sessions, and use `processId` when you plan to follow up with `command/exec/write`, `command/exec/resize`, or `command/exec/terminate`.
- Set `streamStdoutStderr: true` to receive `command/exec/outputDelta` notifications while the command is running.

#### Read admin requirements (`configRequirements/read`)

Use `configRequirements/read` to inspect the effective admin requirements loaded from `requirements.toml` and/or MDM.

```json
{ "method": "configRequirements/read", "id": 52, "params": {} }
```

```json
{ "id": 52, "result": {
  "requirements": {
    "allowedApprovalPolicies": ["onRequest", "unlessTrusted"],
    "allowedSandboxModes": ["readOnly", "workspaceWrite"],
    "featureRequirements": {
      "personality": true,
      "unified_exec": false
    },
    "network": {
      "enabled": true,
      "allowedDomains": ["api.openai.com"],
      "allowUnixSockets": ["/tmp/example.sock"],
      "dangerouslyAllowAllUnixSockets": false
    }
  }
} }
```

`result.requirements` is `null` when no requirements are configured. See the docs on `requirements.toml` (/codex/config-file/config-reference#requirementstoml) for details on supported keys and values.

#### Windows sandbox setup (`windowsSandbox/setupStart`)

Custom Windows clients can trigger sandbox setup asynchronously instead of blocking on startup checks.

```json
{ "method": "windowsSandbox/setupStart", "id": 53, "params": { "mode": "elevated" } }
```

```json
{ "id": 53, "result": { "started": true } }
```

App-server starts setup in the background and later emits a completion notification:

```json
{
  "method": "windowsSandbox/setupCompleted",
  "params": { "mode": "elevated", "success": true, "error": null }
}
```

Modes:

- `elevated`: run the elevated Windows sandbox setup path.
- `unelevated`: run the legacy setup/preflight path.

### Filesystem

The v2 filesystem APIs operate on absolute paths. Use `fs/watch` when a client needs to invalidate UI state after a file or directory changes.

```json
{ "method": "fs/watch", "id": 54, "params": {
  "watchId": "0195ec6b-1d6f-7c2e-8c7a-56f2c4a8b9d1",
  "path": "/Users/me/project/.git/HEAD"
} }
```

```json
{ "id": 54, "result": { "path": "/Users/me/project/.git/HEAD" } }
```

```json
{ "method": "fs/changed", "params": {
  "watchId": "0195ec6b-1d6f-7c2e-8c7a-56f2c4a8b9d1",
  "changedPaths": ["/Users/me/project/.git/HEAD"]
} }
```

```json
{ "method": "fs/unwatch", "id": 55, "params": {
  "watchId": "0195ec6b-1d6f-7c2e-8c7a-56f2c4a8b9d1"
} }
```

```json
{ "id": 55, "result": {} }
```

Watching a file emits `fs/changed` for that file path, including updates delivered by replace or rename operations.

### Events

Event notifications are the server-initiated stream for thread lifecycles, turn lifecycles, and the items within them. After you start or resume a thread, keep reading the active transport stream for `thread/started`, `thread/archived`, `thread/unarchived`, `thread/closed`, `thread/status/changed`, `turn/*`, `item/*`, and `serverRequest/resolved` notifications.

#### Notification opt-out

Clients can suppress specific notifications per connection by sending exact method names in `initialize.params.capabilities.optOutNotificationMethods`.

- Exact-match only: `item/agentMessage/delta` suppresses only that method.
- Unknown method names are ignored.
- Applies to the current `thread/*`, `turn/*`, `item/*`, and related v2 notifications.
- Doesn't apply to requests, responses, or errors.

#### Fuzzy file search events (experimental)

The fuzzy file search session API emits per-query notifications:

- `fuzzyFileSearch/sessionUpdated`: `{ sessionId, query, files }` with the current matches for the active query.
- `fuzzyFileSearch/sessionCompleted`: `{ sessionId }` once indexing and matching for that query completes.

#### Warning events

- `configWarning`: `{ summary, details?, path?, range? }` for recoverable configuration or initialization problems.
- `warning`: `{ threadId?, message }` for non-fatal runtime warnings.

#### Windows sandbox setup events

- `windowsSandbox/setupCompleted`: `{ mode, success, error }` emitted after a `windowsSandbox/setupStart` request finishes.

#### Turn events

- `turn/started`: `{ turn }` with the turn id, empty `items`, and `status: "inProgress"`.
- `turn/completed`: `{ turn }` where `turn.status` is `completed`, `interrupted`, or `failed`; failures carry `{ error: { message, codexErrorInfo?, additionalDetails? } }`.
- `turn/diff/updated`: `{ threadId, turnId, diff }` with the latest aggregated unified diff across every file change in the turn.
- `turn/plan/updated`: `{ turnId, explanation?, plan }` whenever the agent shares or changes its plan; each `plan` entry is `{ step, status }` with `status` in `pending`, `inProgress`, or `completed`.
- `hook/started` and `hook/completed`: `{ threadId, turnId?, run }` when a lifecycle hook starts and when its final run summary is available.
- `model/safetyBuffering/updated`: `{ threadId, turnId, model, useCases, reasons, showBufferingUi, fasterModel }` when a response enters transient safety buffering.
- `model/rerouted`: `{ threadId, turnId, fromModel, toModel, reason }` when the service routes a request to another model.
- `model/verification`: `{ threadId, turnId, verifications }` when the service requires additional account verification.
- `thread/tokenUsage/updated`: usage updates for the active thread.

`turn/diff/updated` and `turn/plan/updated` currently include empty `items` arrays even when item events stream. Use `item/*` notifications as the source of truth for turn items.

#### Items

`ThreadItem` is the tagged union carried in turn responses and `item/*` notifications. Common item types include:

- `userMessage`: `{id, content}` where `content` is a list of user inputs (`text`, `image`, or `localImage`).
- `agentMessage`: `{id, text, phase?}` containing the accumulated agent reply. When present, `phase` uses Responses API wire values (`commentary`, `final_answer`).
- `plan`: `{id, text}` containing proposed plan text in plan mode. Treat the final `plan` item from `item/completed` as authoritative.
- `reasoning`: `{id, summary, content}` where `summary` holds streamed reasoning summaries and `content` holds raw reasoning blocks.
- `commandExecution`: `{id, command, cwd, status, commandActions, aggregatedOutput?, exitCode?, durationMs?}`.
- `fileChange`: `{id, changes, status}` describing proposed edits; `changes` list `{path, kind, diff}`.
- `mcpToolCall`: `{id, server, tool, status, arguments, appContext?, pluginId?, result?, error?}`. For trusted MCP apps, `appContext` can include `connectorId`, `linkId`, `resourceUri`, `appName`, `templateId`, and the stable connector `actionName`. Older persisted items can omit newer metadata. Use `appContext.resourceUri` instead of the deprecated top-level `mcpAppResourceUri`.
- `dynamicToolCall`: `{id, tool, arguments, status, contentItems?, success?, durationMs?}` for client-executed dynamic tool invocations.
- `collabToolCall`: `{id, tool, status, senderThreadId, receiverThreadId?, newThreadId?, prompt?, agentStatus?}`.
- `webSearch`: `{id, query, action?}` for web search requests issued by the agent.
- `imageView`: `{id, path}` emitted when the agent invokes the image viewer tool.
- `enteredReviewMode`: `{id, review}` sent when the reviewer starts.
- `exitedReviewMode`: `{id, review}` emitted when the reviewer finishes.
- `contextCompaction`: `{id}` emitted when Codex compacts the conversation history.

For `webSearch.action`, the action `type` can be `search` (`query?`, `queries?`), `openPage` (`url?`), or `findInPage` (`url?`, `pattern?`).

The app server deprecates the legacy `thread/compacted` notification; use the `contextCompaction` item instead.

All items emit two shared lifecycle events:

- `item/started`: emits the full `item` when a new unit of work begins; the `item.id` matches the `itemId` used by deltas.
- `item/completed`: sends the final `item` once work finishes; treat this as the authoritative state.

#### Item deltas

- `item/agentMessage/delta`: appends streamed text for the agent message.
- `item/plan/delta`: streams proposed plan text. The final `plan` item may not exactly equal the concatenated deltas.
- `item/reasoning/summaryTextDelta`: streams readable reasoning summaries; `summaryIndex` increments when a new summary section opens.
- `item/reasoning/summaryPartAdded`: marks a boundary between reasoning summary sections.
- `item/reasoning/textDelta`: streams raw reasoning text (when supported by the model).
- `item/commandExecution/outputDelta`: streams stdout/stderr for a command; append deltas in order.
- `item/fileChange/outputDelta`: deprecated compatibility notification for legacy `apply_patch` text output. Current app-server versions no longer emit it; use `fileChange` items and `turn/diff/updated` instead.

### Errors

If a turn fails, the server emits an `error` event with `{ error: { message, codexErrorInfo?, additionalDetails? } }` and then finishes the turn with `status: "failed"`. When an upstream HTTP status is available, it appears in `codexErrorInfo.httpStatusCode`.

Common `codexErrorInfo` values include:

- `ContextWindowExceeded`
- `UsageLimitExceeded`
- `HttpConnectionFailed` (4xx/5xx upstream errors)
- `ResponseStreamConnectionFailed`
- `ResponseStreamDisconnected`
- `ResponseTooManyFailedAttempts`
- `BadRequest`, `Unauthorized`, `SandboxError`, `InternalServerError`, `Other`

When an upstream HTTP status is available, the server forwards it in `httpStatusCode` on the relevant `codexErrorInfo` variant.

### Approvals

Depending on a user's Codex settings, command execution and file changes may require approval. The app-server sends a server-initiated JSON-RPC request to the client, and the client responds with a decision payload.

- Command execution decisions: `accept`, `acceptForSession`, `decline`, `cancel`, or `{ "acceptWithExecpolicyAmendment": { "execpolicy_amendment": ["cmd", "..."] } }`.
- File change decisions: `accept`, `acceptForSession`, `decline`, `cancel`.
- Requests include `threadId` and `turnId`. Use them to scope UI state to the active conversation.
- The server resumes or declines the work and ends the item with `item/completed`.

#### Command execution approvals

Order of messages:

1. `item/started` shows the pending `commandExecution` item with `command`, `cwd`, and other fields.
2. `item/commandExecution/requestApproval` includes `itemId`, `threadId`, `turnId`, optional `reason`, optional `command`, optional `cwd`, optional `commandActions`, optional `proposedExecpolicyAmendment`, optional `networkApprovalContext`, and optional `availableDecisions`. When `initialize.params.capabilities.experimentalApi = true`, the payload can also include experimental `additionalPermissions` describing requested per-command sandbox access. Any filesystem paths inside `additionalPermissions` are absolute on the wire.
3. Client responds with one of the command execution approval decisions above.
4. `serverRequest/resolved` confirms that the pending request has been answered or cleared.
5. `item/completed` returns the final `commandExecution` item with `status: completed | failed | declined`.

When `networkApprovalContext` is present, the prompt is for managed network access (not a general shell-command approval). The current v2 schema exposes the target `host` and `protocol`; clients should render a network-specific prompt and not rely on `command` being a user-meaningful shell command preview.

Codex groups concurrent network approval prompts by destination (`host`, protocol, and port). The app-server may therefore send one prompt that unblocks multiple queued requests to the same destination, while different ports on the same host are treated separately.

#### File change approvals

Order of messages:

1. `item/started` emits a `fileChange` item with proposed `changes` and `status: "inProgress"`.
2. `item/fileChange/requestApproval` includes `itemId`, `threadId`, `turnId`, optional `reason`, and optional `grantRoot`.
3. Client responds with one of the file change approval decisions above.
4. `serverRequest/resolved` confirms that the pending request has been answered or cleared.
5. `item/completed` returns the final `fileChange` item with `status: completed | failed | declined`.

#### `tool/requestUserInput`

When the client responds to `item/tool/requestUserInput`, app-server emits `serverRequest/resolved` with `{ threadId, requestId }`. If the pending request is cleared by turn start, turn completion, or turn interruption before the client answers, the server emits the same notification for that cleanup.

Request params include `autoResolutionMs` as an integer millisecond timeout or `null`. When present, host clients can resolve the prompt automatically after that interval if the user doesn't answer.

#### Permission requests

The built-in `request_permissions` tool sends `item/permissions/requestApproval` with the `threadId`, `turnId`, `itemId`, `environmentId`, `cwd`, optional `reason`, and requested network or filesystem permissions. Respond with `permissions` containing only the granted subset. Set `scope` to `"session"` to persist the grant for later turns in the same session; omit it or use `"turn"` for a turn-scoped grant. Permissions that weren't requested are ignored.

#### MCP server elicitation requests

An MCP server can interrupt a turn with `mcpServer/elicitation/request`. The request includes `threadId`, an optional `turnId`, `serverName`, and one of these request shapes:

- `mode: "form"` or `mode: "openai/form"`, with `message` and `requestedSchema`.
- `mode: "url"`, with `message`, `url`, and `elicitationId`.

Respond with `action: "accept"` and the requested `content`, or with `action: "decline"` or `"cancel"` and `content: null`. App-server then emits `serverRequest/resolved`. To receive the `openai/form` variant, opt in with `initialize.params.capabilities.mcpServerOpenaiFormElicitation`.

#### Dynamic tool calls (experimental)

`dynamicTools` on `thread/start` and the corresponding `item/tool/call` request or response flow are experimental APIs.

Dynamic tool names and namespace names must follow Responses API naming constraints. Avoid reserved namespace names used by built-in Codex tools.

When a dynamic tool is invoked during a turn, app-server emits:

1. `item/started` with `item.type = "dynamicToolCall"`, `status = "inProgress"`, plus `tool` and `arguments`.
2. `item/tool/call` as a server request to the client.
3. The client response payload with returned content items.
4. `item/completed` with `item.type = "dynamicToolCall"`, the final `status`, and any returned `contentItems` or `success` value.

#### MCP tool-call approvals (apps)

App (connector) tool calls can also require approval. When an app tool call has side effects, the server may elicit approval with `tool/requestUserInput` and options such as Accept, Decline, and Cancel. Destructive tool annotations always trigger approval even when the tool also advertises less-privileged hints. If the user declines or cancels, the related `mcpToolCall` item completes with an error instead of running the tool.

### Skills

Invoke a skill by including `$<skill-name>` in the user text input. Add a `skill` input item (recommended) so the server injects full skill instructions instead of relying on the model to resolve the name.

```json
{
  "method": "turn/start",
  "id": 101,
  "params": {
    "threadId": "thread-1",
    "input": [
      {
        "type": "text",
        "text": "$skill-creator Add a new skill for triaging flaky CI."
      },
      {
        "type": "skill",
        "name": "skill-creator",
        "path": "/Users/me/.codex/skills/skill-creator/SKILL.md"
      }
    ]
  }
}
```

If you omit the `skill` item, the model will still parse the `$<skill-name>` marker and try to locate the skill, which can add latency.

Example:

```
$skill-creator Add a new skill for triaging flaky CI and include step-by-step usage.
```

Use `skills/list` to fetch available skills (optionally scoped by `cwds`, with `forceReload`). You can also include `perCwdExtraUserRoots` to scan extra absolute paths as `user` scope for specific `cwd` values. App-server ignores entries whose `cwd` isn't present in `cwds`. `skills/list` may reuse a cached result per `cwd`; set `forceReload: true` to refresh from disk. When present, the server reads `interface` and `dependencies` from `SKILL.json`.

```json
{ "method": "skills/list", "id": 25, "params": {
  "cwds": ["/Users/me/project", "/Users/me/other-project"],
  "forceReload": true,
  "perCwdExtraUserRoots": [
    {
      "cwd": "/Users/me/project",
      "extraUserRoots": ["/Users/me/shared-skills"]
    }
  ]
} }
```

```json
{ "id": 25, "result": {
  "data": [{
    "cwd": "/Users/me/project",
    "skills": [
      {
        "name": "skill-creator",
        "description": "Create or update a Codex skill",
        "enabled": true,
        "interface": {
          "displayName": "Skill Creator",
          "shortDescription": "Create or update a Codex skill"
        },
        "dependencies": {
          "tools": [
            {
              "type": "env_var",
              "value": "GITHUB_TOKEN",
              "description": "GitHub API token"
            },
            {
              "type": "mcp",
              "value": "github",
              "transport": "streamable_http",
              "url": "https://example.com/mcp"
            }
          ]
        }
      }
    ],
    "errors": []
  }]
} }
```

The server also emits `skills/changed` notifications when watched local skill files change. Treat this as an invalidation signal and rerun `skills/list` with your current params when needed.

To enable or disable a skill by path:

```json
{
  "method": "skills/config/write",
  "id": 26,
  "params": {
    "path": "/Users/me/.codex/skills/skill-creator/SKILL.md",
    "enabled": false
  }
}
```

### Apps (connectors)

Use `app/list` to fetch available apps. In the CLI/TUI, `/apps` is the user-facing picker; in custom clients, call `app/list` directly. Each entry includes both `isAccessible` (available to the user) and `isEnabled` (enabled in `config.toml`) so clients can distinguish install/access from local enabled state. App entries can also include optional `branding`, `appMetadata`, and `labels` fields.

```json
{ "method": "app/list", "id": 50, "params": {
  "cursor": null,
  "limit": 50,
  "threadId": "thread-1",
  "forceRefetch": false
} }
```

```json
{ "id": 50, "result": {
  "data": [
    {
      "id": "demo-app",
      "name": "Demo App",
      "description": "Example connector for documentation.",
      "logoUrl": "https://example.com/demo-app.png",
      "logoUrlDark": null,
      "distributionChannel": null,
      "branding": null,
      "appMetadata": null,
      "labels": null,
      "installUrl": "https://chatgpt.com/apps/demo-app/demo-app",
      "isAccessible": true,
      "isEnabled": true
    }
  ],
  "nextCursor": null
} }
```

If you provide `threadId`, app feature gating (`features.apps`) uses that thread's config snapshot. When omitted, app-server uses the latest global config.

`app/list` returns after both accessible apps and directory apps load. Set `forceRefetch: true` to bypass app caches and fetch fresh data. Cache entries are only replaced when refreshes succeed.

The server also emits `app/list/updated` notifications whenever either source (accessible apps or directory apps) finishes loading. Each notification includes the latest merged app list.

---

## Use Codex with the Agents SDK

### Running Codex as an MCP Server

You can run Codex as an MCP server and connect it from other MCP clients, such as agents built with the OpenAI Agents SDK MCP integration.

Launch Codex as an MCP server using:

```bash
codex mcp-server
```

You can also use the Model Context Protocol Inspector:

```bash
npx @modelcontextprotocol/inspector codex mcp-server
```

#### Available Tools

The server exposes two tools:

1. `codex`: Runs a Codex session with a prompt and configuration overrides
   - `prompt` (required): Initial user prompt
   - `approval-policy`: Controls shell command approval (`untrusted`, `on-request`, `never`)
   - `sandbox`: Sandbox mode (`read-only`, `workspace-write`, `danger-full-access`)
   - Other optional parameters: `model`, `config`, `cwd`, `developer-instructions`, etc.

2. `codex-reply`: Continues a session using thread ID and new prompt
   - `prompt` (required): Next user prompt
   - `threadId` (required): Thread ID from previous response

### Creating Multi-Agent Workflows

By exposing Codex CLI as an MCP server and orchestrating it with the OpenAI Agents SDK, you can build deterministic, reviewable workflows that scale from a single agent to a complete software delivery pipeline.

#### Prerequisites

- Codex CLI installed
- Python 3.10+
- Node.js 18+ (optional)
- OpenAI API key

#### Implementation Overview

The guide demonstrates building from a simple single-agent workflow to a multi-agent system with a Project Manager coordinating Designer, Frontend Developer, Backend Developer, and Tester agents.

Key workflow features: sequential hand-offs with file gating, parallel task execution, and full execution traces for audit and debugging purposes.

---

## Codex GitHub Action

### Overview

The Codex GitHub Action (`openai/codex-action@v1`) enables you to trigger Codex actions from GitHub Events by running Codex in CI/CD jobs, applying patches, or posting reviews from workflows.

### Use Cases

The action is useful when you want to:

- Automate Codex feedback on pull requests or releases without managing the CLI yourself
- Gate changes on Codex-driven quality checks as part of your CI pipeline
- Run repeatable Codex tasks (code review, release prep, migrations) from a workflow file

### Prerequisites

- Store your OpenAI key as a GitHub secret (for example `OPENAI_API_KEY`)
- Run the job on a Linux or macOS runner; for Windows, set `safety-strategy: unsafe`
- Check out your code before invoking the action
- Decide which prompts to run via `prompt` or `prompt-file`

### Example Workflow

```yaml
name: Codex pull request review
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  codex:
    runs-on: ubuntu-latest
    permissions:
      contents: read
    outputs:
      final_message: ${{ steps.run_codex.outputs.final-message }}
    steps:
      - uses: actions/checkout@v5
        with:
          ref: refs/pull/${{ github.event.pull_request.number }}/merge
          fetch-depth: 0
          persist-credentials: false

      - name: Run Codex
        id: run_codex
        uses: openai/codex-action@v1
        with:
          openai-api-key: ${{ secrets.OPENAI_API_KEY }}
          prompt-file: .github/codex/prompts/review.md
          output-file: codex-output.md

  post_feedback:
    runs-on: ubuntu-latest
    needs: codex
    if: needs.codex.outputs.final_message != ''
    permissions:
      issues: write
      pull-requests: write
    steps:
      - name: Post Codex feedback
        uses: actions/github-script@v7
        with:
          github-token: ${{ github.token }}
          script: |
            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.payload.pull_request.number,
              body: process.env.CODEX_FINAL_MESSAGE,
            });
        env:
          CODEX_FINAL_MESSAGE: ${{ needs.codex.outputs.final_message }}
```

### Configure `codex exec`

Key action inputs that map to `codex exec` options:

- `prompt` or `prompt-file`: Inline instructions or repository path to Markdown/text
- `codex-args`: Extra CLI flags as JSON array or shell string
- `model` and `effort`: Agent configuration settings
- `sandbox`: Sandbox mode (`workspace-write`, `read-only`, `danger-full-access`)
- `output-file`: Save final message to disk
- `codex-version`: Pin specific CLI release
- `codex-home`: Shared Codex home directory for configuration reuse

### Manage Privileges

Control Codex access with these inputs:

- `safety-strategy` (default `drop-sudo`): Removes `sudo` before running Codex
- `unprivileged-user`: Runs Codex as specific account with `safety-strategy: unprivileged-user`
- `read-only`: Prevents file changes and network access
- `sandbox`: Limits filesystem and network access within Codex
- `allow-users` and `allow-bots`: Restrict workflow triggers

### Capture Outputs

The action emits the last Codex message through `final-message` output. Combine `output-file` with artifact uploads for full transcripts, or use `--output-schema` for structured JSON data.

### Security Checklist

- Limit who can start the workflow using trusted events or explicit approvals
- Sanitize prompt inputs from pull requests and commit messages to avoid injection
- Protect `OPENAI_API_KEY` by keeping safety-strategy on `drop-sudo` or using unprivileged user
- Run Codex as the last step so later steps don't inherit unexpected changes
- Rotate keys immediately if secret exposure is suspected

### Troubleshooting

- Both prompt and prompt-file set: Remove the duplicate input
- responses-api-proxy didn't write server info: Confirm API key validity
- Unexpected `sudo` behavior: Verify runner OS is Linux/macOS
- Permission errors after `drop-sudo`: Grant write access before action runs
- Unauthorized trigger blocked: Adjust `allow-users` or `allow-bots` inputs

---

## Non-interactive mode

### Use `codex exec` to run Codex in scripts and CI

Non-interactive mode enables running Codex from scripts without opening the interactive TUI through the `codex exec` command.

### When to use `codex exec`

Use `codex exec` when you need Codex to:

- Run as part of a pipeline (CI, pre-merge checks, scheduled jobs)
- Produce output that pipes into other tools (for generating release notes or summaries)
- Fit naturally into CLI workflows that chain command output into Codex
- Run with explicit, pre-set sandbox and approval settings

### Basic usage

Pass a task prompt as a single argument:

```
codex exec "summarize the repository structure and list the top 5 risky areas"
```

Codex streams progress to `stderr` and prints only the final agent message to `stdout`, enabling straightforward redirection or piping:

```
codex exec "generate release notes for the last 10 commits" | tee release-notes.md
```

Use `--ephemeral` to avoid persisting session rollout files to disk:

```
codex exec --ephemeral "triage this repository and suggest next steps"
```

When stdin is piped with a prompt argument, Codex treats the prompt as instruction and piped content as additional context:

```
curl -s https://jsonplaceholder.typicode.com/comments \
  | codex exec "format the top 20 items into a markdown table" \
  > table.md
```

### Permissions and safety

By default, `codex exec` runs in a read-only sandbox. Set the least permissions needed:

- Allow edits: `codex exec --sandbox workspace-write "<task>"`
- Allow broader access: `codex exec --sandbox danger-full-access "<task>"`

Use `danger-full-access` only in controlled environments like isolated CI runners or containers.

The `--full-auto` flag is deprecated; prefer explicit `--sandbox workspace-write` in new scripts.

Use `--ignore-user-config` when a run shouldn't load `$CODEX_HOME/config.toml`, and `--ignore-rules` to skip user and project execpolicy `.rules` files for controlled automation.

If an MCP server is configured with `required = true` and fails to initialize, `codex exec` exits with an error.

### Make output machine-readable

To consume Codex output in scripts, use JSON Lines output:

```
codex exec --json "summarize the repo structure" | jq
```

When `--json` is enabled, `stdout` becomes a JSON Lines stream capturing every event Codex emits. Event types include `thread.started`, `turn.started`, `turn.completed`, `turn.failed`, `item.*`, and `error`.

Sample JSON stream (each line is a JSON object):

```
{"type":"thread.started","thread_id":"0199a213-81c0-7800-8aa1-bbab2a035a53"}
{"type":"turn.started"}
{"type":"item.started","item":{"id":"item_1","type":"command_execution","command":"bash -lc ls","status":"in_progress"}}
{"type":"item.completed","item":{"id":"item_3","type":"agent_message","text":"Repo contains docs, sdk, and examples directories."}}
{"type":"turn.completed","usage":{"input_tokens":24763,"cached_input_tokens":24448,"output_tokens":122,"reasoning_output_tokens":0}}
```

For only the final message, use `-o <path>`/`--output-last-message <path>` to write to a file while printing to `stdout`.

### Create structured outputs with a schema

Use `--output-schema` to request final responses conforming to a JSON Schema. This supports automated workflows needing stable fields like job summaries, risk reports, or release metadata.

`schema.json`:

```json
{
  "type": "object",
  "properties": {
    "project_name": { "type": "string" },
    "programming_languages": {
      "type": "array",
      "items": { "type": "string" }
    }
  },
  "required": ["project_name", "programming_languages"],
  "additionalProperties": false
}
```

Run Codex with the schema and write the final JSON response to disk:

```
codex exec "Extract project metadata" \
  --output-schema ./schema.json \
  -o ./project-metadata.json
```

Example final output (stdout):

```json
{
  "project_name": "Codex CLI",
  "programming_languages": ["Rust", "TypeScript", "Shell"]
}
```

### Authenticate in automation

`codex exec` reuses saved CLI authentication by default. In CI, provide credentials explicitly.

#### Use API key auth

For GitHub Actions, use the Codex GitHub Action instead of installing and authenticating the CLI directly. This reduces API key exposure by installing Codex, starting a Responses API proxy, and running Codex with configurable safety strategy.

Do not set `OPENAI_API_KEY` or `CODEX_API_KEY` as job-level environment variables in workflows checking out or running repository-controlled code. Build scripts, tests, dependency lifecycle hooks, or compromised actions can read environment variables.

For other automation environments, set `CODEX_API_KEY` only for the single `codex exec` invocation:

```
CODEX_API_KEY=<api-key> codex exec --json "triage open bug reports"
```

`CODEX_API_KEY` is only supported in `codex exec`.

#### Use ChatGPT-managed auth in CI/CD (advanced)

API keys are the default for automation. Use ChatGPT-managed auth only when running CI/CD jobs with a Codex user account instead of an API key.

Treat `~/.codex/auth.json` as sensitive: it contains access tokens. Don't commit, share, or expose it.

### Resume a non-interactive session

Continue previous runs using the `resume` subcommand:

```
codex exec "review the change for race conditions"
codex exec resume --last "fix the race conditions you found"
```

Target specific session IDs with `codex exec resume <SESSION_ID>`.

### Git repository required

Codex requires running inside a Git repository to prevent destructive changes. Override with `codex exec --skip-git-repo-check` if the environment is safe.

### Common automation patterns

#### Example: Autofix CI failures in GitHub Actions

For GitHub Actions, use `openai/codex-action` (https://github.com/openai/codex-action) instead of installing Codex and passing the API key directly. The action secures the API key with a proxy.

This pattern automatically proposes fixes when CI fails:

1. Trigger a follow-up workflow when main CI completes with error
2. Check out the failing commit with read-only repository access
3. Run setup commands before Codex without exposing the API key
4. Run the Codex GitHub Action
5. Save Codex's local changes as a patch artifact
6. In a separate job, apply the patch and open a pull request

The Codex job below has only `contents: read`. After Codex runs, it serializes only the diff as an artifact. The `open_pr` job receives repository write permissions but not `OPENAI_API_KEY`.

The example assumes a Node.js project; adjust setup and test commands for your stack.

```yaml
name: Codex auto-fix on CI failure

on:
  workflow_run:
    workflows: ["CI"]
    types: [completed]

jobs:
  generate_fix:
    if: ${{ github.event.workflow_run.conclusion == 'failure' }}
    runs-on: ubuntu-latest
    permissions:
      contents: read
    outputs:
      has_patch: ${{ steps.diff.outputs.has_patch }}
    steps:
      - uses: actions/checkout@v5
        with:
          ref: ${{ github.event.workflow_run.head_sha }}
          fetch-depth: 0
          persist-credentials: false

      - uses: actions/setup-node@v4
        with:
          node-version: "20"

      - name: Install dependencies
        run: |
          if [ -f package-lock.json ]; then npm ci; fi

      - name: Run Codex
        uses: openai/codex-action@v1
        with:
          openai-api-key: ${{ secrets.OPENAI_API_KEY }}
          prompt: |
            The CI workflow "${{ github.event.workflow_run.name }}" failed for commit
            ${{ github.event.workflow_run.head_sha }}.

            Run `npm test --silent` to reproduce the failure. Identify the minimal
            change needed to make the tests pass, implement only that change, and
            run `npm test --silent` again.

            Do not refactor unrelated files.

      - name: Create patch artifact
        id: diff
        run: |
          git add -N .
          git diff --binary HEAD > codex.patch
          if [ -s codex.patch ]; then
            echo "has_patch=true" >> "$GITHUB_OUTPUT"
          else
            echo "has_patch=false" >> "$GITHUB_OUTPUT"
          fi

      - name: Upload patch artifact
        if: steps.diff.outputs.has_patch == 'true'
        uses: actions/upload-artifact@v4
        with:
          name: codex-fix-patch
          path: codex.patch
          if-no-files-found: error

  open_pr:
    runs-on: ubuntu-latest
    needs: generate_fix
    if: needs.generate_fix.outputs.has_patch == 'true'
    permissions:
      contents: write
      pull-requests: write
    steps:
      - uses: actions/checkout@v5
        with:
          ref: ${{ github.event.workflow_run.head_sha }}
          fetch-depth: 0

      - uses: actions/download-artifact@v4
        with:
          name: codex-fix-patch

      - name: Apply Codex patch
        run: git apply --index codex.patch

      - name: Open pull request
        env:
          GH_TOKEN: ${{ github.token }}
          FAILED_HEAD_BRANCH: ${{ github.event.workflow_run.head_branch }}
          FAILED_HEAD_SHA: ${{ github.event.workflow_run.head_sha }}
          RUN_ID: ${{ github.event.workflow_run.run_id }}
        run: |
          branch="codex/auto-fix-$RUN_ID"

          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git switch -c "$branch"
          git commit -m "Auto-fix failing CI via Codex"
          git push origin "$branch"

          {
            echo "Codex generated this patch after CI failed for \`$FAILED_HEAD_SHA\`."
            echo
            echo "Review the changes before merging."
          } > pr-body.md

          gh pr create \
            --base "$FAILED_HEAD_BRANCH" \
            --head "$branch" \
            --title "Auto-fix failing CI via Codex" \
            --body-file pr-body.md
```

### Advanced stdin piping

Choose the stdin pattern based on where the instruction comes from. Use prompt-plus-stdin when you already know the instruction and want piped output as context. Use `codex exec -` when stdin should become the full prompt.

#### Use prompt-plus-stdin

This pattern is useful when another command produces data for Codex to inspect. Write the instruction and pipe output as context, fitting naturally into CLI workflows with command output, logs, and generated data.

```
npm test 2>&1 \
  | codex exec "summarize the failing tests and propose the smallest likely fix" \
  | tee test-summary.md
```

More prompt-plus-stdin examples:

Summarize logs:

```
tail -n 200 app.log \
  | codex exec "identify the likely root cause, cite the most important errors, and suggest the next three debugging steps" \
  > log-triage.md
```

Inspect TLS or HTTP issues:

```
curl -vv https://api.example.com/health 2>&1 \
  | codex exec "explain the TLS or HTTP failure and suggest the most likely fix" \
  > tls-debug.md
```

Prepare a Slack-ready update:

```
gh run view 123456 --log \
  | codex exec "write a concise Slack-ready update on the CI failure, including the likely cause and next step" \
  | pbcopy
```

Draft a pull request comment from CI logs:

```
gh run view 123456 --log \
  | codex exec "summarize the failure in 5 bullets for the pull request thread" \
  | gh pr comment 789 --body-file -
```

#### Use `codex exec -` when stdin is the prompt

Omit the prompt argument to read from stdin. Use `codex exec -` to force this behavior explicitly.

The `-` sentinel is useful when another command generates the entire prompt dynamically, good for storing prompts in files, assembling them with shell scripts, or combining live command output with instructions before sending to Codex.

```
cat prompt.txt | codex exec -

printf "Summarize this error log in 3 bullets:\n\n%s\n" "$(tail -n 200 app.log)" \
  | codex exec -

generate_prompt.sh | codex exec - --json > result.jsonl
```

---

## Codex code review in GitHub

### Overview

Use Codex code review to get another high-signal review pass on GitHub pull requests. Codex reviews the pull request diff, follows your repository guidance, and posts a standard GitHub code review focused on serious issues.

### Before you start

Make sure you have:

- Codex cloud (/codex/cloud) set up for the repository you want to review.
- Access to Codex code review settings (https://chatgpt.com/codex/settings/code-review).
- An `AGENTS.md` file if you want Codex to follow repository-specific review guidance.

### Set up Codex code review

1. Set up Codex cloud (/codex/cloud).
2. Go to Codex settings (https://chatgpt.com/codex/settings/code-review).
3. Turn on Code review for your repository.

### Request a Codex review

1. In a pull request comment, mention `@codex review`.
2. Wait for Codex to react (eyes emoji) and post a review.

Codex posts a review on the pull request, just like a teammate would. In GitHub, Codex flags only P0 and P1 issues so review comments stay focused on high-priority risks.

### Enable automatic reviews

If you want Codex to review every pull request automatically, turn on Automatic reviews in Codex settings (https://chatgpt.com/codex/settings/code-review). Codex will post a review whenever someone opens a new PR for review, without needing an `@codex review` comment.

### Customize what Codex reviews

Codex searches your repository for `AGENTS.md` files and follows any Review guidelines you include.

To set guidelines for a repository, add or update a top-level `AGENTS.md` with a section like this:

```
## Review guidelines

- Don't log PII.
- Verify that authentication middleware wraps every route.
```

Codex applies guidance from the closest `AGENTS.md` to each changed file. You can place more specific instructions deeper in the tree when particular packages need extra scrutiny.

For a one-off focus, add it to your pull request comment:

`@codex review for security regressions`

If you want Codex to flag typos in documentation, add guidance in `AGENTS.md` (for example, "Treat typos in docs as P1.").

### Act on review findings

After Codex posts a review, you can ask it to fix issues in the same pull request by leaving another comment:

```
@codex fix the P1 issue
```

Codex starts a cloud task with the pull request as context and can push a fix back to the branch when it has permission to do so.

### Give Codex other tasks

If you mention `@codex` in a comment with anything other than `review`, Codex starts a cloud task (/codex/cloud) using your pull request as context.

```
@codex fix the CI failures
```

### Troubleshoot code review

If Codex doesn't react or post a review:

- Confirm you turned on Code review for the repository in Codex settings (https://chatgpt.com/codex/settings/code-review).
- Confirm the pull request belongs to a repository with Codex cloud (/codex/cloud) set up.
- Use the exact trigger `@codex review` in a pull request comment.
- For automatic reviews, check that you turned on Automatic reviews and that the pull request event matches your review trigger settings.

---

## Use Codex in Slack

### Overview

Ask Codex to run tasks from channels and threads by mentioning `@Codex` with a prompt. Codex creates a cloud task and replies with results.

### Set up the Slack app

1. Set up Codex cloud tasks. You need a Plus, Pro, Business, Enterprise, or Edu plan, a connected GitHub account, and at least one environment.
2. Go to Codex settings and install the Slack app for your workspace. An admin may need to approve the install depending on workspace policies.
3. Add `@Codex` to a channel. Slack prompts you to add it when you mention it.

### Start a task

1. In a channel or thread, mention `@Codex` and include your prompt. Codex can reference earlier messages in the thread.
2. (Optional) Specify an environment or repository in your prompt (example: `@Codex fix the above in openai/codex`).
3. Wait for Codex to react and reply with a task link. When finished, Codex posts the result.

#### How Codex chooses an environment and repo

- Codex reviews accessible environments and selects the best match for your request
- Falls back to your most recently used environment if ambiguous
- Runs against the default branch of the first repository in that environment's repo map
- Replies with instructions if no suitable environment or repository is available

#### Enterprise data controls

Enterprise admins can disable answers in workspace settings to prevent Codex from posting environment information in Slack threads. When disabled, Codex replies only with a task link.

#### Data usage, privacy, and security

Message and thread history are shared with Codex to understand requests and create tasks. Data handling follows OpenAI's policies on privacy and terms of use.

#### Tips and troubleshooting

- Missing connections: Codex replies with reconnection links if unable to confirm Slack or GitHub connections
- Unexpected environment choice: Reply in thread with desired environment, then mention `@Codex` again
- Long threads: Summarize key details in your latest message for better context
- Workspace posting restrictions: Some Enterprise workspaces restrict answer posting; open the task link to view progress

---

## Use Codex in Linear

Run Codex tasks from Linear issues

Codex in Linear allows you to delegate work from issues. Assign an issue to Codex or mention `@Codex` in a comment, and Codex creates a cloud task and replies with progress and results.

Codex in Linear is available on paid plans. Enterprise plan users should contact their ChatGPT workspace admin to enable this feature in workspace and connector settings.

### Set up the Linear integration

1. Configure Codex cloud tasks by connecting GitHub in Codex and creating an environment for your repository
2. Go to Codex settings and install Codex for Linear for your workspace
3. Link your Linear account by mentioning `@Codex` in a Linear issue comment thread

### Delegate work to Codex

#### Assign an issue to Codex

Assign issues to Codex like you would to teammates. Codex starts work and posts updates back to the issue.

#### Mention `@Codex` in comments

Mention `@Codex` in comment threads to delegate tasks or ask questions. You can specify a repository by including it in your comment, like: `@Codex fix this in openai/codex`

Track progress by opening the Activity section or the task link for detailed updates.

#### How Codex chooses an environment and repo

- Linear suggests repositories based on issue context
- Codex selects the matching environment, defaulting to your most recently used one if ambiguous
- Tasks run against the default branch of the first repository in the environment's repo map

### Automatically assign issues to Codex

Create triage rules in Linear settings to automatically assign issues to Codex, which then runs tasks using the issue creator's account.

### Data usage, privacy, and security

Issue content is shared with Codex to understand requests. Data handling follows OpenAI's Privacy Policy, Terms of Use, and other applicable policies.

### Tips and troubleshooting

- Check for missing connection links in issue replies
- Specify environment preferences in follow-up comments
- Add context to issues for accurate code targeting
- Consult the OpenAI Help Center for additional support

### Connect Linear for local tasks (MCP)

For ChatGPT desktop app, Codex CLI, or IDE extension users:

#### Use the CLI (recommended)

```
codex mcp add linear --url https://mcp.linear.app/mcp
```

#### Configure manually

Add to `~/.codex/config.toml`:

```
[mcp_servers.linear]
url = "https://mcp.linear.app/mcp"
```

Then run: `codex mcp login linear`

---

## CLI customization

Customize the Codex terminal interface, editor, completions, and shortcuts

The Codex CLI provides terminal-specific options for how interactive sessions look and how you enter commands and prompts.

### Syntax highlighting and themes

The terminal UI (TUI) syntax-highlights fenced Markdown code blocks and file diffs. Run `/theme` to open the theme picker, preview themes, and save your selection to `tui.theme` in `$CODEX_HOME/config.toml`.

To add a custom theme, place a `.tmTheme` file in `$CODEX_HOME/themes`, then select it from the theme picker.

### Shell completions

Generate a completion script for Bash, the Z shell, Fish, or PowerShell:

```
codex completion zsh
```

Load the script from your shell configuration. For the Z shell, add:

```
eval "$(codex completion zsh)"
```

If the Z shell reports `command not found: compdef`, initialize its completion system before loading the Codex completions:

```
autoload -Uz compinit && compinit
eval "$(codex completion zsh)"
```

Restart the shell, type `codex`, and press Tab to verify completion.

### Prompt editor

For longer prompts, press Ctrl+G in the composer to open the editor configured by `VISUAL`, or `EDITOR` when `VISUAL` isn't set. Save and close the editor to return the text to the composer before sending it.

For interactive keyboard controls and the full command and option list, see Commands (/codex/developer-commands?surface=cli#cli-interactive-shortcuts).

---

## Developer commands

Reference for commands and slash commands in Codex developer surfaces

### ChatGPT desktop app

The ChatGPT web composer has its own command menu accessed via `/`. The web interface doesn't expose the Codex app or CLI command set, Codex slash commands, CLI subcommands, and flags documented here don't apply to ChatGPT web.

### How to read this reference

This page catalogs every documented Codex CLI command and flag. Interactive tables allow searching by key or description. Each section indicates stability status (experimental or stable) and highlights risky flag combinations.

The CLI inherits most defaults from `~/.codex/config.toml`. Command-line `-c key=value` overrides take precedence for that invocation. See configuration precedence documentation for more information.

### Global flags

| Key | Type / Values | Details |
|-----|---|---|
| `--add-dir` | `path` | Grant additional directories write access alongside the main workspace. Repeat for multiple paths. |
| `--ask-for-approval, -a` | `untrusted \| on-request \| never` | Control when Codex pauses for human approval before running a command. |
| `--cd, -C` | `path` | Set the working directory for the agent before it starts processing your request. |
| `--config, -c` | `key=value` | Override configuration values. Values parse as TOML if possible; otherwise the literal string is used. |
| `--dangerously-bypass-approvals-and-sandbox, --yolo` | `boolean` | Run every command without approvals or sandboxing. Only use inside an externally hardened environment. |
| `--dangerously-bypass-hook-trust` | `boolean` | Run enabled hooks without requiring persisted hook trust for this invocation. Intended only for automation that already vets hook sources. |
| `--disable` | `feature` | Force-disable a feature flag (translates to `-c features.<name>=false`). Repeatable. |
| `--enable` | `feature` | Force-enable a feature flag (translates to `-c features.<name>=true`). Repeatable. |
| `--image, -i` | `path[,path...]` | Attach one or more image files to the initial prompt. Separate multiple paths with commas or repeat the flag. |
| `--local-provider` | `lmstudio \| ollama` | Choose the local provider used with `--oss`, overriding `oss_provider` for this run. |
| `--model, -m` | `string` | Override the model set in configuration (for example `gpt-5.4`). |
| `--no-alt-screen` | `boolean` | Disable alternate screen mode for the TUI (overrides `tui.alternate_screen` for this run). |
| `--oss` | `boolean` | Use a local open source model provider. Codex uses `--local-provider`, your configured `oss_provider`, or prompts you to choose between LM Studio and Ollama. |
| `--profile, -p` | `string` | Layer `$CODEX_HOME/profile-name.config.toml` on top of the base user config. |
| `--remote` | `ws://host:port \| wss://host:port \| unix:// \| unix://PATH` | Connect to a remote app-server endpoint over WebSocket or a Unix socket. Supported for `codex`, `codex resume`, `codex fork`, `codex archive`, `codex delete`, and `codex unarchive`; other subcommands reject remote mode. |
| `--remote-auth-token-env` | `ENV_VAR` | Read a bearer token from this environment variable and send it when connecting with `--remote`. Requires `--remote`; tokens are only sent over `wss://` URLs or local-only `ws://` URLs. |
| `--sandbox, -s` | `read-only \| workspace-write \| danger-full-access` | Select the sandbox policy for model-generated shell commands. |
| `--search` | `boolean` | Enable live web search (sets `web_search = "live"` instead of the default `"cached"`). |
| `--strict-config` | `boolean` | Error when `config.toml` contains fields this Codex version does not recognize. Supported by runtime commands such as `codex`, `exec`, `review`, `resume`, `fork`, `app-server`, `mcp-server`, and `exec-server`. |
| `PROMPT` | `string` | Optional text instruction to start the session. Omit to launch the TUI without a pre-filled message. |

### Command overview

| Key | Maturity | Details |
|-----|----------|---------|
| `codex` | Stable | Launch the terminal UI. Accepts global flags plus an optional prompt or image attachments. |
| `codex app` | Stable | Launch the ChatGPT desktop app on macOS or Windows. On macOS, Codex can open a workspace path; on Windows, Codex prints the path to open. |
| `codex app-server` | Experimental | Launch the Codex app server for local development or debugging over stdio, WebSocket, or a Unix socket. |
| `codex apply` | Stable | Apply the latest diff generated by a Codex cloud task to your local working tree. Alias: `codex a`. |
| `codex archive` | Stable | Archive a saved interactive session by session ID or session name. |
| `codex cloud` | Experimental | Browse or execute Codex cloud tasks from the terminal without opening the TUI. Alias: `codex cloud-tasks`. |
| `codex completion` | Stable | Generate shell completion scripts for Bash, Zsh, Fish, or PowerShell. |
| `codex debug app-server send-message-v2` | Experimental | Debug app-server by sending a single V2 message through the built-in test client. |
| `codex debug models` | Experimental | Print the raw model catalog Codex sees, including an option to inspect only the bundled catalog. |
| `codex debug prompt-input` | Experimental | Render the model-visible prompt input list as JSON, optionally with a prompt and images. |
| `codex delete` | Stable | Permanently delete a saved interactive session by session ID or session name. |
| `codex doctor` | Stable | Generate a diagnostic report for local installation, config, auth, runtime, Git, terminal, app-server, and thread inventory issues. |
| `codex exec` | Stable | Run Codex non-interactively. Alias: `codex e`. Stream results to stdout or JSONL and optionally resume previous sessions. |
| `codex execpolicy` | Experimental | Evaluate execpolicy rule files and see whether a command would be allowed, prompted, or blocked. |
| `codex features` | Stable | List feature flags and persistently enable or disable them in `config.toml`. |
| `codex fork` | Stable | Fork a previous interactive session into a new task, preserving the original transcript. |
| `codex login` | Stable | Authenticate Codex using ChatGPT OAuth, device auth, an API key, or an access token piped over stdin. |
| `codex logout` | Stable | Remove stored authentication credentials. |
| `codex mcp` | Stable | Manage Model Context Protocol servers (list, add, remove, authenticate). |
| `codex mcp-server` | Stable | Run Codex itself as an MCP server over stdio. Useful when another agent consumes Codex. |
| `codex plugin` | Stable | Install, list, and remove plugins from configured marketplace sources. |
| `codex plugin marketplace` | Stable | Add, list, upgrade, or remove plugin marketplaces from Git or local sources. |
| `codex remote-control` | Experimental | Run or manage remote control for the local app-server, or create a short-lived pairing code. |
| `codex resume` | Stable | Continue a previous interactive session by ID or resume the most recent conversation. |
| `codex review` | Stable | Run a non-interactive review of uncommitted changes, a base branch diff, a commit, or custom review instructions. |
| `codex sandbox` | Stable | Run arbitrary commands inside Codex-provided macOS, Linux, or Windows sandboxes. |
| `codex unarchive` | Stable | Restore an archived interactive session by session ID or session name. |
| `codex update` | Stable | Check for and apply a Codex CLI update when the installed release supports self-update. |

### Command details

#### `codex` (interactive)

Running `codex` with no subcommand launches the interactive terminal UI (TUI). The agent accepts the global flags plus image attachments. Web search defaults to cached mode; use `--search` to switch to live browsing. For low-friction local work, use `--sandbox workspace-write --ask-for-approval on-request`.

Use `--remote ws://host:port` or `--remote wss://host:port` to connect the TUI to an app server started with `codex app-server --listen ws://IP:PORT`. For a local Unix socket, use `--remote unix://` for the default socket or `--remote unix://PATH` for an explicit path. Add `--remote-auth-token-env <ENV_VAR>` when the server requires a bearer token for WebSocket authentication.

#### `codex app-server`

Launch the Codex app server locally. This is primarily for development and debugging and may change without notice.

| Key | Type / Values | Details |
|-----|---|---|
| `--analytics-default-enabled` | `boolean` | Defaults analytics to enabled for first-party app-server clients unless the user opts out in config. |
| `--listen` | `stdio:// \| ws://IP:PORT \| unix:// \| unix://PATH \| off` | Transport listener URL. Use `stdio://` for JSONL, `ws://IP:PORT` for a TCP WebSocket endpoint, `unix://` for the default Unix socket, `unix://PATH` for a custom Unix socket, or `off` to disable the local transport. |
| `--stdio` | `boolean` | Use stdio transport. Equivalent to `--listen stdio://` and mutually exclusive with `--listen`. |
| `--ws-audience` | `string` | Expected `aud` claim for signed bearer tokens. Requires `--ws-auth signed-bearer-token`. |
| `--ws-auth` | `capability-token \| signed-bearer-token` | Authentication mode for app-server WebSocket clients. If omitted, WebSocket auth is disabled; non-local listeners warn during startup. |
| `--ws-issuer` | `string` | Expected `iss` claim for signed bearer tokens. Requires `--ws-auth signed-bearer-token`. |
| `--ws-max-clock-skew-seconds` | `number` | Clock skew allowance when validating signed bearer token `exp` and `nbf` claims. Requires `--ws-auth signed-bearer-token`. |
| `--ws-shared-secret-file` | `absolute path` | File containing the HMAC shared secret used to validate signed JWT bearer tokens. Required with `--ws-auth signed-bearer-token`. |
| `--ws-token-file` | `absolute path` | File containing the shared capability token. Use with `--ws-auth capability-token` unless you provide `--ws-token-sha256` instead. |
| `--ws-token-sha256` | `hexadecimal SHA-256 digest` | Expected SHA-256 digest for capability-token authentication. Use instead of `--ws-token-file` when the client token comes from another source. |

`codex app-server --listen stdio://` keeps the default JSONL-over-stdio behavior, and `codex app-server --stdio` is an alias for that transport. `--listen ws://IP:PORT` enables WebSocket transport for app-server clients. The server accepts `ws://` listen URLs; use TLS termination or a secure proxy when clients connect with `wss://`. Use `--listen unix://` to accept WebSocket handshakes on Codex's default Unix socket, or `--listen unix:///absolute/path.sock` to choose a socket path. If you generate schemas for client bindings, add `--experimental` to include gated fields and methods.

#### `codex remote-control`

Run `codex remote-control` to start remote control in the foreground. Use `codex remote-control start` to start the local app-server daemon with remote control enabled, and `codex remote-control stop` to stop it. Managed remote-control clients and SSH remote workflows use these commands; they aren't a replacement for `codex app-server --listen` when you're building a local protocol client.

After the daemon is running, use `codex remote-control pair` to create and print a short-lived manual pairing code. Add `--json` to any remote-control command for machine-readable output. For `pair`, the JSON response includes `pairingCode`, `manualPairingCode`, `environmentId`, and `expiresAt`.

#### `codex app`

Launch the ChatGPT desktop app from the terminal on macOS or Windows. On macOS, Codex can open a specific workspace path; on Windows, Codex prints the path to open.

| Key | Type / Values | Details |
|-----|---|---|
| `--download-url` | `url` | Advanced override for the ChatGPT desktop app installer URL used during install. |
| `PATH` | `path` | Workspace path for the ChatGPT desktop app. On macOS, Codex opens this path; on Windows, Codex prints the path. |

`codex app` opens an installed ChatGPT desktop app, or starts the installer when the app is missing. On macOS, Codex opens the provided workspace path; on Windows, it prints the path to open after installation.

#### `codex debug app-server send-message-v2`

Send one message through app-server's V2 thread/turn flow using the built-in app-server test client.

| Key | Type / Values | Details |
|-----|---|---|
| `USER_MESSAGE` | `string` | Message text sent to app-server through the built-in V2 test-client flow. |

This debug flow initializes with `experimentalApi: true`, starts a thread, sends a turn, and streams server notifications. Use it to reproduce and inspect app-server protocol behavior locally.

#### `codex debug models`

Print the raw model catalog Codex sees as JSON.

| Key | Type / Values | Details |
|-----|---|---|
| `--bundled` | `boolean` | Skip refresh and print only the model catalog bundled with the current Codex binary. |

Use `--bundled` when you want to inspect only the catalog bundled with the current binary, without refreshing from the remote models endpoint.

#### `codex debug prompt-input`

Render the exact model-visible prompt input list as JSON. Use this when debugging instruction discovery, session context, or prompt construction.

| Key | Type / Values | Details |
|-----|---|---|
| `--image, -i` | `path[,path...]` | Attach one or more images to the user prompt. Separate multiple paths with commas or repeat the flag. |
| `PROMPT` | `string` | Optional user prompt appended after the session context. |

#### `codex apply`

Apply the most recent diff from a Codex cloud task to your local repository. You must authenticate and have access to the task.

| Key | Type / Values | Details |
|-----|---|---|
| `TASK_ID` | `string` | Identifier of the Codex cloud task whose diff should be applied. |

Codex prints the patched files and exits non-zero if `git apply` fails (for example, due to conflicts).

#### `codex review`

Run a code review non-interactively. Choose exactly one review target, or pass custom review instructions as a prompt.

| Key | Type / Values | Details |
|-----|---|---|
| `--base` | `branch` | Review changes against the specified base branch. |
| `--commit` | `SHA` | Review the changes introduced by the specified commit. |
| `--strict-config` | `boolean` | Error when `config.toml` contains fields this Codex version does not recognize. |
| `--title` | `string` | Set the commit title shown in the review summary. Requires `--commit`. |
| `--uncommitted` | `boolean` | Review staged, unstaged, and untracked changes. |
| `PROMPT` | `string \| - (read stdin)` | Custom review instructions. Use `-` to read the instructions from stdin. |

`--uncommitted`, `--base`, `--commit`, and a custom `PROMPT` conflict with one another. Use `--title` only with `--commit`.

#### `codex archive` and `codex unarchive`

Archive or restore a saved interactive session by session ID or session name. Use these commands when you want to clean up the session picker without deleting the transcript. Session IDs take precedence over session names.

```
codex archive <SESSION>
codex unarchive <SESSION>
```

| Key | Type / Values | Details |
|-----|---|---|
| `--remote` | `ws://host:port \| wss://host:port \| unix:// \| unix://PATH` | Connect to a remote app-server endpoint before changing archive state. |
| `--remote-auth-token-env` | `ENV_VAR` | Read a bearer token from this environment variable when `--remote` requires authentication. |
| `SESSION` | `session ID \| session name` | Saved session to archive or restore. Session IDs take precedence over session names. |

#### `codex delete`

Permanently delete a saved interactive session by session ID or session name. Use this only when you want to remove the transcript instead of hiding it from active session lists.

```
codex delete <SESSION>
codex delete <SESSION_UUID> --force
```

| Key | Type / Values | Details |
|-----|---|---|
| `--force` | `boolean` | Delete without prompting. The session argument must be a UUID; names still require interactive confirmation. |
| `--remote` | `ws://host:port \| wss://host:port \| unix:// \| unix://PATH` | Connect to a remote app-server endpoint before deleting the session. |
| `--remote-auth-token-env` | `ENV_VAR` | Read a bearer token from this environment variable when `--remote` requires authentication. |
| `SESSION` | `session ID \| session name` | Saved session to delete. Session IDs take precedence over session names. |

Use `--force` only with a session UUID. Named sessions still require confirmation so Codex doesn't delete a repeated or ambiguous name without a prompt.

#### `codex cloud`

Interact with Codex cloud tasks from the terminal. The default command opens an interactive picker; `codex cloud exec` submits a task directly, and `codex cloud list` returns recent tasks for scripting or quick inspection.

| Key | Type / Values | Details |
|-----|---|---|
| `--attempts` | `1-4` | Number of assistant attempts (best-of-N) Codex cloud should run. |
| `--env` | `ENV_ID` | Target Codex cloud environment identifier (required). Use `codex cloud` to list options. |
| `QUERY` | `string` | Task prompt. If omitted, Codex prompts interactively for details. |

Authentication follows the same credentials as the main CLI. Codex exits non-zero if the task submission fails.

##### `codex cloud list`

List recent cloud tasks with optional filtering and pagination.

| Key | Type / Values | Details |
|-----|---|---|
| `--cursor` | `string` | Pagination cursor returned by a previous request. |
| `--env` | `ENV_ID` | Filter tasks by environment identifier. |
| `--json` | `boolean` | Emit machine-readable JSON instead of plain text. |
| `--limit` | `1-20` | Maximum number of tasks to return. |

Plain-text output prints a task URL followed by status details. Use `--json` for automation. The JSON payload contains a `tasks` array plus an optional `cursor` value. Each task includes `id`, `url`, `title`, `status`, `updated_at`, `environment_id`, `environment_label`, `summary`, `is_review`, and `attempt_total`.

#### `codex completion`

Generate shell completion scripts and redirect the output to the appropriate location, for example `codex completion zsh > "${fpath[1]}/_codex"`.

| Key | Type / Values | Details |
|-----|---|---|
| `SHELL` | `bash \| zsh \| fish \| power-shell \| elvish` | Shell to generate completions for. Output prints to stdout. |

#### `codex doctor`

Generate a local diagnostic report before filing a support issue or while investigating a broken Codex installation. The report checks installation, configuration, authentication, runtime, Git, terminal, app-server, and thread inventory health.

| Key | Type / Values | Details |
|-----|---|---|
| `--all` | `boolean` | Expand long lists in the detailed human-readable report. |
| `--ascii` | `boolean` | Use ASCII status labels and separators in human-readable output. |
| `--json` | `boolean` | Emit a redacted machine-readable support report. |
| `--no-color` | `boolean` | Disable ANSI color in human-readable output. |
| `--summary` | `boolean` | Show grouped check rows and the final count summary only. |

#### `codex features`

Manage feature flags stored in `$CODEX_HOME/config.toml`. The `enable` and `disable` commands persist changes so they apply to future sessions. The `features` subcommand doesn't accept `--profile`.

| Key | Type / Values | Details |
|-----|---|---|
| Disable subcommand | `codex features disable <feature>` | Persistently disable a feature flag in `$CODEX_HOME/config.toml`. |
| Enable subcommand | `codex features enable <feature>` | Persistently enable a feature flag in `$CODEX_HOME/config.toml`. |
| List subcommand | `codex features list` | Show known feature flags, their maturity stage, and their effective state. |

#### `codex exec`

Use `codex exec` (or the short form `codex e`) for scripted or CI-style runs that should finish without human interaction.

| Key | Type / Values | Details |
|-----|---|---|
| `--cd, -C` | `path` | Set the workspace root before executing the task. |
| `--color` | `always \| never \| auto` | Control ANSI color in stdout. |
| `--dangerously-bypass-approvals-and-sandbox, --yolo` | `boolean` | Bypass approval prompts and sandboxing. Dangerous, only use inside an isolated runner. |
| `--dangerously-bypass-hook-trust` | `boolean` | Run enabled hooks without requiring persisted hook trust for this invocation. Intended only for automation that already vets hook sources. |
| `--ephemeral` | `boolean` | Run without persisting session rollout files to disk. |
| `--full-auto` | `boolean` | Deprecated compatibility flag. Prefer `--sandbox workspace-write`; Codex prints a warning when this flag is used. |
| `--ignore-rules` | `boolean` | Do not load user or project execpolicy `.rules` files for this run. |
| `--ignore-user-config` | `boolean` | Do not load `$CODEX_HOME/config.toml`. Authentication still uses `CODEX_HOME`. |
| `--image, -i` | `path[,path...]` | Attach images to the first message. Repeatable; supports comma-separated lists. |
| `--json, --experimental-json` | `boolean` | Print newline-delimited JSON events instead of formatted text. |
| `--local-provider` | `lmstudio \| ollama` | Choose the local provider used with `--oss`, overriding `oss_provider` for this run. |
| `--model, -m` | `string` | Override the configured model for this run. |
| `--oss` | `boolean` | Use a local open source provider. Codex uses `--local-provider` or your configured `oss_provider`, and exits with an error if neither is set. |
| `--output-last-message, -o` | `path` | Write the assistant's final message to a file. Useful for downstream scripting. |
| `--output-schema` | `path` | JSON Schema file describing the expected final response shape. Codex validates tool output against it. |
| `--profile, -p` | `string` | Layer `$CODEX_HOME/profile-name.config.toml` on top of the base user config. |
| `--sandbox, -s` | `read-only \| workspace-write \| danger-full-access` | Sandbox policy for model-generated commands. Defaults to configuration. |
| `--skip-git-repo-check` | `boolean` | Allow running outside a Git repository (useful for one-off directories). |
| `-c, --config` | `key=value` | Inline configuration override for the non-interactive run (repeatable). |
| `PROMPT` | `string \| - (read stdin)` | Initial instruction for the task. Use `-` to pipe the prompt from stdin. |
| Resume subcommand | `codex exec resume [SESSION_ID]` | Resume an exec session by ID or add `--last` to continue the most recent session from the current working directory. Add `--all` to consider sessions from any directory. Accepts an optional follow-up prompt. |

Codex writes formatted output by default. Add `--json` to receive newline-delimited JSON events (one per state change). The optional `resume` subcommand lets you continue non-interactive tasks. Use `--last` to pick the most recent session from the current working directory, or add `--all` to search across all sessions:

| Key | Type / Values | Details |
|-----|---|---|
| `--all` | `boolean` | Include sessions outside the current working directory when selecting the most recent session. |
| `--image, -i` | `path[,path...]` | Attach one or more images to the follow-up prompt. Separate multiple paths with commas or repeat the flag. |
| `--last` | `boolean` | Resume the most recent conversation from the current working directory. |
| `PROMPT` | `string \| - (read stdin)` | Optional follow-up instruction sent immediately after resuming. |
| `SESSION_ID` | `uuid \| session name` | Resume the specified session. Omit and use `--last` to continue the most recent session. |

#### `codex execpolicy`

Check `execpolicy` rule files before you save them. `codex execpolicy check` accepts one or more `--rules` flags (for example, files under `~/.codex/rules`) and emits JSON showing the strictest decision and any matching rules. Add `--pretty` to format the output. The `execpolicy` command is currently in preview.

| Key | Type / Values | Details |
|-----|---|---|
| `--pretty` | `boolean` | Pretty-print the JSON result. |
| `--rules, -r` | `path (repeatable)` | Path to an execpolicy rule file to evaluate. Provide multiple flags to combine rules across files. |
| `COMMAND...` | `var-args` | Command to be checked against the specified policies. |

#### `codex login`

Authenticate the CLI with a ChatGPT account, API key, or access token. With no flags, Codex opens a browser for the ChatGPT OAuth flow.

| Key | Type / Values | Details |
|-----|---|---|
| `--device-auth` | `boolean` | Use OAuth device code flow instead of launching a browser window. |
| `--with-access-token` | `boolean` | Read an access token from stdin (for example `printenv CODEX_ACCESS_TOKEN \| codex login --with-access-token`). |
| `--with-api-key` | `boolean` | Read an API key from stdin (for example `printenv OPENAI_API_KEY \| codex login --with-api-key`). |
| status subcommand | `codex login status` | Print the active authentication mode and exit with 0 when logged in. |

`codex login status` exits with `0` when credentials are present, which is helpful in automation scripts.

#### `codex logout`

Remove saved credentials for both API key and ChatGPT authentication. This command has no flags.

#### `codex mcp`

Manage Model Context Protocol server entries stored in `~/.codex/config.toml`.

| Key | Type / Values | Details |
|-----|---|---|
| `add <name>` | `-- <command...> \| --url <value>` | Register a server using a stdio launcher command or a streamable HTTP URL. Supports `--env KEY=VALUE` for stdio transports. |
| `get <name>` | `--json` | Show a specific server configuration. `--json` prints the raw config entry. |
| `list` | `--json` | List configured MCP servers. Add `--json` for machine-readable output. |
| `login <name>` | `--scopes scope1,scope2` | Start an OAuth login for a streamable HTTP server (servers that support OAuth only). |
| `logout <name>` | | Remove stored OAuth credentials for a streamable HTTP server. |
| `remove <name>` | | Delete a stored MCP server definition. |

The `add` subcommand supports both stdio and streamable HTTP transports:

| Key | Type / Values | Details |
|-----|---|---|
| `--bearer-token-env-var` | `ENV_VAR` | Environment variable whose value is sent as a bearer token when connecting to a streamable HTTP server. |
| `--env KEY=VALUE` | `repeatable` | Environment variable assignments applied when launching a stdio server. |
| `--oauth-client-id` | `CLIENT_ID` | OAuth client identifier for a streamable HTTP MCP server. Requires `--url`. |
| `--oauth-resource` | `RESOURCE` | OAuth resource parameter to include during login for a streamable HTTP MCP server. Requires `--url`. |
| `--url` | `https://…` | Register a streamable HTTP server instead of stdio. Mutually exclusive with `COMMAND...`. |
| `COMMAND...` | `stdio transport` | Executable plus arguments to launch the MCP server. Provide after `--`. |

OAuth actions (`login`, `logout`) only work with streamable HTTP servers (and only when the server supports OAuth).

#### `codex plugin`

Install, list, and remove plugins from configured marketplaces.

| Key | Type / Values | Details |
|-----|---|---|
| `add <plugin[@marketplace]>` | `[--marketplace, -m NAME] [--json]` | Install a plugin from a configured marketplace. Use `--marketplace` or `-m` when the plugin argument omits `@marketplace`. |
| `list` | `[--marketplace, -m NAME] [--available --json] [--json]` | List installed plugins. With `--json`, output has `installed` and `available` arrays; `--available` includes uninstalled marketplace plugins and requires `--json`. |
| `marketplace` | | Manage configured marketplace sources. See `codex plugin marketplace` below. |
| `remove <plugin[@marketplace]>` | `[--marketplace, -m NAME] [--json]` | Remove an installed plugin from local config and cache. Use `--json` for automation-friendly output. |

`codex plugin add --json` prints `pluginId`, `name`, `marketplaceName`, `version`, `installedPath`, and `authPolicy`. `codex plugin list --json` prints `installed` and `available` arrays. Entries include `pluginId`, `name`, `marketplaceName`, `version`, `installed`, `enabled`, `source`, `installPolicy`, `authPolicy`, and, when available, `marketplaceSource` with the configured marketplace source type and value. `codex plugin remove --json` prints `pluginId`, `name`, and `marketplaceName`.

#### `codex plugin marketplace`

Manage plugin marketplace sources that Codex can browse and install from.

| Key | Type / Values | Details |
|-----|---|---|
| `add <source>` | `[--ref REF] [--sparse PATH] [--json]` | Install a plugin marketplace from GitHub shorthand, a Git URL, an SSH URL, or a local marketplace root directory. `--sparse` is supported only for Git sources and can be repeated. |
| `list` | `[--json]` | Show plugin marketplaces Codex is currently considering and the root path for each marketplace. |
| `remove <marketplace-name>` | `[--json]` | Remove a configured plugin marketplace. |
| `upgrade [marketplace-name]` | `[--json]` | Refresh one configured Git marketplace, or all configured Git marketplaces when no name is provided. |

`codex plugin marketplace add` accepts GitHub shorthand such as `owner/repo` or `owner/repo@ref`, HTTP or HTTPS Git URLs, SSH Git URLs, and local marketplace root directories. Use `--ref` to pin a Git ref, and repeat `--sparse PATH` to use a sparse checkout for Git-backed marketplace repositories.

`codex plugin marketplace list` prints in-scope marketplace names and roots, including implicitly discovered default marketplaces and configured marketplace snapshots.

Add `--json` to marketplace add, list, upgrade, or remove commands for automation-friendly output. Marketplace add JSON includes `marketplaceName`, `installedRoot`, and `alreadyAdded`; list JSON includes a `marketplaces` array with `name`, `root`, and optional `marketplaceSource`; upgrade JSON includes `selectedMarketplaces`, `upgradedRoots`, and `errors`; remove JSON includes `marketplaceName` and `installedRoot`.

#### `codex mcp-server`

Run Codex as an MCP server over stdio so that other tools can connect. This command inherits global configuration overrides and exits when the downstream client closes the connection.

#### `codex resume`

Continue an interactive session by ID or resume the most recent conversation. `codex resume` scopes `--last` to the current working directory unless you pass `--all`. It accepts the same global flags as `codex`, including model and sandbox overrides.

| Key | Type / Values | Details |
|-----|---|---|
| `--all` | `boolean` | Include sessions outside the current working directory when selecting the most recent session. |
| `--include-non-interactive` | `boolean` | Include non-interactive sessions in the picker and `--last` selection. |
| `--last` | `boolean` | Skip the picker and resume the most recent conversation from the current working directory. |
| `SESSION_ID` | `uuid \| session name` | Resume the specified session. Omit and use `--last` to continue the most recent session. |

#### `codex fork`

Fork a previous interactive session into a new task. By default, `codex fork` opens the session picker; add `--last` to fork your most recent session instead.

| Key | Type / Values | Details |
|-----|---|---|
| `--all` | `boolean` | Show sessions beyond the current working directory in the picker. |
| `--last` | `boolean` | Skip the picker and fork the most recent conversation automatically. |
| `SESSION_ID` | `uuid` | Fork the specified session. Omit and use `--last` to fork the most recent session. |

#### `codex sandbox`

Use the sandbox helper to run a command under the same policies Codex uses internally.

##### macOS seatbelt

| Key | Type / Values | Details |
|-----|---|---|
| `--allow-unix-socket` | `path` | Allow the sandboxed command to bind or connect Unix sockets rooted at this path. Repeat to allow multiple paths. |
| `--cd, -C` | `DIR` | Working directory used for profile resolution and command execution. Requires `--permission-profile`. |
| `--config, -c` | `key=value` | Pass configuration overrides into the sandboxed run (repeatable). |
| `--include-managed-config` | `boolean` | Include managed requirements while resolving an explicit permissions profile. Requires `--permission-profile`. |
| `--log-denials` | `boolean` | Capture macOS sandbox denials with `log stream` while the command runs and print them after exit. |
| `--permission-profile, -P` | `NAME` | Apply a named permissions profile from the active configuration stack. |
| `--profile, -p` | `NAME` | Layer `$CODEX_HOME/NAME.config.toml` on top of the base user config. |
| `COMMAND...` | `var-args` | Shell command to execute under macOS Seatbelt. Everything after `--` is forwarded. |

##### Linux Landlock

| Key | Type / Values | Details |
|-----|---|---|
| `--cd, -C` | `DIR` | Working directory used for profile resolution and command execution. Requires `--permission-profile`. |
| `--config, -c` | `key=value` | Configuration overrides applied before launching the sandbox (repeatable). |
| `--include-managed-config` | `boolean` | Include managed requirements while resolving an explicit permissions profile. Requires `--permission-profile`. |
| `--permission-profile, -P` | `NAME` | Apply a named permissions profile from the active configuration stack. |
| `--profile, -p` | `NAME` | Layer `$CODEX_HOME/NAME.config.toml` on top of the base user config. |
| `COMMAND...` | `var-args` | Command to execute under Landlock + seccomp. Provide the executable after `--`. |

##### Windows

| Key | Type / Values | Details |
|-----|---|---|
| `--cd, -C` | `DIR` | Working directory used for profile resolution and command execution. Requires `--permission-profile`. |
| `--config, -c` | `key=value` | Configuration overrides applied before launching the sandbox (repeatable). |
| `--include-managed-config` | `boolean` | Include managed requirements while resolving an explicit permissions profile. Requires `--permission-profile`. |
| `--permission-profile, -P` | `NAME` | Apply a named permissions profile from the active configuration stack. |
| `--profile, -p` | `NAME` | Layer `$CODEX_HOME/NAME.config.toml` on top of the base user config. |
| `COMMAND...` | `var-args` | Command to execute under the native Windows sandbox. Provide the executable after `--`. |

#### `codex update`

Check for and apply a Codex CLI update when the installed release supports self-update. Debug builds print a message telling you to install a release build instead.

### Flag combinations and safety tips

- Use `--sandbox workspace-write` for unattended local work that can stay inside the workspace, and avoid `--dangerously-bypass-approvals-and-sandbox` unless you are inside a dedicated sandbox VM.
- When you need to grant Codex write access to more directories, prefer `--add-dir` rather than forcing `--sandbox danger-full-access`.
- Pair `--json` with `--output-last-message` in CI to capture machine-readable progress and a final natural-language summary.

### Interactive shortcuts

- Type `@` to search for a file in the workspace and add its path to the prompt.
- Press Up or Down to restore draft history.
- Press Ctrl+R to search prompt history, then press Enter to use a match or Esc to cancel.
- Press Ctrl+O or run `/copy` to copy the latest completed Codex output.
- Prefix a line with `!` to run a local shell command under the current approval and sandbox settings.
- Press Tab while Codex is working to queue a follow-up prompt, slash command, or shell command for the next turn.
- Press Enter while Codex is working to inject new instructions into the current turn.
- Press Esc twice with an empty composer to edit the previous user message and fork the conversation from that point.
- Press Ctrl+C or run `/exit` to close the session.

### Related resources

- Codex CLI overview (/codex/cli): installation, upgrades, and quick tips.
- Config basics (/codex/config-file/config-basic): persist defaults like the model and provider.
- Advanced Config (/codex/config-file/config-advanced): profiles, providers, sandbox tuning, and integrations.
- AGENTS.md (/codex/agent-configuration/agents-md): conceptual overview of Codex agent capabilities and best practices.

Slash commands give you fast, keyboard-first control over Codex. Type `/` in the composer to open the slash popup, choose a command, and Codex will perform actions such as switching models, adjusting permissions, or summarizing long conversations without leaving the terminal.

This guide shows you how to:

- Find the right built-in slash command for a task
- Steer an active session with commands like `/model`, `/fast`, `/personality`, `/permissions`, `/approve`, `/raw`, `/agent`, and `/status`

### Built-in slash commands

Codex ships with the following commands. Open the slash popup and start typing the command name to filter the list.

When a task is already running, you can type a slash command and press `Tab` to queue it for the next turn. Codex parses queued slash commands when they run, so command menus and errors appear after the current turn finishes. Slash completion still works before you queue the command.

| Command | Purpose | When to use it |
|---------|---------|----------------|
| `/permissions` | Set what Codex can do without asking first. | Relax or tighten approval requirements mid-session, such as switching between Auto and Read Only. |
| `/ide` | Include open files, current selection, and other IDE context. | Pull editor context into the next prompt without re-explaining what's open in your IDE. |
| `/keymap` | Remap TUI keyboard shortcuts. | Inspect and persist custom shortcut bindings in `config.toml`. |
| `/vim` | Toggle Vim mode for the composer. | Switch between Vim normal/insert behavior and the default composer editing mode. |
| `/setup-default-sandbox` | Set up the elevated agent sandbox (Windows only). | Replace the degraded Windows sandbox after Codex offers the elevated setup. |
| `/sandbox-add-read-dir` | Grant sandbox read access to an extra directory (Windows only). | Unblock commands that need to read an absolute directory path outside the current readable roots. |
| `/agent`, `/subagents` | Switch the active agent thread. | Inspect or continue work in a spawned subagent thread. |
| `/apps` | Browse apps (connectors) and insert them into your prompt. | Attach an app as `$app-slug` before asking Codex to use it. |
| `/plugins` | Browse installed and discoverable plugins. | Inspect plugin tools, install suggested plugins, or manage plugin availability. |
| `/hooks` | View and manage lifecycle hooks. | Inspect configured hooks, trust new or changed hooks, or disable non-managed hooks before they run. |
| `/clear` | Clear the terminal and start a new task. | Reset the visible UI and task context together when you want a fresh start. |
| `/rename` | Rename the current task. | Give a saved session a recognizable name without leaving the TUI. |
| `/archive` | Archive the current session and exit Codex. | Remove the current session from active session lists without deleting its transcript. |
| `/delete` | Permanently delete the current session and exit Codex. | Remove the transcript and descendant sessions when archiving isn't enough. |
| `/compact` | Summarize the visible conversation to free tokens. | Use after long runs so Codex retains key points without blowing the context window. |
| `/copy` | Copy the latest completed Codex output. | Grab the latest finished response or plan text without manually selecting it. You can also press `Ctrl+O`. |
| `/diff` | Show the Git diff, including files Git isn't tracking yet. | Review Codex's edits before you commit or run tests. |
| `/exit` | Exit the CLI (same as `/quit`). | Alternative spelling; both commands exit the session. |
| `/experimental` | Toggle experimental features. | Enable options such as Network proxy or Prevent sleep while running. |
| `/approve` | Approve one retry of a recent auto review denial. | Retry a command or action that the auto reviewer denied. |
| `/memories` | Configure memory use and generation. | Turn memory injection or memory generation on or off without leaving the TUI. |
| `/skills` | Browse and use skills. | Improve task-specific behavior by selecting a relevant local skill. |
| `/import` | Import Claude Code setup, project files, and recent chats. | Migrate supported external-agent artifacts into Codex configuration and local files. |
| `/feedback` | Send logs to the Codex maintainers. | Report issues or share diagnostics with support. |
| `/init` | Generate an `AGENTS.md` scaffold in the current directory. | Capture persistent instructions for the repository or subdirectory you're working in. |
| `/logout` | Sign out of Codex. | Clear local credentials when using a shared machine. |
| `/mcp` | List configured Model Context Protocol (MCP) tools. | Check which external tools Codex can call during the session; add `verbose` for server details. |
| `/mention` | Attach a file to the conversation. | Point Codex at specific files or folders you want it to inspect next. |
| `/model` | Choose the active model (and reasoning effort, when available). | Switch between models such as `gpt-5.4-mini` and `gpt-5.5` before running a task. |
| `/fast` | Toggle a Fast service tier when the model catalog exposes one. | Turn the current model's Fast tier on or off and persist the selection. |
| `/plan` | Switch to plan mode and optionally send a prompt. | Ask Codex to propose an execution plan before implementation work starts. |
| `/goal` | Set, edit, pause, resume, view, or clear a task goal. | Give Codex a persistent target to track while a larger task runs. |
| `/personality` | Choose a communication style for responses. | Make Codex more concise, more explanatory, or more collaborative without changing your instructions. |
| `/ps` | Show background terminals and their recent output. | Check long-running commands without leaving the main transcript. |
| `/stop` | Stop all background terminals. | Cancel background terminal work started by the current session. |
| `/fork` | Fork the current task into a new task. | Branch the active session to explore a new approach without losing the current transcript. |
| `/app` | Continue the current session in the ChatGPT desktop app. | Move from the TUI to the desktop app on macOS or Windows. |
| `/side`, `/btw` | Start an ephemeral side conversation. | Ask a focused follow-up without disrupting the main task's transcript. |
| `/raw` | Toggle raw scrollback mode. | Make terminal selection and copying less formatted while reviewing long output. |
| `/resume` | Resume a saved conversation from your session list. | Continue work from a previous CLI session without starting over. |
| `/new` | Start a new task inside the same CLI session. | Reset the task context without leaving the CLI when you want a fresh prompt in the same repo. |
| `/quit` | Exit the CLI. | Leave the session immediately. |
| `/review` | Ask Codex to review your working tree. | Run after Codex completes work or when you want a second set of eyes on local changes. |
| `/status` | Display session configuration and token usage. | Confirm the active model, approval policy, writable roots, and remaining context capacity. |
| `/usage` | View account token usage or use a rate-limit reset. | Inspect daily, weekly, or cumulative ChatGPT token activity from inside the TUI. |
| `/debug-config` | Print config layer and requirements diagnostics. | Debug precedence and policy interpretation when configuration is unclear. |

---

## Developer settings

Configure development settings across ChatGPT and Codex clients

### ChatGPT web

ChatGPT web stores product and workspace preferences in ChatGPT settings. Work conversations run in a managed environment and don't read local Codex configuration files. Use the controls your workspace exposes in ChatGPT settings; workspace administrators may manage some settings for you.

The general Settings page covers app preferences, including profile, keyboard shortcuts, notifications, appearance, personalization, memories, and archived tasks.

### Project and terminal behavior

Choose where files open, how much command output appears in tasks, and where terminal tabs open by default.

### Code review

Under Settings > Git, use Review delivery to choose Inline to run `/review` in the current task when possible or Detached to start a separate review task.

### IDE extension sync

When the ChatGPT desktop app and IDE extension are open in the same project, they share active tasks and editor context. Turn on IDE context from the app composer to let Codex use files currently open in your editor. Turn it off when you want the app prompt to exclude that editor context.

You can open an app task from the IDE extension and continue an IDE task in the app. Both surfaces use the same project to determine which tasks and context to share.

### Agent configuration

Codex agents in the app inherit the same configuration as the IDE extension and CLI. Use the in-app controls for common settings, or edit `config.toml` for advanced options.

### Git

Use Git settings to standardize branch naming and choose whether Codex uses force pushes. You can also set prompts that Codex uses to generate commit messages and pull request descriptions.

### Integrations and MCP

Connect external tools through Model Context Protocol (MCP). Enable recommended servers or add your own. If a server requires OAuth, the app starts the authentication flow. These settings also apply to the Codex CLI and IDE extension because MCP configuration lives in `config.toml`.

### Browser developer mode

Under Developer mode, turn on Enable full CDP access to let ChatGPT use the Chrome DevTools Protocol for performance profiling and deeper browser debugging. If your organization has disabled full CDP access, you can't enable it locally.

### Codex CLI

Codex CLI reads your personal defaults from `~/.codex/config.toml`. Add a `.codex/config.toml` file to a trusted project or subfolder when you need project-specific overrides. The CLI and IDE extension share these configuration layers.

### Inspect your settings

Use these commands to understand the effective settings for the current session:

- Run `/status` to see the active model, approval policy, writable roots, and token usage.
- Run `/debug-config` to see the configuration layers in precedence order and the source of managed policy requirements.
- Start Codex with `--strict-config` to treat unrecognized `config.toml` keys as errors instead of ignoring them.

### Change settings for one run

Use a dedicated flag when one exists. Common examples include `--model`, `--sandbox`, `--ask-for-approval`, `--profile`, and `--search`. Use `-c` or `--config` to override any supported configuration key for one run:

```
codex --model gpt-5.5
codex --profile deep-review
codex --config model_reasoning_effort='"high"'
```

Command-line flags and `--config` values have the highest precedence.

### Configuration layers

The CLI applies command-line flags and `--config` overrides before project, profile, user, system, and built-in settings. Use that precedence to keep shared defaults in configuration files and one-off changes on the command line.

### Change settings in the TUI

The interactive terminal UI provides pickers for common session and display settings:

| Goal | Command | Related configuration |
|------|---------|----------------------|
| Choose a model | `/model` | `model`, `model_reasoning_effort` |
| Change permissions | `/permissions` | Approval and sandbox settings |
| Choose a response style | `/personality` | `personality` |
| Configure optional tools | `/experimental`, `/memories` | Feature-specific `config.toml` keys |
| Customize the terminal UI | `/keymap`, `/statusline`, `/title`, `/theme` | Keys under `[tui]` |
| Set editing defaults | `/vim`, `/raw` | `tui.vim_mode_default`, `tui.raw_output_mode` |

Some commands apply only to the current session, while some pickers offer to save the choice.

### Codex IDE extension

The Codex IDE extension has two settings layers:

- Codex settings control agent behavior shared with Codex CLI, including the model, reasoning effort, permissions, sandbox, MCP servers, and personalization. Codex reads these settings from `config.toml`.
- Editor settings control how the extension behaves inside VS Code and compatible editors. These settings use `chatgpt.*` keys in the editor's settings system.

### Open Codex settings

Select the gear icon in the Codex sidebar, then select Codex Settings. Use the settings panel for common agent controls, or select Open config.toml to edit the active configuration layer directly.

### Change an editor setting

To change a setting, follow these steps:

1. Open your editor settings.
2. Search for `@ext:openai.chatgpt`, `Codex`, or the setting name.
3. Update the value.

The extension also honors VS Code's built-in chat font settings for Codex conversation surfaces.

### Editor settings reference

| Setting | Default | Description |
|---------|---------|-------------|
| `chatgpt.commentCodeLensEnabled` | `true` | Show CodeLens above `TODO` comments so Codex can address them. |
| `chatgpt.openOnStartup` | `false` | Focus the Codex sidebar when the extension finishes starting. |
| `chatgpt.followUpQueueMode` | `queue` | Choose whether messages sent during a run wait for the next run (`queue`) or steer the current run (`steer`). The extension treats the legacy `interrupt` value as `steer`. Press Cmd/Ctrl+Shift+Enter to invert the behavior for one message. |
| `chatgpt.composerEnterBehavior` | `enter` | Choose whether Enter always sends (`enter`), Cmd/Ctrl+Enter sends multiline prompts (`cmdIfMultiline`), or the modifier is always required (`cmdAlways`). |
| `chatgpt.reviewDelivery` | `inline` | Run `/review` in the current task when possible (`inline`) or start a separate review task (`detached`). |
| `chatgpt.localeOverride` | Auto | Set the preferred language for the Codex UI. Leave empty to detect it automatically. |
| `chatgpt.runCodexInWindowsSubsystemForLinux` | `false` | Windows only: Run Codex in WSL when WSL is available. Use this when your repositories and tooling live in WSL2 or when you need Linux-native tooling. Changing this setting reloads VS Code. |
| `chatgpt.cliExecutable` | Unset | Development only: Set the path to the Codex CLI executable. You don't need this setting unless you're developing the Codex CLI; manually overriding the bundled executable can prevent parts of the extension from working. |
| `chat.fontSize` | Editor default | Control chat text in the Codex sidebar, including conversation content and the composer. |
| `chat.editor.fontSize` | Editor default | Control code-rendered content in Codex conversations, including code snippets and diffs. |

The `chatgpt.*` keys above belong to the IDE extension and don't go in `config.toml`. For shared agent settings, use the configuration documentation.
