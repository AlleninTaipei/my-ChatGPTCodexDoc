# Use Codex in Linear

Run Codex tasks from Linear issues

Codex in Linear allows you to delegate work from issues. Assign an issue to Codex or mention `@Codex` in a comment, and Codex creates a cloud task and replies with progress and results.

Codex in Linear is available on paid plans. Enterprise plan users should contact their ChatGPT workspace admin to enable this feature in workspace and connector settings.

## Set up the Linear integration

1. Configure Codex cloud tasks by connecting GitHub in Codex and creating an environment for your repository
2. Go to Codex settings and install Codex for Linear for your workspace
3. Link your Linear account by mentioning `@Codex` in a Linear issue comment thread

## Delegate work to Codex

### Assign an issue to Codex

Assign issues to Codex like you would to teammates. Codex starts work and posts updates back to the issue.

### Mention `@Codex` in comments

Mention `@Codex` in comment threads to delegate tasks or ask questions. You can specify a repository by including it in your comment, like: `@Codex fix this in openai/codex`

Track progress by opening the Activity section or the task link for detailed updates.

### How Codex chooses an environment and repo

- Linear suggests repositories based on issue context
- Codex selects the matching environment, defaulting to your most recently used one if ambiguous
- Tasks run against the default branch of the first repository in the environment's repo map

## Automatically assign issues to Codex

Create triage rules in Linear settings to automatically assign issues to Codex, which then runs tasks using the issue creator's account.

## Data usage, privacy, and security

Issue content is shared with Codex to understand requests. Data handling follows OpenAI's Privacy Policy, Terms of Use, and other applicable policies.

## Tips and troubleshooting

- Check for missing connection links in issue replies
- Specify environment preferences in follow-up comments
- Add context to issues for accurate code targeting
- Consult the OpenAI Help Center for additional support

## Connect Linear for local tasks (MCP)

For ChatGPT desktop app, Codex CLI, or IDE extension users:

### Use the CLI (recommended)

```
codex mcp add linear --url https://mcp.linear.app/mcp
```

### Configure manually

Add to `~/.codex/config.toml`:

```
[mcp_servers.linear]
url = "https://mcp.linear.app/mcp"
```

Then run: `codex mcp login linear`
