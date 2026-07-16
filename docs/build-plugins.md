# Build plugins

Create, test, and distribute plugins for ChatGPT

## Overview

This documentation targets plugin authors. Users seeking to browse, install, and use plugins should visit the [Plugins section](/codex/plugins). For single-repository or personal workflows, start with local skills. Build a plugin when sharing workflows across teams, bundling connectors or MCP configuration, packaging lifecycle hooks, or publishing stable packages.

A plugin can include skills, an MCP-backed app, or both. For service connections or MCP server tool exposure, see [Build an app](/codex/build-app).

Public examples are available in [Figma](https://github.com/openai/plugins/tree/main/plugins/figma), [Notion](https://github.com/openai/plugins/tree/main/plugins/notion), and [Build web apps](https://github.com/openai/plugins/tree/main/plugins/build-web-apps) repositories.

## Create a plugin with `@plugin-creator`

The built-in `@plugin-creator` skill provides the fastest setup path.

It scaffolds the required `.codex-plugin/plugin.json` manifest and generates local marketplace entries for testing. Existing plugin folders can still use `@plugin-creator` to integrate into local marketplaces.

### Create and test a plugin locally with MCP-server-backed dev-mode app

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

## Build your own curated plugin list

A marketplace is a JSON plugin catalog. `@plugin-creator` generates one for a single plugin; you can expand it into a curated list by adding entries.

In Work or Codex, marketplaces appear as selectable sources in the plugin directory. Use `$REPO_ROOT/.agents/plugins/marketplace.json` for repository scope or `~/.agents/plugins/marketplace.json` for personal scope. Add entries under `plugins[]`, point each `source.path` at the plugin folder with `./`-prefixed relative paths, and set `interface.displayName` for the marketplace label shown in the picker. Restart the ChatGPT desktop app afterward.

One marketplace can expose a single testing plugin or grow into a larger curated catalog as you add plugins.

### Add a marketplace from the CLI

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

### Create a plugin manually

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

### Install a local plugin manually

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

### Share a local plugin with your workspace

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

## Marketplace metadata

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

### How the ChatGPT desktop app uses marketplaces

The ChatGPT desktop app can read marketplace files from:

- Official curated Plugins Directory marketplace
- Repo marketplace at `$REPO_ROOT/.agents/plugins/marketplace.json`
- Legacy-compatible marketplace at `$REPO_ROOT/.claude-plugin/marketplace.json`
- Personal marketplace at `~/.agents/plugins/marketplace.json`

Any marketplace-exposed plugin is installable. The app installs plugins to `~/.codex/plugins/cache/$MARKETPLACE_NAME/$PLUGIN_NAME/$VERSION/`. For local plugins, `$VERSION` is `local`, and the app loads from that cache path rather than directly from the marketplace entry.

Plugins can be individually enabled or disabled. The app stores each plugin's state in `~/.codex/config.toml`.

## Package and distribute plugins

### Plugin structure

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

### Manifest fields

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

### Path rules

- Keep manifest paths relative to plugin root, starting with `./`
- Store visual assets (`composerIcon`, `logo`, `screenshots`) under `./assets/` when possible
- Use `skills` for skill folders, `apps` for `.app.json`, `mcpServers` for `.mcp.json`, `hooks` for lifecycle hooks
- Enabled plugins can include lifecycle hooks alongside skills, MCP servers, and connectors
- If hooks are stored at `./hooks/hooks.json`, no `hooks` manifest entry is required; Codex checks this default automatically

### Bundled MCP servers and lifecycle hooks

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

### Publish official public plugins

To publish plugins for public use, submit through the plugin submission portal. See [Submit plugins](/codex/submit-plugins) for the complete review and publishing process.
