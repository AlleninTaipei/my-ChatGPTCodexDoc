# Use Codex in Slack

## Overview

Ask Codex to run tasks from channels and threads by mentioning `@Codex` with a prompt. Codex creates a cloud task and replies with results.

## Set up the Slack app

1. Set up Codex cloud tasks. You need a Plus, Pro, Business, Enterprise, or Edu plan, a connected GitHub account, and at least one environment.
2. Go to Codex settings and install the Slack app for your workspace. An admin may need to approve the install depending on workspace policies.
3. Add `@Codex` to a channel. Slack prompts you to add it when you mention it.

## Start a task

1. In a channel or thread, mention `@Codex` and include your prompt. Codex can reference earlier messages in the thread.
2. (Optional) Specify an environment or repository in your prompt (example: `@Codex fix the above in openai/codex`).
3. Wait for Codex to react and reply with a task link. When finished, Codex posts the result.

### How Codex chooses an environment and repo

- Codex reviews accessible environments and selects the best match for your request
- Falls back to your most recently used environment if ambiguous
- Runs against the default branch of the first repository in that environment's repo map
- Replies with instructions if no suitable environment or repository is available

### Enterprise data controls

Enterprise admins can disable answers in workspace settings to prevent Codex from posting environment information in Slack threads. When disabled, Codex replies only with a task link.

### Data usage, privacy, and security

Message and thread history are shared with Codex to understand requests and create tasks. Data handling follows OpenAI's policies on privacy and terms of use.

### Tips and troubleshooting

- Missing connections: Codex replies with reconnection links if unable to confirm Slack or GitHub connections
- Unexpected environment choice: Reply in thread with desired environment, then mention `@Codex` again
- Long threads: Summarize key details in your latest message for better context
- Workspace posting restrictions: Some Enterprise workspaces restrict answer posting; open the task link to view progress
