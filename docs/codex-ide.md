# Codex IDE Extension

## Build with the context already in your editor

Work with Codex beside your code. Bring open files and selections into the prompt, review edits in place, and hand off longer work without breaking your flow.

[Install the extension](https://marketplace.visualstudio.com/items?itemName=openai.chatgpt)

[Extension quickstart](#getting-started)

## Code Example

```typescript
import { wait } from './backoff';

export async function retryOperation<T>(
  fn: () => Promise<T>,
  retries = 3,
): Promise<T> {
  let attempt = 0;
  let lastError: unknown;

  while (attempt <= retries) {
    try {
      return await fn();
    } catch (error) {
      lastError = error;
      if (attempt >= retries) break;
      await wait(200 * 2 ** attempt);
      attempt++;
    }
  }
  throw lastError;
}
```

```typescript
import { retryOperation } from './retry';

describe('retryOperation', () => {
  it('stops after max retries', async () => {
    const operation = vi.fn().mockRejectedValue('nope');

    await expect(retryOperation(operation, 2))
      .rejects.toBe('nope');

    expect(operation).toHaveBeenCalledTimes(3);
  });
});
```

```typescript
import { setTimeout as wait } from 'timers/promises';

export function nextDelay(baseDelay: number, attempt: number) {
  const jitter = Math.random() * 0.2 + 0.9;
  return baseDelay * 2 ** attempt * jitter;
}

export { wait };
```

## Three Core Capabilities

### 01 Use the context already open

Reference open files, selected code, and recent threads directly from the composer. Codex starts with the code you are already looking at, so you spend less time restating the problem.

### 02 Review changes beside your code

Read the summary, inspect a focused diff, and follow up in the same thread. Keep only the changes you want while the source and rationale stay visible together.

### 03 Delegate when the task grows

Keep quick iterations local, or connect Codex web when a task needs more time and room. Return to a reviewable result from the same editor workflow.

## Quickstart: Get started in your IDE

Install or enable Codex, sign in, and start a task with the context already open in your editor.

### 1. Install or enable Codex

Choose your IDE. VS Code and compatible editors use the Codex extension; Xcode and JetBrains IDEs provide their own integrations.

- [Visual Studio Code](vscode:extension/openai.chatgpt)
- [Cursor](cursor:extension/openai.chatgpt)
- [Windsurf](windsurf:extension/openai.chatgpt)
- [Visual Studio Code Insiders](https://marketplace.visualstudio.com/items?itemName=openai.chatgpt)
- [Xcode](https://developer.apple.com/documentation/Xcode/setting-up-coding-intelligence)
- [JetBrains IDEs](https://www.jetbrains.com/help/ai-assistant/codex-agent.html)

### 2. Open Codex

VS Code, Cursor, or Windsurf: choose the Codex icon. If it is not visible, open the Command Palette and run **Codex: Open Codex Sidebar**.

Xcode: open the coding assistant, start a new conversation, and choose Codex as the agent.

JetBrains IDEs: open AI Chat and select Codex.

### 3. Start your first task

Open a project and ask Codex to explain the codebase, make a focused change, or help you debug an issue. Create Git checkpoints before and after a task so you can revert changes.

## Next steps

- [Prompt with editor context](/codex/prompting#use-editor-context)
- [Explore IDE commands](/codex/developer-commands?surface=ide)
- [Configure the extension](/codex/developer-settings?surface=ide)

## See what Codex can do in your IDE

Stay close to the code while Codex explains, edits, reviews, and delegates.

### 01 Use the context already open

Add an open file, a selection, or a recent thread to the composer, then ask Codex to explain or edit the code with that context already attached.

[Learn more](/codex/prompting#use-editor-context)

### 02 Review changes beside your code

Review a concise summary and the changed lines without an extra navigation pane. Inspect the two affected files, keep the edits you want, and ask for a follow-up from the same view.

[Learn more](/codex/prompting)

### 03 Delegate when the task gets bigger

Choose local work for fast, hands-on iteration, or connect Codex web to delegate a longer task. The thread stays available when you return to review the result.

[Learn more](/codex/cloud#delegate-from-the-ide-extension)

## Use Codex IDE extension when…

### You are making focused edits

Keep the relevant files and Codex in the same view.

### You are learning unfamiliar code

Ask about the files and symbols already open in the editor.

### You want to review changes in place

Inspect and apply edits alongside the source.

### You want to delegate a larger task

Start cloud work from the IDE and return to the result.

## Other ChatGPT and Codex surfaces

- [Desktop app](/codex/app) - Coordinate projects and long-running tasks on your desktop.
- [ChatGPT web](/codex/web) - Research, analyze, and create from your browser.
- [Codex CLI](/codex/cli) - Inspect, edit, and automate from the terminal.
- [Codex cloud](/codex/cloud) - Run coding tasks in parallel cloud environments.
