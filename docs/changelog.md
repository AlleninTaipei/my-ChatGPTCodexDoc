# Codex changelog

Latest updates to Codex, OpenAI's coding agent

## July 2026

### 2026-07-14

**Codex CLI 0.144.4**

```
$ npm install -g @openai/codex@0.144.4
```

**Chores**

- No user-facing changes in this patch release.

---

### 2026-07-13

**ChatGPT for iOS 1.2026.188**

**New features**

- Added support for inline visualizations in Codex tasks.

**Improvements and bug fixes**

- Improved creating and managing tasks from conversations, with reliable links to newly created tasks.
- Improved tool activity styling and progress indicators.
- Improved file-opening feedback.
- Improved the composer so controls remain visible above the keyboard for long prompts and larger text sizes.
- Fixed Fast mode selection and restoration for each task.
- Fixed initial prompts ignoring the selected approval preset.
- Fixed autocomplete backgrounds and task rows becoming unresponsive during swipe gestures.

---

**Codex CLI 0.144.3**

```
$ npm install -g @openai/codex@0.144.3
```

**Chores**

- Published a version-only release with no merged pull request changes since `rust-v0.144.2`.

---

**Codex CLI 0.144.2**

```
$ npm install -g @openai/codex@0.144.2
```

**Bug Fixes**

- Restored the previous Guardian auto-review policy, request format, and tool behavior after rolling back a prompting regression.

---

### 2026-07-09

**Codex joins the ChatGPT desktop app**

"Codex is now part of the ChatGPT desktop app on macOS and Windows" with project preservation and existing setting maintenance.

**New features**

- Edit Markdown and code directly in the app, use inline annotations, and ask Codex to revise selected content.
- Review GitHub pull requests in the sidebar, with reviewer feedback alongside the diff, without leaving the app.
- Work across repositories in one project.

**Performance improvements and bug fixes**

- Made Computer Use faster with GPT-5.6.
- Made task activity and progress easier to follow while Codex works.
- Simplified plugin management by moving it into Settings.
- Improved mobile connection reliability and fixed video rendering for SSH projects.
- Additional performance improvements and bug fixes.

---

**Codex CLI 0.144.1**

```
$ npm install -g @openai/codex@0.144.1
```

**Bug Fixes**

- Fixed standalone installs failing when GitHub returns compact or reordered release metadata.
- Ensured macOS package installs expose the code-mode host alongside the `codex` executable.
- Kept code mode working when the companion host binary is unavailable by falling back to the embedded runtime.

---

**Codex CLI 0.144.0**

```
$ npm install -g @openai/codex@0.144.0
```

**New Features**

- "Usage-limit reset credits now show their type and expiration" with the ability to choose which credit to redeem.
- Added a `writes` app-approval mode that allows declared read-only actions while prompting for writes.
- MCP tools can now request authentication interactively without an experimental opt-in.
- App-server hosts can provide Codex authentication at runtime and redirect successful logins to a hosted page.
- Global pnpm installs are now detected so diagnostics and updates use the correct package manager.
- Selecting Ultra reasoning now warns when high multi-agent concurrency could increase usage quickly.

**Bug Fixes**

- Resumed ChatGPT threads recover when compaction references a retired model by retrying with the currently selected model.
- Fixed Code Mode crashes in Intel macOS release binaries.
- Windows sandbox sessions can delete files in writable roots and access the managed primary runtime.
- Pasted terminal control sequences can no longer corrupt TUI rendering or resumed conversation history.
- Long-running app sessions now refresh expired authentication for the hosted `codex_apps` connector.
- Responses WebSockets continue using the low-latency transport while respecting system proxies and custom certificate authorities.

**Documentation**

- Device-code login warnings now explain how to recognize and stop phishing attempts.

---

### 2026-07-08

**Codex CLI 0.143.0**

```
$ npm install -g @openai/codex@0.143.0
```

**New Features**

- "Remote plugins are now enabled by default, with richer catalog rows, npm marketplace sources, and visible remote/local versions."
- Codex can route authentication and Responses API traffic through macOS and Windows system proxies, including PAC and WPAD configurations.
- Added `codex remote-control pair` for generating manual pairing codes from a running daemon.
- Added Amazon Bedrock GPT-5.6 Sol, Terra, and Luna models, with first-class support for `max` reasoning effort.
- "MCP tools now use tool search by default, and ChatGPT-hosted MCP servers can explicitly use session authentication."
- App-server clients can inspect environments, list descendant threads, and fork history through a specific turn.

**Bug Fixes**

- Fixed Windows ConPTY input handling for line endings and backspace, plus sandbox credential retry edge cases.
- Fixed stale TUI safety prompts and cancelled reviews that could leave MCP startup appearing busy.
- Improved recovery when exec servers are temporarily offline and prevented remote-control token refresh retry storms.
- Preserved trailing realtime transcript text and terminal rollout events during shutdown.
- Improved incremental WebSocket request success by ignoring response metadata during comparisons.
- Reduced installer failures from GitHub API rate limits by reusing release metadata.

**Documentation**

- Documented UUID7 thread and turn IDs, plus recommended remote-executor integration-test workflows.

**Chores**

- Updated OpenSSL, Hono, fast-uri, quick-xml, and crossbeam-epoch to address security advisories.

---

### 2026-07-06

**ChatGPT for iOS 1.2026.181**

**New features**

- Added support for creating, searching, opening, forking, and managing Codex tasks directly from a conversation.
- Added filters for staged, unstaged, branch, and last-turn changes, with controls for comparing branches.
- Added support for adding selected transcript text directly to the composer.
- Added previews for image and file attachments before sending.
- Added inline Photos and Camera pickers to the attachment menu.
- Added a connection shortcut and support for SSH hosts using private keys or no credentials.
- Added usage limits and credit details to the task menu.

**Improvements and bug fixes**

- Improved the task list with consistent task terminology, clearer delegated task titles, and a Needs input status.
- Improved initial task loading and foreground recovery.
- Improved autocomplete by selecting the first result automatically and accepting it with Return.
- Improved model, reasoning, and Fast settings so changes remain scoped to the current task.
- Improved task-management and dynamic tool activity presentation.
- Improved side chats to open directly when only one conversation is available.
- Improved plugin autocomplete with installed plugins and their icons.
- Improved workspace diff accuracy and expand-and-collapse navigation.
- Improved recovery by preserving thread state across reconnects and host pairings across sign-out.
- Fixed stuck thread-list loading, prompt mode deadlocks, stale images, and microphone permission alerts.
- Fixed shake to undo and keyboard refocusing after sending a prompt.

---

### 2026-07-01

**Codex CLI 0.142.5**

```
$ npm install -g @openai/codex@0.142.5
```

**Bug Fixes**

- Prevented full Responses WebSocket request payloads from being written to trace logs.

---

## June 2026

### 2026-06-29

**Codex CLI 0.142.4**

```
$ npm install -g @openai/codex@0.142.4
```

**Chores**

- No user-facing changes were identified for this release.

---

### 2026-06-26

**Codex CLI 0.142.3**

```
$ npm install -g @openai/codex@0.142.3
```

**Chores**

- Maintenance-only patch release with no user-facing changes since 0.142.2.

---

### 2026-06-25

**Codex Remote reaches general availability**

"Codex Remote has reached general availability. Use Codex from the ChatGPT mobile app to start or continue work on a connected Mac or Windows host, review progress, and approve actions from your phone."

Remote Control now uses authenticated one-to-one QR pairing between each iOS or Android device and each host. Update the ChatGPT mobile app and Codex App to the latest versions before connecting. Connections used since June 8, 2026, remain paired; older inactive connections need to pair again.

The new DigitalOcean plugin lets Codex provision a DigitalOcean Droplet, configure SSH access, and connect it to the Codex App as a remote workspace.

---

**Codex CLI 0.142.2**

```
$ npm install -g @openai/codex@0.142.2
```

**New Features**

- "MCP tools now use tool search by default when supported" while maintaining compatibility with older models and providers.
- macOS authentication clients can honor system proxy, PAC, and WPAD settings when `respect_system_proxy` is enabled.
- Plugins can provide dedicated dark-mode logos through local manifests and remote catalogs.
- Apps can display richer safety-buffering UI using server-provided visibility and faster-model metadata.

**Bug Fixes**

- Remote plugin catalogs now return curated featured-plugin rankings.
- Expired Amazon Bedrock credentials now produce actionable recovery guidance instead of a generic authorization error.
- Remote stdio MCP servers now accept absolute working directories written in the remote platform's path format.
- "Remote HTTP(S) image inputs now return clear model-visible validation errors" while local images remain supported.
- PowerShell commands containing executable AST regions the safety classifier cannot inspect now require approval.
- Code Mode now warns when the selected model lacks the required metadata.

**Chores**

- Updated bundled OpenSSL and esbuild dependencies to patched releases.
- Successful formatter runs are now quiet while failures still show diagnostics.
