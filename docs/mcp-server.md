# Use Codex with the Agents SDK

## Running Codex as an MCP Server

You can run Codex as an MCP server and connect it from other MCP clients, such as agents built with the OpenAI Agents SDK MCP integration.

Launch Codex as an MCP server using:

```bash
codex mcp-server
```

You can also use the Model Context Protocol Inspector:

```bash
npx @modelcontextprotocol/inspector codex mcp-server
```

### Available Tools

The server exposes two tools:

1. `codex`: Runs a Codex session with a prompt and configuration overrides
   - `prompt` (required): Initial user prompt
   - `approval-policy`: Controls shell command approval (`untrusted`, `on-request`, `never`)
   - `sandbox`: Sandbox mode (`read-only`, `workspace-write`, `danger-full-access`)
   - Other optional parameters: `model`, `config`, `cwd`, `developer-instructions`, etc.

2. `codex-reply`: Continues a session using thread ID and new prompt
   - `prompt` (required): Next user prompt
   - `threadId` (required): Thread ID from previous response

## Creating Multi-Agent Workflows

By exposing Codex CLI as an MCP server and orchestrating it with the OpenAI Agents SDK, you can build deterministic, reviewable workflows that scale from a single agent to a complete software delivery pipeline.

### Prerequisites

- Codex CLI installed
- Python 3.10+
- Node.js 18+ (optional)
- OpenAI API key

### Implementation Overview

The guide demonstrates building from a simple single-agent workflow to a multi-agent system with a Project Manager coordinating Designer, Frontend Developer, Backend Developer, and Tester agents.

Key workflow features: sequential hand-offs with file gating, parallel task execution, and full execution traces for audit and debugging purposes.
