# Pricing

ChatGPT Work and Codex are included in your ChatGPT Free, Go, Plus, Pro, Business, Edu, or Enterprise plan.

ChatGPT Work and Codex share usage. Work usage inside ChatGPT uses the same pricing, credits, and usage limits as Codex.

## Pricing options

### Free

Explore Codex capabilities on quick coding tasks.

$0/month

### Go

Use Codex for lightweight coding tasks.

$8/month

### Plus

Power a few focused coding sessions each week.

$20/month

- Codex on the web, in the CLI, in the IDE extension, and on iOS
- Cloud based integrations like automatic code review and Slack integration
- The GPT-5.6 model family, including Sol, Terra, and Luna
- GPT-5.6 Luna for higher usage limits on lighter weight or high volume workloads
- Flexibly extend usage with ChatGPT credits
- Other ChatGPT features as part of the Plus plan

### Pro

Choose 5x or 20x higher rate limits than Plus.

From $100/month

Everything in Plus and:

- Access to GPT-5.3-Codex-Spark (research preview), a fast Codex model for day to day coding tasks
- 5x or 20x more Codex usage than Plus
- Other ChatGPT features as part of the Pro plan

### API Key

Great for automation in shared environments like CI.

- Codex in the CLI, SDK, or IDE extension
- No cloud based features (GitHub code review, Slack, etc.)
- Model availability follows the API models available to your key
- Pay only for the tokens Codex uses, based on API pricing

### Business

Bring Codex into your startup or growing business.

$20/user/month (2+ users, billed annually. $25 per user per month when billed monthly.)

- Access ChatGPT and Codex across desktop and mobile apps
- Larger virtual machines to run cloud tasks faster
- Flexibly extend usage with ChatGPT credits
- A secure, dedicated workspace with essential admin controls, SAML SSO, and MFA
- No training on your business data by default
- Other ChatGPT features as part of the Business plan

### Enterprise & Edu

Unlock Codex for your entire organization with enterprise grade functionality.

Everything in Business and:

- Priority request processing
- Enterprise level security and controls, including SCIM, EKM, user analytics, domain verification, and role based access control (RBAC)
- Audit logs and usage monitoring via the Compliance API
- Data retention and data residency controls
- Other ChatGPT features as part of the Enterprise plan

## Invite friends and coworkers

Eligible users can send Codex invitations from the profile menu. Choose "Invite a friend" on an eligible personal plan or "Invite a coworker" in an eligible Business workspace.

From June 11 through June 24, 2026, eligible Plus and Pro users can invite up to three friends. When an eligible recipient sends their first Codex message, both people receive a banked rate limit reset usable for 30 days.

## Frequently asked questions

### How much does Sites cost?

Sites is included with eligible ChatGPT plans during public beta.

### What are the usage limits for my plan?

The number of Codex messages you can send depends on the model used, size and complexity of your coding tasks and whether you run them locally or in the cloud.

Choose the GPT-5.6 model that best fits your work:

- Sol: Best for quality and reasoning depth in complex analysis and advanced workflows
- Terra: The everyday default with strong capability and balanced performance/price
- Luna: Optimized for speed and affordability on lighter weight or high volume workloads

#### Usage Limits by Plan

| Model | Plus | Pro 5x | Pro 20x | Business | API Key |
|-------|------|--------|---------|----------|---------|
| GPT-5.6 Sol | 15-90 | 75-450 | 300-1800 | 15-90 | Usage-based |
| GPT-5.6 Terra | 20-110 | 100-550 | 400-2200 | 20-110 | Usage-based |
| GPT-5.6 Luna | 50-280 | 250-1400 | 1000-5600 | 50-280 | Usage-based |
| GPT-5.5 | 15-80 | 75-400 | 300-1600 | 15-80 | Usage-based |
| GPT-5.4 | 20-100 | 100-500 | 400-2000 | 20-100 | Usage-based |
| GPT-5.4 mini | 60-350 | 300-1750 | 1200-7000 | 60-350 | Usage-based |

本地訊息與雲端任務共用同一個五小時區間, 可能另有每週上限.

### What happens when you hit usage limits?

如果在一個進行中的回合期間達到使用上限, 代理仍可繼續完成該回合的工作, 但需受公平使用限制約束.

Users can purchase additional credits to continue working. Business, Edu, and Enterprise plans with flexible pricing can purchase additional workspace credits.

### How does image generation count toward usage limits?

Image generation counts toward the same general Codex usage limits as local messages and cloud tasks. 影像產生根據影像品質與大小不同, 平均會比同類型且不含影像產生的回合快 3 到 5 倍消耗掉包含的額度上限.

### Where can I see my current usage limits?

Find your current limits in the Codex usage dashboard. If you want to see your remaining limits during an active Codex CLI session, use `/status`.

### What are tokens and credits?

Tokens are small units of information that ChatGPT reads and writes. Your prompt, files, conversation history, tool results, and ChatGPT's response all use tokens.

Credits translate token usage into a simpler unit for tracking and managing consumption.

#### Credit Rates per 1M Tokens

| Model | Input Tokens | Cached Input Tokens | Output Tokens |
|-------|--------------|---------------------|---------------|
| GPT-5.6 Sol | 125 credits | 12.5 credits | 750 credits |
| GPT-5.6 Terra | 62.5 credits | 6.25 credits | 375 credits |
| GPT-5.6 Luna | 25 credits | 2.5 credits | 150 credits |
| GPT-5.5 | 125 credits | 12.50 credits | 750 credits |
| GPT-5.4 | 62.50 credits | 6.250 credits | 375 credits |
| GPT-5.4 mini | 18.75 credits | 1.875 credits | 113 credits |
| GPT-Image-2 (image) | 200 credits | 50 credits | 750 credits |
| GPT-Image-2 (text) | 125 credits | 31.25 credits | 250 credits |

GPT-5.6 平均每則訊息消耗約 5 到 40 個額度.

### What counts as Code Review usage?

Code Review 的用量僅適用於 Codex 透過 GitHub 執行審查時, 例如在 pull request 中標記 @Codex 進行審查, 或在您的儲存庫啟用自動審查時.

### What can I do to make my usage limits last longer?

- Control the size of your prompts with precise instructions
- Limit source material to only relevant files
- Match the output to the need
- Reduce the size of your AGENTS.md
- Limit the number of MCP servers you use
- Switch to a smaller model for routine tasks

## Feature availability

| Feature | Plus | Pro | Business | Enterprise | API Key |
|---------|------|-----|----------|-----------|---------|
| Codex cloud | v | v | v | v | - |
| ChatGPT Work on Web | v | v | v | v | - |
| ChatGPT desktop app | v | v | v | v | - |
| Codex CLI | v | v | v | v | v |
| IDE extension | v | v | v | v | v |
| Codex SDK and scripting | v | v | v | v | v |
| Automation access tokens | - | - | v | v | v |
| ChatGPT for Excel | v | v | v | v | - |
| GPT-5.6 | v | v | v | v | v |
| Fast mode | - | v | - | v | v |
| Codex-Spark research preview | - | v | - | - | - |
| Image generation | v | v | v | v | - |
| Voice dictation | v | v | v | v | - |
| Web search | v | v | v | v | - |
| Local code review | v | v | v | v | v |
| Auto-review | - | v | v | v | - |
| Sandboxing | - | v | v | v | - |
| Scheduled tasks | v | v | v | v | - |
| Built-in Git tools | - | v | v | v | - |
| Repeatable actions | - | v | v | v | - |
| Appshots | - | v | v | v | - |
| Browser features | 有限* | 有限* | 有限* | 有限* | 有限* |
| Computer Use | 有限* | 有限* | 有限* | 有限* | 有限* |
| Record & Replay | 有限* | 有限* | 有限* | 有限* | 有限* |
| SSH remote | - | v | v | v | - |
| Mobile remote | - | v | v | v | - |
| Chrome browser | 有限* | 有限* | 有限* | 有限* | 有限* |
| Custom instructions | v | v | v | v | v |
| Skills | v | v | v | v | - |
| Plugins | v(dagger) | v | v | v | - |
| Plugin sharing | - | v | v | v | - |
| Connectors | - | v | v | v | - |
| MCP | v | v | v | v | v |
| Subagents | - | v | v | v | - |
| Memories | 有限* | 有限* | 有限* | 有限* | 有限* |
| Chronicle | - | v | v | v | - |
| Cloud tasks | - | v | v | v | - |
| Cloud environments | - | v | v | v | - |
| Internet controls | - | v | v | v | - |
| Sites | - | v | v | v | - |
| GitHub delegation | - | v | v | v | - |
| GitHub PR reviews | - | v | v | v | - |
| Slack integration | - | v | v | v | - |
| Linear integration | - | v | v | v | - |
| Workspace management | - | - | v | v | - |
| requirements.toml config | - | - | v | v | - |
| Cloud-managed policies | - | - | v | v | - |
| RBAC and roles | - | - | v | v | - |
| SCIM, EKM, domains | - | - | - | v | - |
| Retention and residency | - | - | - | v | - |
| No default training | - | - | v | v | v |
| Analytics dashboard | - | - | v | v | - |
| Analytics API | - | - | - | v | - |
| Compliance and audit logs | - | - | - | v | - |
| Codex Security | - | - | - | v | - |

*標記表示該功能目前僅限於特定地區.
(dagger) 標記表示部分第一方 plugins 不提供.
