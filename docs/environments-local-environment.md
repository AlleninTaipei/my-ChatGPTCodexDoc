# Local environments

Configure common actions and setup scripts for worktrees

Local environments enable you to set up initialization steps for worktrees and define frequently-used actions within a project.

**Availability:** Local environments are exclusive to Codex in the ChatGPT desktop app. Access Codex before configuring or using a local environment.

You can manage your local environments via the ChatGPT desktop app settings pane. This configuration can be version-controlled in your project's Git repository for team sharing.

Codex maintains this setup in the `.codex` folder at your project root. For multi-project repositories, navigate to the project directory containing the shared `.codex` folder.

## Setup scripts

Worktrees operate in distinct directories from your local tasks, which may leave your project incomplete with missing dependencies or untracked files. "Setup scripts run automatically when Codex creates a new worktree at the start of a new task."

Execute commands necessary for environment configuration, including dependency installation or build processes.

**Example for TypeScript projects:**

```
npm install
npm run build
```

For platform-specific setups, create distinct scripts for macOS, Windows, or Linux to replace the default.

## Actions

Define frequent operations like launching your development server or executing tests. These appear in the ChatGPT desktop app toolbar for convenient access and run within the app's integrated terminal.

Actions eliminate repetitive typing for common tasks such as project builds or development server startup.

**Example for Node.js projects:**

```
npm start
```

For platform-specific commands, establish separate scripts for macOS, Windows, and Linux.

Each action requires an associated icon for identification.

## Use built-in Git tools

Codex provides standard Git operations alongside local projects and worktrees in the ChatGPT desktop app. The diff view displays current changes and enables inline comments for Codex to resolve. You can stage/revert chunks or files, commit, push branches, or open pull requests without leaving the app.

Use the integrated terminal for Git operations unavailable in the app interface. Start tasks in worktrees to separate concurrent modifications from your main checkout.
