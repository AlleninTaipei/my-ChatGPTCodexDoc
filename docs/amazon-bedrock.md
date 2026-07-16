# Use Codex with Amazon Bedrock

Configure Codex to use OpenAI models available through Amazon Bedrock

Codex can run locally and send model requests to Bedrock using AWS-managed authentication and access controls.

## How it works

When configured with Amazon Bedrock as the model provider, Codex sends requests directly to Amazon Bedrock rather than through OpenAI-hosted services. Bedrock provides an OpenAI-compatible Responses API implementation for supported OpenAI models. Users authenticate with Bedrock API keys or AWS IAM credentials instead of ChatGPT sign-in.

## Before you start

Required prerequisites:

- Access to supported OpenAI models in Amazon Bedrock
- An AWS Region where the selected model is available
- Authentication for the Amazon Bedrock Mantle path configured for the AWS account

## Configure Codex

Add the model provider to `~/.codex/config.toml`:

```toml
model_provider = "amazon-bedrock"
```

Note: This covers the Amazon Bedrock Mantle path in commercial AWS Regions only; GovCloud Regions are not supported.

## Authentication options

Codex checks authentication methods in this order:

### Option 1: Bedrock API key

Requires specifying a Region:

```bash
export AWS_BEARER_TOKEN_BEDROCK=<your-bedrock-api-key>
export AWS_REGION=us-east-2
```

### Option 2: AWS SDK credentials

Use standard AWS SDK credential sources:

1. Shared AWS config and credentials files via `aws configure`
2. Environment variables (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN)
3. AWS Management Console credentials via `aws login`
4. AWS SSO or named profiles
5. Federated identity with `credential_process`

## Desktop app and VS Code extension

Since these may not inherit shell environment variables, place required values in `~/.codex/.env`:

```bash
export AWS_BEARER_TOKEN_BEDROCK=<your-bedrock-api-key>
export AWS_REGION=us-east-2
```

Then restart the application.

## Verify setup

- Check `/status` in Codex CLI to confirm `amazon-bedrock` provider is active
- Verify the model is available in your configured AWS Region
- Confirm the AWS identity has permission to access the selected model

## Supported models

Exact model IDs:

- `openai.gpt-5.5`
- `openai.gpt-5.4`

Model availability varies by region; consult AWS documentation for regional compatibility.

## Feature availability

This configuration supports local Codex workflows. Cloud-dependent features including Fast Mode (which requires priority processing) are unavailable. The detailed compatibility table indicates:

Available: Local code review, auto-review, sandboxing, scheduled tasks, Git tools, local environments, appshots, some browser/computer use features (with regional limits), custom instructions, skills, local plugins, MCP, and subagents.

Unavailable: Codex cloud, ChatGPT desktop for cloud tasks, image generation, voice dictation, web search, plugin sharing, connectors, memories, chronicle, sites, GitHub integration, Slack/Linear integration, enterprise SSO features, analytics, and Codex Security.

## Troubleshooting

Verify:

- Model ID exactly matches a supported model
- AWS Region supports the selected model
- Bedrock API key or AWS credentials are valid and not expired
- AWS identity has necessary permissions
- No expired tokens in `AWS_BEARER_TOKEN_BEDROCK`
- Desktop app/VS Code users have environment variables in `~/.codex/.env`

## Support boundaries

OpenAI Support assists with Codex client setup and local behavior. For AWS credentials, IAM permissions, Bedrock access, billing, or service issues, contact your AWS administrator or AWS Support.
