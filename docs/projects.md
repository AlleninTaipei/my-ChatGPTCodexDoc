# Projects, chats, and tasks

Keep related chats, tasks, files, and sources together in ChatGPT

## Choose a project or start without one

Create a project when work will continue over time, produce more than one output, or depend on the same files and sources. Start a task without a project when the work is self-contained and doesn't need shared project context.

Use a project to keep related chats, files, instructions, and sources together. The same project can contain chats started in Chat and ChatGPT Work.

## Each project structure

Each project has a **Chats** section for conversations and a **Sources** section for uploaded files and connected context. Project instructions apply across its chats. A ChatGPT project doesn't provide direct access to a folder on your computer, so upload or connect the sources you want ChatGPT to use.

Chat and Work change how ChatGPT approaches a request, not what the conversation is called. In either experience, start a new chat from the project and find it again under **Chats**.

## Work in different environments

### Codex CLI
Codex CLI treats the directory where you start it as the project for the task. Run `codex` from the directory you want Codex to work in, or pass `--cd <directory>` (`-C`) to set it explicitly. The CLI doesn't expose the ChatGPT Projects view.

### IDE Extension
The IDE extension treats the folder or workspace open in your IDE as the local project. In a multi-root workspace, select the workspace root for the task. The extension doesn't expose the ChatGPT Projects view from the web or desktop app.

## Work in a project

The **Projects** view brings ChatGPT projects and local projects into one place. ChatGPT projects carry project files and context across related tasks. A local project gives tasks access to one or more folders on your computer, such as a collection of source files or a codebase.

Start a separate task for each distinct outcome so its messages and results stay focused while the project keeps related work organized.

## Organize projects and tasks

Keep active work visible and move finished work out of the way:

- **Pin a project** to keep it near the top of the sidebar. You can also pin it from the Projects view.
- **Pin a task** when you return to it often, even if newer tasks appear in the project.
- **Rename a task** with a short title that describes its outcome, such as "Q3 launch brief" or "Checkout accessibility review."
- **Search projects** from the Projects view. Press Cmd/Ctrl+G to search past tasks when you remember a phrase or branch name but not the title.
- **Archive a task** when you finish the work. From a project's menu, select **Archive tasks** to archive its tasks together.

Pinning doesn't add context or change what ChatGPT can access. It only changes where the project or task appears in the sidebar.

Restore archived tasks from **Settings > Archived tasks**.

## Use local projects for folders and codebases

Add a local project when ChatGPT needs to read or change files in a folder on your computer. For a codebase, the project folder becomes the working directory for Codex tasks.

If a repository contains more than one app or package, use distinct local projects when each task should access only one part of the repository. This keeps the working context focused. Use local environments to define setup actions and common commands for a project.

When you want to isolate code changes from your current checkout, select Codex and start the task in a worktree. Projects and worktrees organize work, but the sandbox enforces what local commands can read, change, or access over the network.

## Start a task without a project

Select **New task** when the work is self-contained and doesn't need shared project files, instructions, or folder access. Create a project first when several tasks will depend on the same context.

## Bring in other tools and context

- Reference open files or select code in the editor to add context for the current turn.
- Configure MCP servers when your organization or developer setup exposes tools through Model Context Protocol.
- Use memories from the connected Codex host, where available, to carry useful context into future tasks.

## Next steps

- Learn how to write and refine prompts
- Learn how to use ChatGPT
- Continue long-running work
