# Web search

Search the web while you work in ChatGPT

## Overview

ChatGPT includes a first-party web search tool. All web results should be treated as untrusted input.

## Using web search across platforms

ChatGPT desktop app: Request current information within a task, and ChatGPT will record search activity alongside other tool calls in the transcript.

ChatGPT web: Ask for current information or sources. Search results and citations display in the conversation when ChatGPT performs web searches. Workspace settings may restrict search availability.

CLI: Use the `--search` flag to fetch live results for a single execution:

```
codex --search "Summarize the latest release notes for this dependency"
```

Searches appear as `web_search` items in the interactive transcript and `codex exec --json` output.

IDE extension: Request web searches while working in the editor. The extension uses the connected Codex host's search configuration, with search activity appearing in the conversation transcript.

## Configuring local web search

Local Codex tasks enable cached search by default, using an OpenAI-maintained index rather than fetching arbitrary pages live. This approach reduces, though doesn't eliminate, prompt injection risks.

For tasks requiring the most recent information, set `web_search = "live"` in `config.toml`. Disable the tool entirely with `web_search = "disabled"`. The `"indexed"` mode restricts external web access to search index-gated requests. When Codex runs with full access, web search defaults to live results.

For network boundaries in Codex cloud environments, see the Internet access documentation.
