# Long-running work

Keep multi-step work focused with clear outcomes and completion criteria

For work that may take many steps, give ChatGPT a clear outcome, constraints, and definition of done. Keep related work in the same task or conversation so ChatGPT can use the same context to choose the next step and decide when the work is complete.

In the ChatGPT desktop app, enter `/goal` to start Goal mode. The progress row lets you pause, resume, edit, or clear the goal while ChatGPT works.

For hosted long-running work in ChatGPT web, use ChatGPT Work and put the outcome, constraints, and review criteria directly in your prompt.

Continue in the same web conversation to add context, change constraints, or ask for a status update. Use separate chats when independent tasks can run in parallel, and avoid giving two tasks write access to the same connected source. For related work, keep the chats and source files together in a project.

In an interactive Codex CLI session, enter `/goal` to start Goal mode. Continue the same session to steer the work or ask for a status update.

In the IDE extension chat, enter `/goal` to start Goal mode for the open workspace. Continue the same chat to steer the task while it runs.

## Start a goal

Type `/goal` in the ChatGPT desktop app, Codex CLI, or the IDE extension. The goal text becomes both the first prompt and the completion criteria for the task.

If the outcome is still unclear, start with `/plan`. Ask ChatGPT to interview you, identify constraints, and turn the result into a goal with measurable success criteria. Then start the refined goal with `/goal`.

## Define what done means

Write a goal that lets ChatGPT verify its own progress. Include three things when they apply:

| Goal element | What to include |
|---|---|
| **Outcome** | Describe the result you want, not only the activity ChatGPT should perform. |
| **Constraints** | Name required tools, boundaries, compatibility needs, or approaches to avoid. |
| **Verification** | Add tests, measurements, or review criteria that prove the work is complete. |

For example:

```
Migrate this codebase from JavaScript to TypeScript. Preserve existing behavior,
compile in strict mode without explicit `any` types, and make the full test suite pass.
```

## Steer a running goal

In the ChatGPT desktop app, the goal progress row appears above the composer. Use it to pause or resume work, edit the goal, or clear it. You can also send follow-up messages while the goal runs to add context or adjust constraints.

Use a side conversation when you want a status recap or an explanation without interrupting the main task. Pause the goal before you expect to lose connectivity, then resume it when you're ready for ChatGPT to continue.

## Steer a running task

Continue in the same conversation to add context, adjust constraints, or ask for a status recap. Start a separate chat when another task can run independently.

## Run goals in parallel

Each task keeps its own context, messages, results, and goal. Run tasks concurrently, but avoid letting two tasks change the same files. Use worktrees to give parallel coding tasks separate checkouts.

For local work, turn on **Prevent sleep while running** in settings so your Mac stays awake. Use Pets or system notifications to see when a task needs input or is ready for review.

## Related docs

- Projects, chats, and tasks
- Goal mode and prompting
- Git worktrees
- Scheduled tasks
- Sandbox and permissions
