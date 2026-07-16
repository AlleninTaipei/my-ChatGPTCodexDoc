# Glossary

## Overview

Use this glossary as a quick reference for Codex terms across the app, CLI, IDE extension, cloud, SDK, and related integrations.

## Terms and Definitions

| Term | Definition | Applies to |
|------|-----------|-----------|
| Agent | The Codex agent that reasons over context, uses tools, and completes a task. | Desktop app, CLI, IDE extension, Cloud |
| AGENTS.md | Repository or user guidance file that gives Codex persistent instructions. | Desktop app, CLI, IDE extension, Cloud |
| Analytics dashboard | Admin hub for ChatGPT workspace adoption and Codex focused reporting. | Enterprise |
| API key sign-in | Authentication using an OpenAI API key. | Desktop app, CLI, IDE extension |
| Approval policy | Rules for when Codex must ask before taking an action. | Desktop app, CLI, IDE extension |
| Approval request | Codex asking to allow a restricted action. | Desktop app, CLI, IDE extension |
| Apps (configuration) | Compatibility name used by Codex configuration and app server fields for connectors. | Desktop app, CLI, IDE extension |
| Appshot | Snapshot of the frontmost app window sent to a Work or Codex task. | Desktop app |
| Auth cache | Locally stored login credentials reused by Codex. | Desktop app, CLI, IDE extension |
| Automatic approval review | Model based review of eligible approval requests before they proceed. | Desktop app, CLI, IDE extension |
| Chat | An ordinary ChatGPT conversation containing prompts and responses. Quick Chat opens this same conversation type. | Desktop app, Web, Mobile |
| ChatGPT desktop app | Desktop app with ChatGPT Work, Codex, and a separate Quick Chat window, including projects, file previews, scheduled tasks, and developer tools. | Desktop |
| ChatGPT sign-in | Authentication using a ChatGPT account and workspace permissions. | Desktop app, CLI, IDE extension, Cloud |
| Chronicle | Opt in feature that builds memories from recent screen context. | Desktop app |
| Cloud | Mode where Codex works remotely in an OpenAI managed environment. | Desktop app, IDE extension, Web |
| Cloud environment | Configured container setup used for Codex cloud tasks. | Cloud |
| Cloud task | A remotely executed Codex task that runs in a cloud environment. | Cloud |
| Codex | OpenAI's coding agent for software development tasks. | Desktop app, CLI, IDE extension, Web, Cloud, SDK |
| Codex app-server | Local JSON-RPC server for embedding Codex threads, turns, approvals, history, and streamed events in custom clients. | Desktop app, IDE extension, SDK |
| Codex CLI | Terminal client for running Codex interactively or in scripts. | Terminal |
| Codex cloud | OpenAI managed execution environment where Codex can work on repository tasks remotely. | Web, Desktop app, IDE extension |
| codex exec | CLI command for running Codex non interactively from scripts or CI. | CLI |
| Codex IDE extension | Editor integration for using Codex inside IDEs like VS Code, JetBrains IDEs, Cursor, and Windsurf. | IDE |
| Codex SDK | Programmatic interface for building Codex powered workflows or integrations. | SDK |
| Codex-managed worktree | A temporary worktree Codex creates and manages for a task. | Desktop app |
| Compaction | Summarizing older context so long running work can continue. | Desktop app, CLI, IDE extension, Cloud |
| Compliance API | API for exporting supported ChatGPT workspace records and audit metadata. | Enterprise |
| Computer Use | Desktop capability that lets ChatGPT interact with other applications through the UI. | Desktop app |
| Computer Use in the browser | Capability that lets ChatGPT operate the built in browser directly. | Desktop app |
| config.toml | Local Codex configuration files. | Desktop app, CLI, IDE extension |
| Connected host | Computer or development environment that provides files, tools, and shell access for Work or Codex tasks opened through Remote. | Desktop app, Mobile |
| Connector | Connector that lets Work or Codex access external services through a plugin. | Desktop app (Work, Codex), Web (Work) |
| Container cache | Saved cloud container state reused to speed up future tasks. | Cloud |
| Context | Information Codex can use while working, such as files, prior messages, tool output, and instructions. | Desktop app, CLI, IDE extension, Cloud, SDK |
| Context window | The maximum amount of information the model can consider at once. | Desktop app, CLI, IDE extension, Cloud, SDK |
| Custom agent | User defined agent role with its own instructions and settings. | Desktop app, CLI |
| Deny-read rule | Filesystem permission rule that prevents Codex from reading sensitive paths or glob matches. | Desktop app, CLI, IDE extension, Enterprise |
| Diff | Set of Git file changes shown for inspection, comments, staging, or reverting. | Desktop app, Git, Review |
| Domain allowlist | Set of domains Codex cloud can reach when agent internet access is enabled. | Cloud |
| Environment (local) | Desktop app configuration that tells Codex how to set up worktrees for a project. | Desktop app, Worktree |
| Environment variable | Runtime configuration value available during task execution. | Cloud, CLI, IDE extension |
| Ephemeral session | Non interactive run that skips saving session state after it completes. | CLI |
| Fast mode | Speed setting that makes supported models respond faster at a higher credit cost. | CLI, IDE extension |
| Filesystem permission | Permission profile rule that grants or denies read and write access to paths. | Desktop app, CLI, IDE extension |
| Finding | A notable result or issue surfaced by a scheduled task. | Desktop app |
| Full access | Mode where Codex runs without normal sandbox restrictions. | Desktop app, CLI, IDE extension |
| Git worktree | A second checkout of the same repository for parallel branch work. | Desktop app, Git |
| Handoff | Moving a task and its work between Local and Worktree. | Desktop app |
| Heartbeat | Shorthand for recurring scheduled work that returns ChatGPT to the same task. | Desktop app |
| Hook | A lifecycle handler that runs when a Codex event matches, such as tool use, permission requests, or when a turn stops. | Desktop app, CLI, IDE extension |
| Hook event | Lifecycle point where configured hook handlers can run. | Desktop app, CLI, IDE extension |
| Hunk | Contiguous section of a diff that can be staged, unstaged, or reverted independently. | Desktop app, Git, Review |
| Inline comment | Line specific feedback attached to a diff. | Desktop app |
| Live web search | Real time web lookup for current information. | Desktop app, CLI, IDE extension |
| Local | Mode where Codex works on the user's computer. | Desktop app, CLI, IDE extension |
| Local task | A Work or Codex task that runs on the user's machine. | Desktop app, CLI, IDE extension |
| Maintenance script | Optional script run when a cached cloud container resumes. | Cloud |
| Managed configuration | Organization controlled Codex defaults and restrictions. | Enterprise |
| MCP | Model Context Protocol, a standard for connecting Codex to external tools and context. | Desktop app, CLI, IDE extension |
| MCP resource | Readable context exposed by an MCP server for Codex to inspect. | Desktop app, CLI, IDE extension |
| MCP server | External tool or context provider exposed through MCP. | Desktop app, CLI, IDE extension |
| MCP tool | Action exposed by an MCP server that Codex can call during a task. | Desktop app, CLI, IDE extension |
| MDM | Mobile device management tooling for distributing device profiles and managed Codex settings. | Enterprise |
| Memories | Locally stored context Codex can reuse across sessions. | Desktop app, CLI, IDE extension |
| Model | The AI model Codex uses for reasoning and tool work. | Desktop app, CLI, IDE extension, Cloud, SDK |
| Network access | Permission for commands or environments to reach the internet. | Desktop app, CLI, IDE extension, Cloud |
| Network policy | Domain based allow and deny rules that constrain sandboxed outbound network traffic. | Desktop app, CLI, IDE extension |
| Non-interactive mode | CLI mode for running Codex from scripts or CI. | CLI |
| Output schema | JSON Schema passed to codex exec to constrain the final response. | CLI |
| Permanent worktree | A long lived worktree kept as its own project. | Desktop app |
| Permission profile | Named least privilege policy that combines filesystem and network rules for local command execution. | Desktop app, CLI, IDE extension |
| Plan | Codex's proposed or tracked steps for completing a task. | Desktop app, CLI, IDE extension, Cloud |
| Plugin | Installable bundle that can distribute skills, tools, and integrations. | Desktop app (Work, Codex), Web (Work), CLI |
| Plugin manifest | Plugin metadata file that identifies a plugin and points to bundled skills, apps, MCP servers, hooks, and metadata. | Desktop app, CLI, IDE extension, Plugins |
| Prefix rule | Command rule pattern that allows, prompts for, or forbids matching command prefixes. | Desktop app, CLI, IDE extension, Enterprise |
| Profile | Named configuration preset for Codex. | CLI, IDE extension |
| Progressive disclosure | Loading skill details only when needed to preserve context. | Desktop app, CLI, IDE extension |
| Project | A group of related chats or tasks and shared sources, or a local folder used for file based work. | Desktop app |
| Prompt | A question, instruction, or goal sent to ChatGPT or Codex. | Desktop app, CLI, IDE extension, Cloud, SDK |
| Pull request review | Codex review of changes or feedback on a pull request. | Desktop app, CLI, GitHub |
| RBAC | Role based access control for workspace permissions. | Enterprise |
| Read-only mode | Mode where Codex can inspect but not modify without approval. | Desktop app, CLI, IDE extension |
| Reasoning effort | Setting that controls how much reasoning budget a model uses. | Desktop app, CLI, IDE extension, SDK |
| Remote connection | Connection that lets you access Work or Codex tasks on another device through a connected host. | Desktop app, Mobile |
| requirements.toml | Admin enforced requirements file for managed Codex setups. | Enterprise |
| Review pane | Desktop app view for inspecting diffs, comments, and Git changes. | Desktop app |
| Rules | Policies that allow, prompt for, or deny command prefixes or permission exceptions. | Desktop app, CLI, IDE extension |
| Sandbox | Enforced boundary limiting what Codex commands can access or modify. | Desktop app, CLI, IDE extension |
| Sandbox mode | Configuration that defines Codex's filesystem and network limits. | Desktop app, CLI, IDE extension |
| Sandbox preset | SDK shorthand for common sandbox policies such as read only, workspace write, or full access. | SDK |
| Schedule | The timing rule for a scheduled task. | Desktop app |
| Scheduled run | One execution of a scheduled task that may report findings or archive itself. | Desktop app, Web |
| Scheduled task | A scheduled or recurring task in ChatGPT. | Desktop app, Web |
| Scheduled work | Work scheduled from an existing task that returns to the same task context on each scheduled run. | Desktop app, Web |
| Secret | Encrypted value available to setup scripts but removed before the agent phase. | Cloud |
| Setup script | Script run before the agent starts to install dependencies or prepare tools. | Desktop app worktrees |
| Skill | Reusable workflow package with instructions and optional scripts or references. | Desktop app, CLI, IDE extension |
| Skill invocation | Explicit or implicit activation of a skill. | Desktop app, CLI, IDE extension |
| Slash command | Command entered with a leading slash to control or inspect a Codex CLI session. | CLI |
| Standalone scheduled task | Scheduled task whose runs each start a new task and report findings in Triage. | Desktop app, Web |
| STDIO MCP server | MCP server launched as a local process by a configured command and arguments. | CLI, IDE extension |
| Streamable HTTP MCP server | MCP server reached over HTTP, optionally with bearer token or OAuth authentication. | CLI, IDE extension |
| Subagent | Specialized child agent spawned to work on part of a task. | Desktop app, CLI |
| Subagent workflow | Workflow where Codex runs delegated agents in parallel and combines their results. | Desktop app, CLI |
| Task | Durable unit of Work or Codex activity containing its context, messages, results, and actions. | Desktop app, Web, Mobile, CLI, IDE extension, Cloud |
| Thread | Technical conversation object exposed by Codex app server APIs and protocol fields. | App-server, SDK |
| Thread fork | New thread branched from the stored history of an existing thread. | App-server, SDK |
| Turn | One exchange in a chat or task, usually a user prompt plus the agent's response and actions. | Desktop app, CLI, IDE extension, Cloud, SDK |
| Universal image | Default Codex cloud container image with common tools preinstalled. | Cloud |
| Web search cache | Pre indexed search results Codex can use without live browsing. | Desktop app, CLI, IDE extension |
| Work | ChatGPT Work is an agent for research, analysis, and creating documents, presentations, spreadsheets, and other finished work. | Desktop app, Web |
| Worktree | Mode where Codex isolates changes in a separate Git worktree. | Desktop app |
| Writable roots | Directories Codex is allowed to modify. | Desktop app, CLI, IDE extension |
