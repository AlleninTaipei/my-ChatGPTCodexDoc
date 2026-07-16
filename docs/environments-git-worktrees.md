# Worktrees

Worktrees enable Codex to "run multiple independent tasks in the same project without interfering with each other" in the ChatGPT desktop app, particularly useful for Git repositories where scheduled tasks operate on dedicated background worktrees.

## What's a worktree

Built on Git's worktree functionality, this feature creates a secondary repository copy while sharing Git metadata. Each maintains its own files but references the same commit history and branches.

## Key Terminology

- **Local checkout**: Your original repository
- **Worktree**: A Git worktree created from your local checkout within the app
- **Handoff**: The process moving tasks between Local and Worktree environments

## Primary Benefits

1. Work in parallel without disrupting your current Local setup
2. Queue background tasks while maintaining foreground focus
3. Transition tasks to Local for inspection or collaboration

## Getting Started

Tasks require a Git repository. The process involves selecting "Worktree," choosing a starting branch, submitting your prompt, then deciding whether to continue on the worktree or hand off to Local.

## Working Between Environments

Since Git restricts simultaneous branch checkouts across locations, Handoff manages safe transitions by handling required Git operations. Two common workflows exist: working exclusively on the worktree or handing tasks off to Local for IDE inspection or testing.

## Advanced Details

Codex maintains managed worktrees in `$CODEX_HOME/worktrees`, typically dedicated to single tasks and retained in detached HEAD states. A `.worktreeinclude` file can specify ignored files for copying into new worktrees, such as environment configuration files.

The system automatically retains 15 recent Codex-managed worktrees by default, with automatic deletion during archiving or when storage limits require cleanup.
