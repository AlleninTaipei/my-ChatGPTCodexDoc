# Troubleshooting

## Frequently Asked Questions

### Files appear in the side panel that Codex didn't edit

When your project exists within a Git repository, the review panel automatically displays changes based on Git state, including modifications Codex didn't make. You can toggle between staged and unstaged changes, and compare your branch against main. To view only your last Codex turn's changes, switch to the **Last turn** view.

### Remove a project from the sidebar

Hover over a project name, click the three dots menu, and select "Remove." Use the **Add new project** button or press Cmd+O to restore it.

### Find archived tasks

Access archived tasks through [Settings](codex://settings). Unarchiving restores tasks to their original sidebar locations.

### Only some tasks appear in the sidebar

Select the filter icon next to **Tasks** and choose **Chronological**. If tasks remain missing, check [Settings](codex://settings) for archived tasks.

### Code doesn't run on a worktree

Worktrees inherit Git-tracked files but may require setup scripts via [local environments](/codex/environments/local-environment) or copying ignored files using [`.worktreeinclude`](/codex/environments/git-worktrees). Alternatively, checkout changes in your regular project.

### App doesn't pick up a teammate's shared local environment

The local environment configuration must be in the `.codex` folder at your project root. For monorepos, open the specific project directory containing the `.codex` folder.

### Codex asks to access Apple Music

Codex may need file system navigation, and certain macOS directories (Music, Downloads, Desktop) require user approval. Approving home directory access resolves this.

### Scheduled tasks create many worktrees

Archive unneeded scheduled runs and avoid pinning runs unless keeping their worktrees. Frequent tasks accumulate worktrees over time.

### Recover a prompt after selecting the wrong target

If you accidentally chose the wrong target (**Local**, **Worktree**, or **Cloud**), cancel the current run and press the up arrow key in the composer to recover your prompt.

### Feature is working in the Codex CLI but not in the ChatGPT desktop app

Different surfaces may bundle different Codex versions, with experimental features sometimes reaching CLI first.

Check versions with:

```bash
codex --version
```

For the desktop app's bundled version:

```bash
/Applications/Codex.app/Contents/Resources/codex --version
```

## Feedback and logs

Type `/` in the message composer to provide feedback. Sharing your session generates a session ID for team reference.

To report issues:

1. Search [existing GitHub issues](https://github.com/openai/codex/issues)
2. [Create a new bug report](https://github.com/openai/codex/issues/new?template=2-bug-report.yml)

Log locations:

- App logs (macOS): `~/Library/Logs/com.openai.codex/YYYY/MM/DD`
- Session transcripts: `$CODEX_HOME/sessions` (default: `~/.codex/sessions`)
- Archived sessions: `$CODEX_HOME/archived_sessions` (default: `~/.codex/archived_sessions`)

Review logs for sensitive information before sharing.

## Stuck states and recovery patterns

If a task appears stuck:

1. Check for pending approvals
2. Run basic terminal commands like `git status`
3. Start a focused new task

Press the up arrow key in the composer to recover lost prompts from cancelled worktree creation.

## Terminal issues

**Terminal appears stuck**

1. Close the terminal panel
2. Reopen with Ctrl+\`
3. Run basic commands like `pwd` or `git status`

Validate your current directory and branch first. If issues persist, wait for active tasks to complete and restart the app.

**Fonts aren't rendering correctly**

Configure the font in [Settings](codex://settings) under **Code font**. Codex applies this font consistently across the review pane, terminal, and code displays.
