# Plugins

Browse, install, and use plugins in ChatGPT and Codex clients

## Overview

Plugins bundle capabilities into reusable workflows in ChatGPT. They can include skills, an MCP-backed app, or both. Plugins are available in ChatGPT Work on the web and in Work or Codex in the ChatGPT desktop app. Codex CLI and the IDE extension can also browse and install plugins for a Codex environment.

In the ChatGPT desktop app, open Plugins from Work or Codex to browse, install, and use plugins. Installed plugins can add skills, connectors, and MCP tools to new chats.

In ChatGPT web, select ChatGPT Work and open Plugins to browse, install, and use plugins. A plugin can prompt you to connect an external service before its tools become available.

In Codex CLI, enter `/plugins` to open the plugin browser. Install a plugin from a configured marketplace, then start a new session before using its bundled skills or tools.

In the IDE extension, open Settings > Plugins to browse and install plugins for the connected Codex host. Start a new chat after installation before using the plugin's bundled skills or tools.

Extend what ChatGPT can do, for example:

- Install the Codex Security plugin to scan authorized code and confirm plausible vulnerability findings.
- Install the Gmail plugin to let ChatGPT read and manage Gmail.
- Install the Google Drive plugin to work across Drive, Docs, Sheets, and Slides.
- Install the Slack plugin to summarize channels or draft replies.

A plugin can contain one or more of these parts:

- Skills: reusable instructions for specific kinds of work. ChatGPT can load them when needed so it follows the right steps and uses the right references or helper scripts for a task.
- Apps: connections to tools like GitHub, Slack, or Google Drive, so ChatGPT can read information from those tools and take actions in them. Apps expose tools and can optionally include custom ChatGPT UI.
- MCP servers: services that give ChatGPT access to more tools or shared information, often from systems outside your local project. They're also the services behind apps. They define tools, enforce auth, return structured data, and perform actions against external systems.
- Browser extensions: browser capabilities that a plugin needs for its workflow.
- Hooks: commands that run at configured lifecycle points. Review and trust plugin hooks before you enable them.
- Scheduled task templates: reusable starting points for recurring tasks where scheduled tasks are available.

You can share plugins by publishing them through a marketplace source, such as a repo marketplace for a project or team. See Build plugins (/codex/build-plugins) for marketplace setup, packaging, and distribution guidance.

If you are building an integration, start with Build an app (/codex/build-app). If the app needs custom ChatGPT UI, use the Apps SDK (https://developers.openai.com/apps-sdk) for that optional UI layer.

## Use and install plugins

### Plugins in ChatGPT

To browse and install curated plugins:

- On the web, select Work and open Plugins.
- In the ChatGPT desktop app, select Work or Codex and open Plugins.

The plugin directory organizes plugins into tabs:

- OpenAI: plugins built by OpenAI.
- Your workspace name: plugins provided by your workspace.
- Personal: personal marketplace plugins, including Created by me and Shared with me sections when those plugins are available.

Use the separate Installed row to review plugins you already installed.

### Install and use a plugin in ChatGPT

Once you open the plugin directory:

1. Search or browse for a plugin, then open its details.
2. Select the plus button to install the plugin.
3. If the plugin needs a connector, connect it when prompted. Some plugins ask you to authenticate during install. Others wait until the first time you use them.
4. After installation, start a new task and ask ChatGPT to use the plugin.

After you install a plugin, you can use it directly in the prompt window:

Describe the task directly

Ask for the outcome you want, such as "Summarize unread Gmail threads from today" or "Pull the latest launch notes from Google Drive."

Use this when you want ChatGPT to choose the right installed tools for the task.

Choose a specific plugin

Type `@` to invoke the plugin or one of its bundled skills explicitly.

Use this when you want to be specific about which plugin or skill ChatGPT should use. See Skills & Plugins (/codex/skills-and-plugins).

### Plugin directory in Codex CLI

In Codex CLI, run the following command to open the plugin browser:

```
codex
/plugins
```

The CLI plugin browser groups plugins by marketplace. Use the marketplace tabs to switch sources, open a plugin to inspect details, install or uninstall marketplace entries, and press Space on an installed plugin to turn it on or off.

### Plugin directory in the IDE extension

Open the extension settings and select Plugins. Browse or search the directory, open a plugin to inspect its details, and install it for the selected Codex host. Start a new chat before using the plugin's bundled skills or tools.

### How permissions and data sharing work

On ChatGPT web, hosted Work conversations use the workspace permissions and tools available to that conversation. Connectors still require their own sign-in and access.

When a plugin capability runs through a Codex host, the host's sandbox and approval policy applies. Connections to external services use that service's own authentication and access controls.

- Bundled skills become available when you start a new chat or CLI session after installation.
- If a plugin includes connectors, ChatGPT may prompt you to install or sign in to those connectors in ChatGPT during setup or the first time you use them.
- If a plugin includes MCP servers, they may require extra setup or authentication before you can use them.
- When ChatGPT sends data through a bundled connector, that service's terms and privacy policy apply.

### Remove a plugin

To remove a plugin, reopen it from the plugin browser and select Uninstall plugin when that action is available. Workspace-installed or default plugins may not offer that action; your workspace administrator controls them instead.

Uninstalling a plugin removes the plugin bundle from that ChatGPT or Codex environment, but bundled connectors stay connected until you manage them in ChatGPT.

## Build your own plugin

If you want to create, test, or distribute your own plugin, see Build plugins (/codex/build-plugins). That page covers local scaffolding, manual marketplace setup, workspace sharing, plugin manifests, and packaging guidance.

If your plugin includes an app, see Build an app (/codex/build-app). Apps are MCP-backed integrations. They can work with tools alone, or add custom ChatGPT UI through the Apps SDK (https://developers.openai.com/apps-sdk) when a visual surface helps the workflow.

When your plugin is ready for review, see Submit plugins (/codex/submit-plugins) for the OpenAI Platform submission flow, required permissions, review materials, MCP checks, and test case requirements.

## Plugin guides

- Record & Replay (/codex/extend/record-and-replay): Show ChatGPT a workflow once and turn it into a reusable skill.
- Codex Security plugin (/codex/security/plugin): Scan authorized code, confirm findings, and prepare reviewed fixes.
