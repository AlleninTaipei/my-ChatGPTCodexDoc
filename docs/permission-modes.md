# Permissions

## Permission modes

Permissions control how ChatGPT (in the desktop app) and Codex (in the CLI or IDE) handle local actions, such as editing files, running commands, and using the internet. The mode you choose sets the boundary for what ChatGPT can do on its own and what needs review.

For most work, start with Ask for approval. It lets ChatGPT work within the current workspace and pauses before reaching beyond that boundary.

### Ask for approval

Codex can read and edit files in the current workspace and run routine local commands. It asks before using the internet or going beyond the workspace boundary.

- Sandbox: workspace-write
- Approvals policy: on-request
- Reviewer: user

## Enable modes

When you're using the ChatGPT desktop app for the first time, you need to enable modes in application settings.

Ask for approval is always available. To add Approve for me (called Auto review in settings) or Full access to the permissions menu, open Settings > General in the ChatGPT desktop app.

Enabling a mode makes it available in the menu; it doesn't select the mode or change an existing conversation.

### Default permissions

- By default, ChatGPT can read and edit files in its workspace. It can ask for additional access when needed.
- Auto review: ChatGPT can read and edit files in its workspace. ChatGPT automatically reviews requests for additional access. Auto review can make mistakes.
- Full access: When ChatGPT runs with full access, it can edit any file on your computer and run commands with network, without your approval.

The available modes can depend on your local configuration and your organization's requirements.

## How permissions work

Two controls work together:

- The sandbox defines which files and network resources ChatGPT can access.
- Approvals determine when ChatGPT pauses before an action or sends the request to automatic review.

Changing who reviews a request doesn't expand the sandbox. For example, Approve for me keeps the same workspace boundary as Ask for approval; it sends requests to cross that boundary to automatic review.

Use the permissions control below the composer in the ChatGPT desktop app or IDE extension.

### How should ChatGPT actions be approved?

- Ask for approval: Always ask to edit external files and use the internet
- Approve for me: Only ask for actions detected as potentially unsafe
- Full access: Disabled by requirements.toml
- Custom (config.toml): Uses permissions defined in config.toml

In the CLI, enter `/permissions`. For technical details, see Sandbox, automatic review, or permission profiles.
