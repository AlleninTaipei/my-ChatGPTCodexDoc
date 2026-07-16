# Chrome extension

Use Chrome with ChatGPT while managing browser permissions, website approvals, and browsing data

## Overview

ChatGPT integrates with Chrome to handle browser tasks requiring your signed-in browser state. This is particularly useful for accessing services like LinkedIn, Salesforce, Gmail, or internal tools that require authentication.

For development servers, file-backed previews, and public pages without sign-in requirements, the built-in browser is recommended. ChatGPT intelligently switches between tools, using plugins for dedicated integrations, Chrome for logged-in contexts, and the built-in browser for localhost.

Important: Review websites before permitting ChatGPT to continue, treating page content as untrusted context.

## Setup Process

### Install Chrome Plugin

1. Open the ChatGPT desktop app and select ChatGPT Work or Codex
2. Navigate to Plugins and add the Chrome plugin
3. Follow the setup flow to install the Chrome extension and approve permission prompts
4. Verify the extension displays Connected status in Chrome

Once configured, ChatGPT suggests Chrome when tasks require signed-in websites. You can also invoke it directly:

```
@Chrome open Salesforce and update the account from these call notes.
```

Chrome tasks run in tab groups, keeping work organized by task.

## Website Access Control

By default, ChatGPT requests confirmation before interacting with new websites based on domain hosts (e.g., `example.com`).

### Response Options

When prompted about website access:

- Allow for current task only
- Always allow the domain without future prompts
- Decline access

### Allowlist and Blocklist Management

In Computer Use settings, manage domain access:

- Allowlist: Domains ChatGPT uses without asking
- Blocklist: Domains ChatGPT cannot access

Removing entries restores the prompt or blocks behavior.

#### Always Allow Browser Content

This elevated-risk option eliminates confirmation prompts for all websites.

#### Browser History Access

Browser history may contain sensitive telemetry, internal URLs, and search terms. Enabling this elevated-risk feature allows ChatGPT to use relevant history entries as context, though malicious content increases data exposure risks.

## Data and Security

### Chrome Extension Permissions

Installation requires accepting permissions including:

- Page debugger access
- Read/modify website data
- Browse history access on signed-in devices
- Notifications display
- Bookmark management
- Download management
- Native application communication
- Tab group management

These permissions enable browser workflows while ChatGPT applies its own confirmations and controls.

### Memories Integration

Computer Use respects your Memories setting. With Memories enabled, ChatGPT can reference saved memories during Chrome tasks. Disabled Memories prevents their use.

### Data Storage

OpenAI does not maintain separate records of Chrome actions. Activity is stored only when included in ChatGPT context (page text, screenshots, tool calls, summaries, messages). Your ChatGPT data controls apply to processed content. Avoid sending secrets or highly sensitive data unless necessary and you review each prompt.

## Troubleshooting

If ChatGPT cannot connect to Chrome:

1. Check blocklist: Verify the target website isn't blocked in Settings
2. Verify extension status: Open the extension and confirm Connected appears. If disconnected or missing native host, remove and re-add the Chrome plugin from Plugins, then follow setup again
3. Enable plugin: In Work or Codex Plugins, ensure Chrome plugin is active
4. Verify profile: Confirm you're using the Chrome profile where the extension is installed (multiple profiles require per-profile installation)
5. Restart task: Start a new task in Work or Codex to clear connection state
6. Restart browsers: Restart Chrome and the app, then retry. If issues persist, uninstall the extension, remove and re-add the plugin, and follow setup
7. Contact support: If the extension shows Connected but ChatGPT still cannot use Chrome, run `/feedback` and include the task ID

### File Upload Configuration

For Chrome tasks involving file uploads:

1. In Chrome, open the extensions icon and select Manage Extensions
2. Click Details on the extension card
3. Enable Allow access to file URLs

Restart the Chrome task after changing this setting.
