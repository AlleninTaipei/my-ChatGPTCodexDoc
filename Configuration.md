# Configuration

## Customization

How to customize Codex with project guidance, skills, MCP, and subagents

Customization is how you make Codex work the way your team works.

In Codex, customization comes from a few layers that work together:

- **Project guidance (`AGENTS.md`)** for persistent instructions
- **[Memories](/codex/customization/memories)** for useful context learned from prior work
- **Skills** for reusable workflows and domain expertise
- **[MCP](/codex/extend/mcp)** for access to external tools and shared systems
- **[Subagents](/codex/agent-configuration/subagents)** for delegating work to specialized subagents

These are complementary, not competing. `AGENTS.md` shapes behavior, memories
carry local context forward, skills package repeatable processes, and
[MCP](/codex/extend/mcp) connects Codex to systems outside the local workspace.

### AGENTS Guidance

`AGENTS.md` gives Codex durable project guidance that travels with your repository and applies before the agent starts work. Keep it small.

Use it for the rules you want Codex to follow every time in a repo, such as:

- Build and test commands
- Review expectations
- repo-specific conventions
- Directory-specific instructions

When the agent makes incorrect assumptions about your codebase, correct them in `AGENTS.md` and ask the agent to update `AGENTS.md` so the fix persists. Treat it as a feedback loop.

**Updating `AGENTS.md`:** Start with only the instructions that matter. Codify recurring review feedback, put guidance in the closest directory where it applies, and tell the agent to update `AGENTS.md` when you correct something so future sessions inherit the fix.

#### When to update `AGENTS.md`

- **Repeated mistakes**: If the agent makes the same mistake repeatedly, add a rule.
- **Too much reading**: If it finds the right files but reads too many documents, add routing guidance (which directories/files to prioritize).
- **Recurring PR feedback**: If you leave the same feedback more than once, codify it.
- **In GitHub**: In a pull request comment, tag `@codex` with a request (for example, `@codex add this to AGENTS.md`) to delegate the update to a cloud task.
- **Automate drift checks**: Use [scheduled tasks](/codex/automations) to run recurring checks (for example, daily) that look for guidance gaps and suggest what to add to `AGENTS.md`.

Pair `AGENTS.md` with infrastructure that enforces those rules: pre-commit hooks, linters, and type checkers catch issues before you see them, so the system gets smarter about preventing recurring mistakes.

Codex can load guidance from multiple locations: a global file in your Codex home directory (for you as a developer) and repo-specific files that teams can check in. Files closer to the working directory take precedence.
Use the global file to shape how Codex communicates with you (for example, review style, verbosity, and defaults), and keep repo files focused on team and codebase rules.

- ~/.codex/ AGENTS.md Global (for you as a developer)
- repo-root/ AGENTS.md repo-specific (for your team)

[Custom instructions with AGENTS.md](/codex/agent-configuration/agents-md)

### Skills

Skills give Codex reusable capabilities for repeatable workflows.
Skills are often the best fit for reusable workflows because they support richer instructions, scripts, and references while staying reusable across tasks.
Skills are loaded and visible to the agent (at least their metadata), so Codex can discover and choose them implicitly. This keeps rich workflows available without bloating context up front.

Use skill folders to author and iterate on workflows locally. If a plugin
already exists for the workflow, install it first to reuse a proven setup. When
you want to distribute your own workflow across teams or bundle it with
connectors, package it as a [plugin](/codex/build-plugins). Skills remain the
authoring format; plugins are the installable distribution unit.

A skill is typically a `SKILL.md` file plus optional scripts, references, and assets.

- my-skill/ SKILL.md Required: instructions + metadatascripts/ Optional: executable codereferences/ Optional: documentationassets/ Optional: templates, resources

The skill directory can include a `scripts/` folder with CLI scripts that Codex invokes as part of the workflow (for example, seed data or run validations). When the workflow needs external systems (issue trackers, design tools, docs servers), pair the skill with [MCP](/codex/extend/mcp).

Example `SKILL.md`:

```md
---
name: commit
description: Stage and commit changes in semantic groups. Use when the user wants to commit, organize commits, or clean up a branch before pushing.
---

1. Do not run `git add .`. Stage files in logical groups by purpose.
2. Group into separate commits: feat → test → docs → refactor → chore.
3. Write concise commit messages that match the change scope.
4. Keep each commit focused and reviewable.
```

Use skills for:

- Repeatable workflows (release steps, review routines, docs updates)
- Team-specific expertise
- Procedures that need examples, references, or helper scripts

Skills can be global (in your user directory, for you as a developer) or repo-specific (checked into `.agents/skills`, for your team). Put repo skills in `.agents/skills` when the workflow applies to that project; use your user directory for skills you want across all repos.

| Layer | Global | Repo |
| --- | --- | --- |
| AGENTS | `~/.codex/AGENTS.md` | `AGENTS.md` in repo root or nested directories |
| Skills | `$HOME/.agents/skills` | `.agents/skills` in repo |

Codex uses progressive disclosure for skills:

- It starts with metadata (`name`, `description`) for discovery
- It loads `SKILL.md` only when a skill is chosen
- It reads references or runs scripts only when needed

Skills can be invoked explicitly, and Codex can also choose them implicitly when the task matches the skill description. Clear skill descriptions improve triggering reliability.

[Build skills](/codex/build-skills)

### MCP

MCP (Model Context Protocol) is the standard way to connect Codex to external tools and context providers.
It’s especially useful for remotely hosted systems such as Figma, Linear, GitHub, or internal knowledge services your team depends on.

Use MCP when Codex needs capabilities that live outside the local repo, such as issue trackers, design tools, browsers, or shared documentation systems.

One way to think about it:

- **Host**: Codex
- **Client**: the MCP connection inside Codex
- **Server**: the external tool or context provider

MCP servers can expose:

- **Tools** (actions)
- **Resources** (readable data)
- **Prompts** (reusable prompt templates)

This separation helps you reason about trust and capability boundaries. Some servers mainly provide context, while others expose powerful actions.

In practice, MCP is often most useful when paired with skills:

- A skill defines the workflow and names the MCP tools to use

[Model Context Protocol](/codex/extend/mcp)

### Subagents

You can create different agents with different roles and prompt them to use tools differently. For example, one agent might run specific testing commands and configurations, while another has MCP servers that fetch production logs for debugging. Each subagent stays focused and uses the right tools for its job.

[Subagents](/codex/agent-configuration/subagents)

### Skills + MCP together

Skills plus MCP is where it all comes together: skills define repeatable workflows, and MCP connects them to external tools and systems.
If a skill depends on MCP, declare that dependency in `agents/openai.yaml` so Codex can install and wire it automatically (see [Build skills](/codex/build-skills)).

### Next step

Build in this order:

1. [Custom instructions with AGENTS.md](/codex/agent-configuration/agents-md) so Codex follows your repo conventions. Add pre-commit hooks and linters to enforce those rules.
2. Install a [plugin](/codex/plugins) when a reusable workflow already exists. Otherwise, create a [skill](/codex/build-skills) and package it as a plugin when you want to share it.
3. [MCP](/codex/extend/mcp) when workflows need external systems (Linear, GitHub, docs servers, design tools).
4. [Subagents](/codex/agent-configuration/subagents) when you’re ready to delegate noisy or specialized tasks to subagents.

---

## Memories

How ChatGPT and Codex carry useful context forward across chats and tasks

Memories let ChatGPT and Codex carry useful context from earlier work into
future work.
ChatGPT web uses ChatGPT memory, while local Codex clients use a separate local
memory store and controls.

Keep required team guidance in `AGENTS.md` or checked-in documentation. Treat
memories as a helpful recall layer, not as the only source for rules that must
always apply.

In the ChatGPT desktop app, use `/memories` to choose whether a task can use
local memories or contribute to future memories. Manage the feature from
**Settings > Personalization** when you need to turn it on or off.

Manage ChatGPT memory from **Settings > Personalization**. ChatGPT Work uses
the memory settings available to your account and workspace; it doesn’t use a
local Codex memory store or local memory controls.

In Codex CLI, use `/memories` in an interactive session to control whether the
current task can use existing local memories or become an input for future
memories. See [Configure local memories](#configure-local-memories) if the
command isn’t available.

The IDE extension uses the connected Codex host’s local memory store. When
memories are enabled for that host, use the same task-level controls as Codex
CLI.

[Chronicle](/codex/customization/chronicle) is a desktop-only feature that helps
Codex recover recent working context from your screen to build up memory.

### How local Codex memories work

After you enable memories, Codex can turn useful context from eligible prior
tasks into local memory files. Codex skips active or short-lived sessions,
redacts secrets from generated memory fields, and updates memories in the
background instead of immediately at the end of every task.

Memories may not update right away when a task ends. Codex waits until a
task has been idle long enough to avoid summarizing work that’s still in
progress.

Memory generation can also skip a background pass when your Codex rate-limit
remaining percentage is below the configured threshold, so Codex doesn’t spend
quota when you’re near a limit.

### Local memory storage

Codex stores memories under your Codex home directory. By default, that’s
`~/.codex`. See [Config and state locations](/codex/config-file/config-advanced#config-and-state-locations)
for how Codex uses `CODEX_HOME`.

The main memory files live under `~/.codex/memories/` and include summaries,
durable entries, recent inputs, and supporting evidence from prior tasks.

Treat these files as generated state. You can inspect them when troubleshooting
or before sharing your Codex home directory, but don’t rely on editing them by
hand as your primary control surface.

### Control local memories per task

In the ChatGPT desktop app and Codex TUI, use `/memories` to control memory behavior for
the current task. Task-level choices let you decide whether the current
task can use existing memories and whether Codex can use the task to
generate future memories.

Task-level choices don’t change your global memory settings.

### Review local memories

Don’t store secrets in memories. Codex redacts secrets from generated memory
fields, but you should still review memory files before sharing your Codex home
directory or generated memory artifacts.

### Configure local memories

Local Codex memories are off by default. In the ChatGPT desktop app, open
**Settings > Personalization** and turn on **Enable memories**.

For config-based setup, add the feature flag to `config.toml`:

```toml
[features]
memories = true
```

For config file locations and the full list of memory-related settings, see
[Config basics](/codex/config-file/config-basic) and the [configuration
reference](/codex/config-file/config-reference).

Common memory-specific settings include:

- `memories.generate_memories`: controls whether newly created tasks can be
stored as memory-generation inputs.
- `memories.use_memories`: controls whether Codex injects existing memories into
future sessions.
- `memories.disable_on_external_context`: when `true`, keeps tasks that used
external context such as MCP tool calls, web search, or tool search out of
memory generation. The older `memories.no_memories_if_mcp_or_web_search` key
is still accepted as an alias.
- `memories.min_rate_limit_remaining_percent`: controls the minimum remaining
Codex rate-limit percentage required before memory generation starts.
- `memories.extract_model`: overrides the model used for per-task memory
extraction.
- `memories.consolidation_model`: overrides the model used for global memory
consolidation.

---

## Chronicle

Build Codex memories from recent screen context.

Chronicle is in an **opt-in research preview**. It is only available for
ChatGPT Pro subscribers on macOS. Please review the [Privacy and
Security](#privacy-and-security) section for details and to understand the
current risks before enabling.

Chronicle augments Codex memories with context from your screen. When you prompt
Codex, those memories can help it understand what you’ve been working on with
less need for you to restate context.

Chronicle is available as an opt-in research preview in the ChatGPT desktop app on macOS.
It requires macOS Screen Recording and Accessibility permissions. Before
enabling, be aware that Chronicle uses rate limits quickly, increases risk of
prompt injection, and stores memories unencrypted on your device.

### How Chronicle helps

We’ve designed Chronicle to reduce the amount of context you have to restate
when you work with Codex. By using recent screen context to improve memory
building, Chronicle can help Codex understand what you’re referring to, identify
the right source to use, and pick up on the tools and workflows you rely on.

#### Use what’s on screen

With Chronicle Codex can understand what you are currently looking at, saving
you time and context switching.

#### Fill in missing context

No need to carefully craft your context and start from zero. Chronicle lets
Codex fill in the gaps in your context.

#### Remember tools and workflows

No need to explain to Codex which tools to use to perform your work. Codex
learns as you work to save you time in the long run.

In these cases, Codex uses Chronicle to provide additional context. When another
source is better for the job, such as reading the specific file, Slack thread,
Google Doc, dashboard, or pull request, Codex uses Chronicle to identify the
source and then use that source directly.

### Enable Chronicle

1. Open Settings in the ChatGPT desktop app.
2. Go to **Personalization** and make sure **Memories** is enabled.
3. Turn on **Chronicle** below the Memories setting.
4. Review the consent dialog and choose **Continue**.
5. Grant macOS Screen Recording and Accessibility permissions when prompted.
6. When setup completes, choose **Try it out** or start a new task.

If macOS reports that Screen Recording or Accessibility permission is denied,
open System Settings > Privacy & Security > Screen Recording or
Accessibility and enable ChatGPT. If a permission is restricted by macOS or
your organization, Chronicle will start after the restriction is removed and
ChatGPT receives the required permission.

### Pause or disable Chronicle at any time

You control when Chronicle generates memories using screen context. Use the
ChatGPT menu bar icon to choose **Pause Chronicle** or **Resume Chronicle**. Pause
Chronicle before meetings or when viewing sensitive content that you do not want
Codex to use as context. To disable Chronicle, return to **Settings >
Personalization > Memories** and turn off **Chronicle**.

You can also control whether memories are used in a given task. [Learn
more](/codex/customization/memories#control-memories-per-task).

### Rate limits

Chronicle works by running sandboxed agents in the background to generate
memories from captured screen images. These agents currently consume rate limits
quickly.

### Privacy and security

Chronicle uses screen captures, which can include sensitive information visible
on your screen. It does not have access to your microphone or system audio.
Don’t use Chronicle to record meetings or communications with others without
their consent. Pause Chronicle when viewing content you do not want remembered
in memories.

#### Where does Chronicle store my data?

Screen captures are ephemeral and will only be saved temporarily on your
computer. Temporary screen capture files may appear under
`$TMPDIR/chronicle/screen_recording/` while Chronicle is running. Screen captures
that are older than 6 hours will be deleted while Chronicle is running.

The memories that Chronicle generates are just like other Codex memories:
unencrypted markdown files that you can read and modify if needed. You can also
ask Codex to search them. If you want to have Codex forget something you can
delete the respective file inside the folder or selectively edit the markdown
files to remove the information you’d like to remove. You should not manually
add new information. The generated Chronicle memories are stored locally on your
computer under `$CODEX_HOME/memories_extensions/chronicle/` (typically
`~/.codex/memories_extensions/chronicle`).

#### What data gets shared with OpenAI?

Chronicle captures screen context locally, then periodically uses Codex to
summarize recent activity into memories. To generate those memories, Chronicle
starts an ephemeral Codex session with access to this screen context. That
session may process selected screenshot frames, OCR text extracted from
screenshots, timing information, and local file paths for the relevant time
window.

Screen captures used for memory generation are stored temporarily on your device. They are processed on our
servers to generate memories, which are then stored locally on device. We do not
store the screenshots on our servers after processing unless required by law,
and do not use them for training.

The generated memories are Markdown files stored locally under
`$CODEX_HOME/memories_extensions/chronicle/`. When Codex uses memories in a
future session, relevant memory contents may be included as context for that
session, and may be used to improve our models if allowed in your ChatGPT
settings. [Learn more](https://help.openai.com/en/articles/7730893-data-controls-faq).

### Prompt injection risk

Using Chronicle increases risk to prompt injection attacks from screen content.
For instance, if you browse a site with malicious agent instructions, Codex may
follow those instructions.

### Troubleshooting

#### How do I enable Chronicle?

If you do not see the Chronicle setting, make sure you are using a ChatGPT desktop app
build that includes Chronicle and that you have Memories enabled inside Settings
> Personalization.

Chronicle is currently only available for ChatGPT Pro subscribers on macOS.

If setup does not complete:

1. Confirm that ChatGPT has Screen Recording and Accessibility permissions.
2. Quit and reopen the ChatGPT desktop app.
3. Open **Settings > Personalization** and check the Chronicle status.

#### Which model is used for generating the Chronicle memories?

Chronicle uses the same model as your other [Memories](/codex/customization/memories). If you
did not configure a specific model it uses your default Codex model. To choose a
specific model, update the `consolidation_model` in your
[configuration](/codex/config-file/config-basic).

```toml
[memories]
consolidation_model = "gpt-5.4-mini"
```

---

## Config basics

Learn the basics of configuring your local Codex client

Codex reads configuration details from more than one location. Your personal defaults live in `~/.codex/config.toml`, and you can add project overrides with `.codex/config.toml` files. For security, Codex loads project `.codex/` layers only when you trust the project.

### Codex configuration file

Codex stores user-level configuration at `~/.codex/config.toml`. To scope settings to a specific project or subfolder, add a `.codex/config.toml` file in your repo.

To open the configuration file from the Codex IDE extension, select the gear icon in the top-right corner, then select **Codex Settings > Open config.toml**.

The CLI and IDE extension share the same configuration layers. You can use them to:

- Set the default model and provider.
- Configure [approval policies and sandbox settings](/codex/agent-approvals-security#sandbox-and-approvals).
- Configure [MCP servers](/codex/extend/mcp).

### Configuration precedence

Codex resolves values in this order (highest precedence first):

1. CLI flags and `--config` overrides
2. Project config files: `.codex/config.toml`, ordered from the project root down to your current working directory (closest wins; trusted projects only)
3. [Profile](/codex/config-file/config-advanced#profiles) files selected with `--profile profile-name` (`~/.codex/profile-name.config.toml`)
4. User config: `~/.codex/config.toml`
5. System config (if present): `/etc/codex/config.toml` on Unix
6. Built-in defaults

Use that precedence to set shared defaults in `config.toml` and keep [profile files](/codex/config-file/config-advanced#profiles) focused on the values that differ.

If you mark a project as untrusted, Codex skips project-scoped `.codex/` layers, including project-local config, hooks, and rules. User and system config still load, including user/global hooks and rules.

For one-off overrides via `-c`/`--config` (including TOML quoting rules), see [Advanced Config](/codex/config-file/config-advanced#one-off-overrides-from-the-cli).

On managed machines, your organization may also enforce constraints via
`requirements.toml` (for example, disallowing `approval_policy = "never"` or
`sandbox_mode = "danger-full-access"`). See [Managed
configuration](/codex/enterprise/managed-configuration) and [Admin-enforced
requirements](/codex/enterprise/managed-configuration#admin-enforced-requirements-requirementstoml).

### Common configuration options

Here are a few options people change most often:

##### Default model

Choose the model Codex uses by default in the CLI and IDE.

```toml
model = "gpt-5.6"
```

##### Approval prompts

Control when Codex pauses to ask before running generated commands.

```toml
approval_policy = "on-request"
```

For behavior differences between `untrusted`, `on-request`, and `never`, see [Run without approval prompts](/codex/agent-approvals-security#run-without-approval-prompts) and [Common sandbox and approval combinations](/codex/agent-approvals-security#common-sandbox-and-approval-combinations).

##### Sandbox level

Adjust how much filesystem and network access Codex has while executing commands.

```toml
sandbox_mode = "workspace-write"
```

For mode-by-mode behavior (including protected `.git`/`.codex` paths and network defaults), see [Sandbox and approvals](/codex/agent-approvals-security#sandbox-and-approvals), [Protected paths in writable roots](/codex/agent-approvals-security#protected-paths-in-writable-roots), and [Network access](/codex/agent-approvals-security#network-access).

##### Permission profiles

Codex also supports named permission profiles for reusable filesystem and
network policies. Built-in profiles are `:read-only`, `:workspace`, and
`:danger-full-access`. Custom profiles use `[permissions.<name>]` tables and a
matching `default_permissions` value. See [Permissions](/codex/permissions).

##### Windows sandbox mode

When running Codex natively on Windows, set the native sandbox mode to `elevated` in the `windows` table. Use `unelevated` only if you don’t have administrator permissions or if elevated setup fails.

```toml
[windows]
sandbox = "elevated"   # Recommended
# sandbox = "unelevated" # Fallback if admin permissions/setup are unavailable
```

##### Web search mode

Codex enables web search by default for local tasks and serves results from a web search cache. The cache is an OpenAI-maintained index of web results, so cached mode returns pre-indexed results instead of fetching live pages. This reduces exposure to prompt injection from arbitrary live content, but you should still treat web results as untrusted. If you are using `--yolo` or another [full access sandbox setting](/codex/agent-approvals-security#common-sandbox-and-approval-combinations), web search defaults to live results. Choose a mode with `web_search`:

- `"cached"` (default) serves results from the web search cache.
- `"indexed"` permits external web access only when the search index gates the request.
- `"live"` fetches the most recent data from the web (same as `--search`).
- `"disabled"` turns off the web search tool.

```toml
web_search = "cached"  # default; serves results from the web search cache
# web_search = "indexed" # gate external web access through the search index
# web_search = "live"  # fetch the most recent data from the web (same as --search)
# web_search = "disabled"
```

##### Reasoning effort

Tune how much reasoning effort the model applies when supported.

```toml
model_reasoning_effort = "high"
```

##### Communication style

Set a default communication style for supported models.

```toml
personality = "friendly" # or "pragmatic" or "none"
```

You can override this later in an active session with `/personality` or per thread/turn when using the app-server APIs.

##### TUI keymap

Customize terminal shortcuts under `tui.keymap`. Selected composer actions fall back to matching `tui.keymap.global` bindings; context-specific bindings take precedence when supported. An empty list unbinds the action.

```toml
[tui.keymap.global]
open_transcript = "ctrl-t"

[tui.keymap.composer]
submit = ["enter", "ctrl-m"]

[tui.keymap.chat]
interrupt_turn = "f12"
```

##### Command environment

Control which environment variables Codex forwards to spawned commands.

```toml
[shell_environment_policy]
include_only = ["PATH", "HOME"]
```

##### Log directory

Override where Codex writes local log files. Setting `log_dir` explicitly also
enables the opt-in plaintext TUI log, `codex-tui.log`, in that directory.

```toml
log_dir = "/absolute/path/to/codex-logs"
```

For one-off runs, you can also set it from the CLI:

```bash
codex -c log_dir=./.codex-log
```

### Feature flags

Use the `[features]` table in `config.toml` to toggle optional and experimental capabilities.

#### Common feature flags

| Key | Default | Maturity | Description |
| --- | --- | --- | --- |
| `apps` | true | Stable | Enable app (connector) integrations |
| `goals` | true | Stable | Enable persisted goals and automatic continuation |
| `hooks` | true | Stable | Enable lifecycle hooks from `hooks.json` or inline `[hooks]`. See [Hooks](/codex/hooks). |
| `fast_mode` | true | Stable | Enable Fast mode selection and the `service_tier = "fast"` path |
| `memories` | false | Experimental | Enable [Memories](/codex/customization/memories) |
| `multi_agent` | true | Stable | Enable subagent collaboration tools |
| `personality` | true | Stable | Enable personality selection controls |
| `remote_plugin` | true | Stable | Enable the remote plugin catalog |
| `shell_snapshot` | true | Stable | Snapshot your shell environment to speed up repeated commands |
| `shell_tool` | true | Stable | Enable the default `shell` tool |
| `unified_exec` | `true` except Windows | Stable | Use the unified PTY-backed exec tool |
| `web_search` | true | Deprecated | Legacy toggle; prefer the top-level `web_search` setting |
| `web_search_cached` | false | Deprecated | Legacy toggle that maps to `web_search = "cached"` when unset |
| `web_search_request` | false | Deprecated | Legacy toggle that maps to `web_search = "live"` when unset |

This table lists common user-facing flags, not every internal or
under-development feature. The Maturity column uses labels such as
Experimental, Beta, and Stable. See [Feature
Maturity](/codex/feature-maturity) for how to interpret these labels.

For lifecycle hook configuration, see [Hooks](/codex/hooks).

#### Enabling features

- In `config.toml`, add `feature_name = true` under `[features]`.
- From the CLI, run `codex --enable feature_name`.
- To enable more than one feature, run `codex --enable feature_a --enable feature_b`.
- To disable a feature, set the key to `false` in `config.toml`.

---

## Advanced Configuration

More advanced configuration options for Codex local clients

Use these options when you need more control over providers, policies, and integrations. For a quick start, see [Config basics](/codex/config-file/config-basic).

For background on project guidance, reusable capabilities, custom slash commands, subagent workflows, and integrations, see [Customization](/codex/customization/overview). For configuration keys, see [Configuration Reference](/codex/config-file/config-reference).

### Profiles

Profiles let you save named configuration layers and switch between them from
the CLI. When you pass `--profile profile-name`, Codex loads
`~/.codex/config.toml`, then overlays `~/.codex/profile-name.config.toml`.
Profile names can contain letters, numbers, hyphens, and underscores.

Create a separate TOML file for each profile. Use top-level config keys in the
profile file; don’t nest them under `[profiles.profile-name]`.

```toml
# ~/.codex/deep-review.config.toml
model = "gpt-5.5"
model_reasoning_effort = "xhigh"
approval_policy = "on-request"
model_catalog_json = "/Users/me/.codex/model-catalogs/deep-review.json"
```

```shell
codex --profile deep-review
codex exec --profile deep-review "review this change"
```

Because the profile file is a layer above your base user config and below
project and CLI config, it only needs the values that differ from your base
config. Profile files can also override `model_catalog_json`; Codex uses the
profile value when both files set it.

In Codex 0.134.0 and later, `--profile` no longer reads `[profiles.profile-name]`
from `config.toml`, and the top-level `profile = "profile-name"` selector is no
longer supported. Move legacy profile settings into
`~/.codex/profile-name.config.toml`, then remove the matching
`[profiles.profile-name]` table and `profile = "profile-name"` selector from
`config.toml`.

### One-off overrides from the CLI

In addition to editing `~/.codex/config.toml`, you can override configuration for a single run from the CLI:

- Prefer dedicated flags when they exist (for example, `--model`).
- Use `-c` / `--config` when you need to override an arbitrary key.

Examples:

```shell
# Dedicated flag
codex --model gpt-5.4

# Generic key/value override (value is TOML, not JSON)
codex --config model='"gpt-5.4"'
codex --config sandbox_workspace_write.network_access=true
codex --config 'shell_environment_policy.include_only=["PATH","HOME"]'
```

Notes:

- Keys can use dot notation to set nested values (for example, `mcp_servers.context7.enabled=false`).
- `--config` values are parsed as TOML. When in doubt, quote the value so your shell doesn’t split it on spaces.
- If the value can’t be parsed as TOML, Codex treats it as a string.

### Config and state locations

Codex stores its local state under `CODEX_HOME` (defaults to `~/.codex`).

Common files you may see there:

- `config.toml` (your local configuration)
- `auth.json` (if you use file-based credential storage) or your OS keychain/keyring
- `history.jsonl` (if history persistence is enabled)
- Other per-user state such as logs and caches

For authentication details (including credential storage modes), see [Authentication](/codex/auth). For the full list of configuration keys, see [Configuration Reference](/codex/config-file/config-reference).

For shared defaults, rules, and skills checked into repos or system paths, see [Team Config](/codex/enterprise/admin-setup#step-4-standardize-local-configuration-with-team-config).

If you just need to point the built-in OpenAI provider at an LLM proxy, router, or data-residency enabled project, set `openai_base_url` in `config.toml` instead of defining a new provider. This changes the base URL for the built-in `openai` provider without requiring a separate `model_providers.<id>` entry.

```toml
openai_base_url = "https://us.api.openai.com/v1"
```

### Project config files (`.codex/config.toml`)

In addition to your user config, Codex reads project-scoped overrides from `.codex/config.toml` files inside your repo. Codex walks from the project root to your current working directory and loads every `.codex/config.toml` it finds. If multiple files define the same key, the closest file to your working directory wins.

For security, Codex loads project-scoped config files only when the project is trusted. If the project is untrusted, Codex ignores project `.codex/` layers, including `.codex/config.toml`, project-local hooks, and project-local rules. User and system layers remain separate and still load.

Relative paths inside a project config (for example, `model_instructions_file`) are resolved relative to the `.codex/` folder that contains the `config.toml`.

Project config files can’t override settings that redirect credentials, alter
host-owned app request metadata, change provider auth, select config profiles,
or run machine-local notification/telemetry commands. Codex ignores the
following keys in project-local `.codex/config.toml` and prints a startup
warning when it sees them: `openai_base_url`, `chatgpt_base_url`,
`apps_mcp_product_sku`, `model_provider`, `model_providers`, `notify`,
`profile`, `profiles`, `experimental_realtime_ws_base_url`, and `otel`. Set
provider, notification, and telemetry keys in your user-level
`~/.codex/config.toml`; select config profiles with `--profile profile-name`
and `~/.codex/profile-name.config.toml`.

### Hooks

Codex can also load lifecycle hooks from either `hooks.json` files or inline
`[hooks]` tables in `config.toml` files that sit next to active config layers.

In practice, the four most useful locations are:

- `~/.codex/hooks.json`
- `~/.codex/config.toml`
- `<repo>/.codex/hooks.json`
- `<repo>/.codex/config.toml`

Project-local hooks load only when the project `.codex/` layer is trusted.
User-level hooks remain independent of project trust.

Inline TOML hooks use the same event structure as `hooks.json`:

```toml
[[hooks.PreToolUse]]
matcher = "^Bash$"

[[hooks.PreToolUse.hooks]]
type = "command"
command = '/usr/bin/python3 "$(git rev-parse --show-toplevel)/.codex/hooks/pre_tool_use_policy.py"'
timeout = 30
statusMessage = "Checking Bash command"
```

If a single layer contains both `hooks.json` and inline `[hooks]`, Codex loads
both and warns. Prefer one representation per layer.

For the current event list, input fields, output behavior, and limitations, see
[Hooks](/codex/hooks).

### Agent roles (`[agents]` in `config.toml`)

For subagent role configuration (`[agents]` in `config.toml`), see [Subagents](/codex/agent-configuration/subagents).

### Project root detection

Codex discovers project configuration (for example, `.codex/` layers and `AGENTS.md`) by walking up from the working directory until it reaches a project root.

By default, Codex treats a directory containing `.git` as the project root. To customize this behavior, set `project_root_markers` in `config.toml`:

```toml
# Treat a directory as the project root when it contains any of these markers.
project_root_markers = [".git", ".hg", ".sl"]
```

Set `project_root_markers = []` to skip searching parent directories and treat the current working directory as the project root.

### Custom model providers

A model provider defines how Codex connects to a model (base URL, wire API, authentication, and optional HTTP headers). Custom providers can’t reuse the reserved built-in provider IDs: `openai`, `ollama`, and `lmstudio`.

Define additional providers and point `model_provider` at them:

```toml
model = "gpt-5.4"
model_provider = "proxy"

[model_providers.proxy]
name = "OpenAI using LLM proxy"
base_url = "http://proxy.example.com"
env_key = "OPENAI_API_KEY"

[model_providers.local_ollama]
name = "Ollama"
base_url = "http://localhost:11434/v1"

[model_providers.mistral]
name = "Mistral"
base_url = "https://api.mistral.ai/v1"
env_key = "MISTRAL_API_KEY"
```

Add request headers when needed:

```toml
[model_providers.example]
http_headers = { "X-Example-Header" = "example-value" }
env_http_headers = { "X-Example-Features" = "EXAMPLE_FEATURES" }
```

Use command-backed authentication when a provider needs Codex to fetch bearer tokens from an external credential helper:

```toml
[model_providers.proxy]
name = "OpenAI using LLM proxy"
base_url = "https://proxy.example.com/v1"
wire_api = "responses"

[model_providers.proxy.auth]
command = "/usr/local/bin/fetch-codex-token"
args = ["--audience", "codex"]
timeout_ms = 5000
refresh_interval_ms = 300000
```

The auth command receives no `stdin` and must print the token to stdout. Codex trims surrounding whitespace, treats an empty token as an error, and refreshes proactively at `refresh_interval_ms`; set `refresh_interval_ms = 0` to refresh only after an authentication retry. Don’t combine `[model_providers.<id>.auth]` with `env_key`, `experimental_bearer_token`, or `requires_openai_auth`.

#### Amazon Bedrock provider

Codex includes a built-in `amazon-bedrock` model provider. Set it directly as
`model_provider`; unlike custom providers, this built-in provider supports only
the nested AWS profile and region overrides.

```toml
model_provider = "amazon-bedrock"
model = "<bedrock-model-id>"

[model_providers.amazon-bedrock.aws]
profile = "default"
region = "eu-central-1"
```

If you omit `profile`, Codex uses the standard AWS credential chain. Set
`region` to the supported Bedrock region that should handle requests.

For the full setup flow, authentication options, supported models, and feature
availability, see [Use Codex with Amazon Bedrock](/codex/amazon-bedrock).

### OSS mode (local providers)

Codex can run against a local “open source” provider such as Ollama or LM
Studio when you pass `--oss`. Choose one for a single run with
`--local-provider`, or set `oss_provider` as the default. If neither is set, the
interactive CLI prompts you to choose; `codex exec` exits with an error.

```toml
# Default local provider used with `--oss`
oss_provider = "ollama" # or "lmstudio"
```

### Azure provider and per-provider tuning

```toml
[model_providers.azure]
name = "Azure"
base_url = "https://YOUR_PROJECT_NAME.openai.azure.com/openai"
env_key = "AZURE_OPENAI_API_KEY"
query_params = { api-version = "2025-04-01-preview" }
wire_api = "responses"
request_max_retries = 4
stream_max_retries = 10
stream_idle_timeout_ms = 300000
```

To change the base URL for the built-in OpenAI provider, use `openai_base_url`; don’t create `[model_providers.openai]`, because you can’t override built-in provider IDs.

### ChatGPT customers using data residency

Projects created with [data residency](https://help.openai.com/en/articles/9903489-data-residency-and-inference-residency-for-chatgpt) enabled can create a model provider to update the base_url with the [correct prefix](https://platform.openai.com/docs/guides/your-data#which-models-and-features-are-eligible-for-data-residency).

```toml
model_provider = "openaidr"
[model_providers.openaidr]
name = "OpenAI Data Residency"
base_url = "https://us.api.openai.com/v1" # Replace 'us' with domain prefix
```

### Model reasoning, verbosity, and limits

```toml
model_reasoning_summary = "none"          # Disable summaries
model_verbosity = "low"                   # Shorten responses
model_supports_reasoning_summaries = true # Force reasoning
model_context_window = 128000             # Context window size
```

`model_verbosity` applies only to providers using the Responses API. Chat Completions providers will ignore the setting.

### Approval policies and sandbox modes

Pick approval strictness (affects when Codex pauses) and sandbox level (affects file/network access).

For operational details to keep in mind while editing `config.toml`, see [Common sandbox and approval combinations](/codex/agent-approvals-security#common-sandbox-and-approval-combinations), [Protected paths in writable roots](/codex/agent-approvals-security#protected-paths-in-writable-roots), and [Network access](/codex/agent-approvals-security#network-access).

For beta permission profiles that configure filesystem and network access together, see [Permissions](/codex/permissions).

You can also use a granular approval policy (`approval_policy = { granular = { ... } }`) to allow or auto-reject individual prompt categories. This is useful when you want normal interactive approvals for some cases but want others, such as `request_permissions` or skill-script prompts, to fail closed automatically.

Set `approvals_reviewer = "auto_review"` to route eligible interactive approval
requests through automatic review. This changes the reviewer, not the sandbox
boundary.

Use `[auto_review].policy` for local reviewer policy instructions. Managed
`guardian_policy_config` takes precedence.

```toml
approval_policy = "untrusted"   # Other options: on-request, never, or { granular = { ... } }
approvals_reviewer = "user"     # Or "auto_review" for automatic review
sandbox_mode = "workspace-write"
allow_login_shell = false       # Optional hardening: disallow login shells for shell tools

# Example granular approval policy:
# approval_policy = { granular = {
#   sandbox_approval = true,
#   rules = true,
#   mcp_elicitations = true,
#   request_permissions = false,
#   skill_approval = false
# } }

[sandbox_workspace_write]
exclude_tmpdir_env_var = false  # Allow $TMPDIR
exclude_slash_tmp = false       # Allow /tmp
writable_roots = ["/Users/YOU/.pyenv/shims"]
network_access = false          # Opt in to outbound network

[auto_review]
policy = """
Use your organization's automatic review policy.
"""
```

#### Named permission profiles

For built-in profiles, custom profile syntax, and the full filesystem and
network configuration model, see [Permissions](/codex/permissions).

For the complete key list and requirements constraints, see
[Configuration Reference](/codex/config-file/config-reference) and
[Managed configuration](/codex/enterprise/managed-configuration).

In workspace-write mode, some environments keep `.git/` and `.codex/`
read-only even when the rest of the workspace is writable. This is why
commands like `git commit` may still require approval to run outside the
sandbox. If you want Codex to skip specific commands (for example, block `git   commit` outside the sandbox), use
[rules](/codex/agent-configuration/rules).

Disable sandboxing entirely (use only if your environment already isolates processes):

```toml
sandbox_mode = "danger-full-access"
```

### Shell environment policy

`shell_environment_policy` controls which environment variables Codex passes to any subprocess it launches (for example, when running a tool-command the model proposes). Start from a clean start (`inherit = "none"`) or a trimmed set (`inherit = "core"`), then layer on excludes, includes, and overrides to avoid leaking secrets while still providing the paths, keys, or flags your tasks need.

```toml
[shell_environment_policy]
inherit = "none"
set = { PATH = "/usr/bin", MY_FLAG = "1" }
ignore_default_excludes = false
exclude = ["AWS_*", "AZURE_*"]
include_only = ["PATH", "HOME"]
```

Patterns are case-insensitive globs (`*`, `?`, `[A-Z]`); `ignore_default_excludes = false` keeps the automatic KEY/SECRET/TOKEN filter before your includes/excludes run.

### MCP servers

See the dedicated [MCP documentation](/codex/extend/mcp) for configuration details.

### Observability and telemetry

Enable OpenTelemetry (OTel) log export to track Codex runs (API requests, SSE/events, prompts, tool approvals/results). Disabled by default; opt in via `[otel]`:

```toml
[otel]
environment = "staging"   # defaults to "dev"
exporter = "none"         # set to otlp-http or otlp-grpc to send events
log_user_prompt = false   # redact user prompts unless explicitly enabled
```

Choose an exporter:

```toml
[otel]
exporter = { otlp-http = {
  endpoint = "https://otel.example.com/v1/logs",
  protocol = "binary",
  headers = { "x-otlp-api-key" = "${OTLP_TOKEN}" }
}}
```

```toml
[otel]
exporter = { otlp-grpc = {
  endpoint = "https://otel.example.com:4317",
  headers = { "x-otlp-meta" = "abc123" }
}}
```

If `exporter = "none"` Codex records events but sends nothing. Exporters batch asynchronously and flush on shutdown. Event metadata includes service name, CLI version, env tag, conversation id, model, sandbox/approval settings, and per-event fields (see [Config Reference](/codex/config-file/config-reference)).

#### What gets emitted

Codex emits structured log events for runs and tool usage. Representative event types include:

- `codex.conversation_starts` (model, reasoning settings, sandbox/approval policy)
- `codex.api_request` (attempt, status/success, duration, and error details)
- `codex.sse_event` (stream event kind, success/failure, duration, plus token counts on `response.completed`)
- `codex.websocket_request` and `codex.websocket_event` (request duration plus per-message kind/success/error)
- `codex.user_prompt` (length; content redacted unless explicitly enabled)
- `codex.tool_decision` (approved/denied and whether the decision came from config vs user)
- `codex.tool_result` (duration, success, output snippet)

#### OTel metrics emitted

When the OTel metrics pipeline is enabled, Codex emits counters and duration histograms for API, stream, and tool activity.

Each metric below also includes default metadata tags: `auth_mode`, `originator`, `session_source`, `model`, and `app.version`.

| Metric | Type | Fields | Description |
| --- | --- | --- | --- |
| `codex.api_request` | counter | `status`, `success` | API request count by HTTP status and success/failure. |
| `codex.api_request.duration_ms` | histogram | `status`, `success` | API request duration in milliseconds. |
| `codex.sse_event` | counter | `kind`, `success` | SSE event count by event kind and success/failure. |
| `codex.sse_event.duration_ms` | histogram | `kind`, `success` | SSE event processing duration in milliseconds. |
| `codex.websocket.request` | counter | `success` | WebSocket request count by success/failure. |
| `codex.websocket.request.duration_ms` | histogram | `success` | WebSocket request duration in milliseconds. |
| `codex.websocket.event` | counter | `kind`, `success` | WebSocket message/event count by type and success/failure. |
| `codex.websocket.event.duration_ms` | histogram | `kind`, `success` | WebSocket message/event processing duration in milliseconds. |
| `codex.tool.call` | counter | `tool`, `success` | Tool invocation count by tool name and success/failure. |
| `codex.tool.call.duration_ms` | histogram | `tool`, `success` | Tool execution duration in milliseconds by tool name and outcome. |

For more security and privacy guidance around telemetry, see [Security](/codex/agent-approvals-security#monitoring-and-telemetry).

#### Metrics

By default, Codex periodically sends a small amount of anonymous usage and health data back to OpenAI. This helps detect when Codex isn’t working correctly and shows what features and configuration options are being used, so the Codex team can focus on what matters most. These metrics don’t contain any personally identifiable information (PII). Metrics collection is independent of OTel log/trace export.

If you want to disable metrics collection entirely across the ChatGPT desktop app, Codex CLI, and IDE extension on a machine, set the analytics flag in your config:

```toml
[analytics]
enabled = false
```

Each metric includes its own fields plus the default context fields below.

##### Default context fields (applies to every event/metric)

- `auth_mode`: `swic` | `api` | `unknown`.
- `model`: name of the model used.
- `app.version`: Codex version.

##### Metrics catalog

Each metric includes the required fields plus the default context fields above. Metric names below omit the `codex.` prefix.
Most metric names are centralized in `codex-rs/otel/src/metrics/names.rs`; feature-specific metrics emitted outside that file are included here too.
If a metric includes the `tool` field, it reflects the internal tool used (for example, `apply_patch` or `shell`) and doesn’t contain the actual shell command or patch `codex` is trying to apply.

##### Runtime and model transport

| Metric | Type | Fields | Description |
| --- | --- | --- | --- |
| `api_request` | counter | `status`, `success` | API request count by HTTP status and success/failure. |
| `api_request.duration_ms` | histogram | `status`, `success` | API request duration in milliseconds. |
| `sse_event` | counter | `kind`, `success` | SSE event count by event kind and success/failure. |
| `sse_event.duration_ms` | histogram | `kind`, `success` | SSE event processing duration in milliseconds. |
| `websocket.request` | counter | `success` | WebSocket request count by success/failure. |
| `websocket.request.duration_ms` | histogram | `success` | WebSocket request duration in milliseconds. |
| `websocket.event` | counter | `kind`, `success` | WebSocket message/event count by type and success/failure. |
| `websocket.event.duration_ms` | histogram | `kind`, `success` | WebSocket message/event processing duration in milliseconds. |
| `responses_api_overhead.duration_ms` | histogram |  | Responses API overhead timing from WebSocket responses. |
| `responses_api_inference_time.duration_ms` | histogram |  | Responses API inference timing from WebSocket responses. |
| `responses_api_engine_iapi_ttft.duration_ms` | histogram |  | Responses API engine IAPI time-to-first-token timing. |
| `responses_api_engine_service_ttft.duration_ms` | histogram |  | Responses API engine service time-to-first-token timing. |
| `responses_api_engine_iapi_tbt.duration_ms` | histogram |  | Responses API engine IAPI time-between-token timing. |
| `responses_api_engine_service_tbt.duration_ms` | histogram |  | Responses API engine service time-between-token timing. |
| `transport.fallback_to_http` | counter | `from_wire_api` | WebSocket-to-HTTP fallback count. |
| `remote_models.fetch_update.duration_ms` | histogram |  | Time to fetch remote model definitions. |
| `remote_models.load_cache.duration_ms` | histogram |  | Time to load the remote model cache. |
| `startup_prewarm.duration_ms` | histogram | `status` | Startup prewarm duration by outcome. |
| `startup_prewarm.age_at_first_turn_ms` | histogram | `status` | Startup prewarm age when the first real turn resolves it. |
| `cloud_requirements.fetch.duration_ms` | histogram |  | Workspace-managed cloud requirements fetch duration. |
| `cloud_requirements.fetch_attempt` | counter | See note | Workspace-managed cloud requirements fetch attempts. |
| `cloud_requirements.fetch_final` | counter | See note | Final workspace-managed cloud requirements fetch outcome. |
| `cloud_requirements.load` | counter | `trigger`, `outcome` | Workspace-managed cloud requirements load outcome. |

The `cloud_requirements.fetch_attempt` metric includes `trigger`, `attempt`, `outcome`, and `status_code` fields. The `cloud_requirements.fetch_final` metric includes `trigger`, `outcome`, `reason`, `attempt_count`, and `status_code` fields.

##### Turn and tool activity

| Metric | Type | Fields | Description |
| --- | --- | --- | --- |
| `turn.e2e_duration_ms` | histogram |  | End-to-end time for a full turn. |
| `turn.ttft.duration_ms` | histogram |  | Time to first token for a turn. |
| `turn.ttfm.duration_ms` | histogram |  | Time to first model output item for a turn. |
| `turn.network_proxy` | counter | `active`, `tmp_mem_enabled` | Whether the managed network proxy was active for the turn. |
| `turn.memory` | counter | `read_allowed`, `feature_enabled`, `config_use_memories`, `has_citations` | Per-turn memory read availability and memory citation usage. |
| `turn.tool.call` | histogram | `tmp_mem_enabled` | Number of tool calls in the turn. |
| `turn.token_usage` | histogram | `token_type`, `tmp_mem_enabled` | Per-turn token usage by token type (`total`, `input`, `cached_input`, `output`, or `reasoning_output`). |
| `tool.call` | counter | `tool`, `success` | Tool invocation count by tool name and success/failure. |
| `tool.call.duration_ms` | histogram | `tool`, `success` | Tool execution duration in milliseconds by tool name and outcome. |
| `tool.unified_exec` | counter | `tty` | Unified exec tool calls by TTY mode. |
| `approval.requested` | counter | `tool`, `approved` | Tool approval request result (`approved`, `approved_with_amendment`, `approved_for_session`, `denied`, `abort`). |
| `mcp.call` | counter | See note | MCP tool invocation result. |
| `mcp.call.duration_ms` | histogram | See note | MCP tool invocation duration. |
| `mcp.tools.list.duration_ms` | histogram | `cache` | MCP tool-list duration, including cache hit/miss state. |
| `mcp.tools.fetch_uncached.duration_ms` | histogram |  | Duration of MCP tool fetches that miss the cache. |
| `mcp.tools.cache_write.duration_ms` | histogram |  | Duration of Codex Apps MCP tool-cache writes. |
| `hooks.run` | counter | `hook_name`, `source`, `status` | Hook run count by hook name, source, and status. |
| `hooks.run.duration_ms` | histogram | `hook_name`, `source`, `status` | Hook run duration in milliseconds. |

The `mcp.call` and `mcp.call.duration_ms` metrics include `status`; normal tool-call emissions also include `tool`, plus `connector_id` and `connector_name` when available. Blocked Codex Apps MCP calls may emit `mcp.call` with only `status`.

##### Threads, tasks, and features

| Metric | Type | Fields | Description |
| --- | --- | --- | --- |
| `feature.state` | counter | `feature`, `value` | Feature values that differ from defaults (emit one row per non-default). |
| `status_line` | counter |  | Session started with a configured status line. |
| `model_warning` | counter |  | Warning sent to the model. |
| `thread.started` | counter | `is_git` | New thread created, tagged by whether the working directory is in a Git repo. |
| `conversation.turn.count` | counter |  | User/assistant turns per thread, recorded at the end of the thread. |
| `thread.fork` | counter | `source` | New thread created by forking an existing thread. |
| `thread.rename` | counter |  | Thread renamed. |
| `thread.side` | counter | `source` | Side conversation created. |
| `thread.skills.enabled_total` | histogram |  | Number of skills enabled for a new thread. |
| `thread.skills.kept_total` | histogram |  | Number of enabled skills kept after prompt rendering. |
| `thread.skills.truncated` | histogram |  | Whether skill rendering truncated the enabled skills list (`1` or `0`). |
| `task.compact` | counter | `type` | Number of compactions per type (`remote` or `local`), including manual and auto. |
| `task.review` | counter |  | Number of reviews triggered. |
| `task.undo` | counter |  | Number of undo actions triggered. |
| `task.user_shell` | counter |  | Number of user shell actions (`!` in the TUI for example). |
| `shell_snapshot` | counter | See note | Whether taking a shell snapshot succeeded. |
| `shell_snapshot.duration_ms` | histogram | `success` | Time to take a shell snapshot. |
| `skill.injected` | counter | `status`, `skill` | Skill injection outcomes by skill. |
| `plugins.startup_sync` | counter | `transport`, `status` | Curated plugin startup sync attempts. |
| `plugins.startup_sync.final` | counter | `transport`, `status` | Final curated plugin startup sync outcome. |
| `multi_agent.spawn` | counter | `role` | Agent spawns by role. |
| `multi_agent.resume` | counter |  | Agent resumes. |
| `multi_agent.nickname_pool_reset` | counter |  | Agent nickname pool resets. |

The `shell_snapshot` metric includes `success` and, on failures, `failure_reason`.

##### Memory and local state

| Metric | Type | Fields | Description |
| --- | --- | --- | --- |
| `memory.phase1` | counter | `status` | Memory phase 1 job counts by status. |
| `memory.phase1.e2e_ms` | histogram |  | End-to-end duration for memory phase 1. |
| `memory.phase1.output` | counter |  | Memory phase 1 outputs written. |
| `memory.phase1.token_usage` | histogram | `token_type` | Memory phase 1 token usage by token type. |
| `memory.phase2` | counter | `status` | Memory phase 2 job counts by status. |
| `memory.phase2.e2e_ms` | histogram |  | End-to-end duration for memory phase 2. |
| `memory.phase2.input` | counter |  | Memory phase 2 input count. |
| `memory.phase2.token_usage` | histogram | `token_type` | Memory phase 2 token usage by token type. |
| `memories.usage` | counter | `kind`, `tool`, `success` | Memory usage by kind, tool, and success/failure. |
| `external_agent_config.detect` | counter | See note | External agent config detections by migration item type. |
| `external_agent_config.import` | counter | See note | External agent config imports by migration item type. |
| `db.backfill` | counter | `status` | Initial state DB backfill results (`upserted`, `failed`). |
| `db.backfill.duration_ms` | histogram | `status` | Duration of the initial state DB backfill. |
| `db.error` | counter | `stage` | Errors during state DB operations. |

The `external_agent_config.detect` and `external_agent_config.import` metrics include `migration_type`; skills migrations also include `skills_count`.

##### Windows sandbox

| Metric | Type | Fields | Description |
| --- | --- | --- | --- |
| `windows_sandbox.setup_success` | counter | `originator`, `mode` | Windows sandbox setup successes. |
| `windows_sandbox.setup_failure` | counter | `originator`, `mode` | Windows sandbox setup failures. |
| `windows_sandbox.setup_duration_ms` | histogram | `result`, `originator`, `mode` | Windows sandbox setup duration. |
| `windows_sandbox.elevated_setup_success` | counter |  | Elevated Windows sandbox setup successes. |
| `windows_sandbox.elevated_setup_failure` | counter | See note | Elevated Windows sandbox setup failures. |
| `windows_sandbox.elevated_setup_canceled` | counter | See note | Canceled elevated Windows sandbox setup attempts. |
| `windows_sandbox.elevated_setup_duration_ms` | histogram | `result` | Elevated Windows sandbox setup duration. |
| `windows_sandbox.elevated_prompt_shown` | counter |  | Elevated sandbox setup prompt shown. |
| `windows_sandbox.elevated_prompt_accept` | counter |  | Elevated sandbox setup prompt accepted. |
| `windows_sandbox.elevated_prompt_use_legacy` | counter |  | User chose legacy sandbox from the elevated prompt. |
| `windows_sandbox.elevated_prompt_quit` | counter |  | User quit from the elevated prompt. |
| `windows_sandbox.fallback_prompt_shown` | counter |  | Fallback sandbox prompt shown. |
| `windows_sandbox.fallback_retry_elevated` | counter |  | User retried elevated setup from the fallback prompt. |
| `windows_sandbox.fallback_use_legacy` | counter |  | User chose legacy sandbox from the fallback prompt. |
| `windows_sandbox.fallback_prompt_quit` | counter |  | User quit from the fallback prompt. |
| `windows_sandbox.legacy_setup_preflight_failed` | counter | See note | Legacy Windows sandbox setup preflight failure. |
| `windows_sandbox.setup_elevated_sandbox_command` | counter |  | Elevated sandbox setup command invoked. |
| `windows_sandbox.createprocessasuserw_failed` | counter | `error_code`, `path_kind`, `exe`, `level` | Windows `CreateProcessAsUserW` failures. |

The elevated setup failure metrics include `code` and `message` when Windows setup failure details are available, and may include `originator` when emitted from the shared setup path. The `windows_sandbox.legacy_setup_preflight_failed` metric includes `originator` when emitted from the shared setup path, but fallback-prompt preflight failures may not include any fields.

#### Feedback controls

By default, local clients let users send feedback from `/feedback`. To disable feedback collection across the ChatGPT desktop app, Codex CLI, and IDE extension on a machine, update your config:

```toml
[feedback]
enabled = false
```

When disabled, `/feedback` shows a disabled message and Codex rejects feedback submissions.

#### Hide or surface reasoning events

If you want to reduce noisy “reasoning” output (for example in CI logs), you can suppress it:

```toml
hide_agent_reasoning = true
```

If you want to surface raw reasoning content when a model emits it:

```toml
show_raw_agent_reasoning = true
```

Enable raw reasoning only if it’s acceptable for your workflow. Some models/providers (like `gpt-oss`) don’t emit raw reasoning; in that case, this setting has no visible effect.

### Notifications

Use `notify` to trigger an external program whenever Codex emits supported events (currently only `agent-turn-complete`). This is handy for desktop toasts, chat webhooks, CI updates, or any side-channel alerting that the built-in TUI notifications don’t cover.

```toml
notify = ["python3", "/path/to/notify.py"]
```

Example `notify.py` (truncated) that reacts to `agent-turn-complete`:

```python
#!/usr/bin/env python3
import json, subprocess, sys

def main() -> int:
    notification = json.loads(sys.argv[1])
    if notification.get("type") != "agent-turn-complete":
        return 0
    title = f"Codex: {notification.get('last-assistant-message', 'Turn Complete!')}"
    message = " ".join(notification.get("input-messages", []))
    subprocess.check_output([
        "terminal-notifier",
        "-title", title,
        "-message", message,
        "-group", "codex-" + notification.get("thread-id", ""),
        "-activate", "com.googlecode.iterm2",
    ])
    return 0

if __name__ == "__main__":
    sys.exit(main())
```

The script receives a single JSON argument. Common fields include:

- `type` (currently `agent-turn-complete`)
- `thread-id` (session identifier)
- `turn-id` (turn identifier)
- `cwd` (working directory)
- `input-messages` (user messages that led to the turn)
- `last-assistant-message` (last assistant message text)

Place the script somewhere on disk and point `notify` to it.

#### `notify` vs `tui.notifications`

- `notify` runs an external program (good for webhooks, desktop notifiers, CI hooks).
- `tui.notifications` is built in to the TUI and can optionally filter by event type (for example, `agent-turn-complete` and `approval-requested`).
- `tui.notification_method` controls how the TUI emits terminal notifications (`auto`, `osc9`, or `bel`).
- `tui.notification_condition` controls whether TUI notifications fire only when
the terminal is `unfocused` or `always`.

In `auto` mode, Codex prefers OSC 9 notifications (a terminal escape sequence some terminals interpret as a desktop notification) and falls back to BEL (`\x07`) otherwise.

See [Configuration Reference](/codex/config-file/config-reference) for the exact keys.

### History persistence

By default, Codex saves local session transcripts under `CODEX_HOME` (for example, `~/.codex/history.jsonl`). To disable local history persistence:

```toml
[history]
persistence = "none"
```

To cap the history file size, set `history.max_bytes`. When the file exceeds the cap, Codex drops the oldest entries and compacts the file while keeping the newest records.

```toml
[history]
max_bytes = 104857600 # 100 MiB
```

### Clickable citations

If you use a terminal/editor integration that supports it, Codex can render file citations as clickable links. Configure `file_opener` to pick the URI scheme Codex uses:

```toml
file_opener = "vscode" # or cursor, windsurf, vscode-insiders, none
```

Example: a citation like `/home/user/project/main.py:42` can be rewritten into a clickable `vscode://file/...:42` link.

### Project instructions discovery

Codex reads `AGENTS.md` (and related files) and includes a limited amount of project guidance in the first turn of a session. Two knobs control how this works:

- `project_doc_max_bytes`: how much to read from each `AGENTS.md` file
- `project_doc_fallback_filenames`: additional filenames to try when `AGENTS.md` is missing at a directory level

For a detailed walkthrough, see [Custom instructions with AGENTS.md](/codex/agent-configuration/agents-md).

### TUI options

Running `codex` with no subcommand launches the interactive terminal UI (TUI). Codex exposes some TUI-specific configuration under `[tui]`, including:

- `tui.notifications`: enable/disable notifications (or restrict to specific types)
- `tui.notification_method`: choose `auto`, `osc9`, or `bel` for terminal notifications
- `tui.notification_condition`: choose `unfocused` or `always` for when
notifications fire
- `tui.animations`: enable/disable ASCII animations and shimmer effects
- `tui.alternate_screen`: control alternate screen usage (set to `never` to keep terminal scrollback)
- `tui.show_tooltips`: show or hide onboarding tooltips on the welcome screen

`tui.notification_method` defaults to `auto`. In `auto` mode, Codex prefers OSC 9 notifications (a terminal escape sequence some terminals interpret as a desktop notification) when the terminal appears to support them, and falls back to BEL (`\x07`) otherwise.

See [Configuration Reference](/codex/config-file/config-reference) for the full key list.

---

## Configuration Reference

Complete reference for Codex config.toml and requirements.toml

Use this page as a searchable reference for Codex configuration files. For conceptual guidance and examples, start with [Config basics](/codex/config-file/config-basic) and [Advanced Config](/codex/config-file/config-advanced).

### `config.toml`

User-level configuration lives in `~/.codex/config.toml`. You can also add project-scoped overrides in `.codex/config.toml` files. Codex loads project-scoped config files only when you trust the project.

Project-scoped config can’t override machine-local provider, auth,
host-owned app request metadata, notification, configuration profile selection,
or telemetry routing keys. Codex ignores `openai_base_url`,
`chatgpt_base_url`, `apps_mcp_product_sku`, `model_provider`,
`model_providers`, `notify`, `profile`, `profiles`,
`experimental_realtime_ws_base_url`, and `otel` when they appear in a
project-local `.codex/config.toml`; put provider, notification, and telemetry
keys in user-level config instead. Config [profile files](/codex/config-file/config-advanced#profiles) live next to
`config.toml` as `$CODEX_HOME/profile-name.config.toml`; select one with
`--profile profile-name`.

For sandbox and approval keys (`approval_policy`, `sandbox_mode`, and `sandbox_workspace_write.*`), pair this reference with [Sandbox and approvals](/codex/agent-approvals-security#sandbox-and-approvals), [Protected paths in writable roots](/codex/agent-approvals-security#protected-paths-in-writable-roots), and [Network access](/codex/agent-approvals-security#network-access). For beta permission profiles, see [Permissions](/codex/permissions).

| Key | Type / Values | Details |
| --- | --- | --- |
| `agents.<name>.config_file` | `string (path)` | Path to a TOML config layer for that role; relative paths resolve from the config file that declares the role. |
| `agents.<name>.description` | `string` | Role guidance shown to Codex when choosing and spawning that agent type. |
| `agents.<name>.nickname_candidates` | `array<string>` | Optional pool of display nicknames for spawned agents in that role. |
| `agents.interrupt_message` | `boolean` | Record a model-visible message when an agent turn is interrupted (default: true). |
| `agents.job_max_runtime_seconds` | `number` | Default per-worker timeout for `spawn_agents_on_csv` jobs. When unset, the tool falls back to 1800 seconds per worker. |
| `agents.max_depth` | `number` | Maximum nesting depth allowed for spawned agent threads (root sessions start at depth 0; default: 1). |
| `agents.max_threads` | `number` | Maximum number of agent threads that can be open concurrently. Defaults to `6` when unset. |
| `allow_login_shell` | `boolean` | Allow shell-based tools to use login-shell semantics. Defaults to `true`; when `false`, `login = true` requests are rejected and omitted `login` defaults to non-login shells. |
| `analytics.enabled` | `boolean` | Enable or disable analytics for this machine/profile. When unset, the client default applies. |
| `approval_policy` | `untrusted \| on-request \| never \| { granular = { sandbox_approval = bool, rules = bool, mcp_elicitations = bool, request_permissions = bool, skill_approval = bool } }` | Controls when Codex pauses for approval before executing commands. You can also use `approval_policy = { granular = { ... } }` to allow or auto-reject specific prompt categories while keeping other prompts interactive. `on-failure` is deprecated; use `on-request` for interactive runs or `never` for non-interactive runs. |
| `approval_policy.granular.mcp_elicitations` | `boolean` | When `true`, MCP elicitation prompts are allowed to surface instead of being auto-rejected. |
| `approval_policy.granular.request_permissions` | `boolean` | When `true`, prompts from the `request_permissions` tool are allowed to surface. |
| `approval_policy.granular.rules` | `boolean` | When `true`, approvals triggered by execpolicy `prompt` rules are allowed to surface. |
| `approval_policy.granular.sandbox_approval` | `boolean` | When `true`, sandbox escalation approval prompts are allowed to surface. |
| `approval_policy.granular.skill_approval` | `boolean` | When `true`, skill-script approval prompts are allowed to surface. |
| `approvals_reviewer` | `user \| auto_review` | Who reviews eligible approval prompts under `on-request` or granular approval policies. Defaults to `user`; `auto_review` uses the reviewer subagent. This setting doesn't change sandboxing or review actions already allowed inside the sandbox. |
| `apps._default.approvals_reviewer` | `user \| auto_review` | Default reviewer for app tool approval prompts unless overridden per app. When omitted, apps inherit the top-level `approvals_reviewer` value. |
| `apps._default.default_tools_approval_mode` | `auto \| prompt \| writes \| approve` | Default approval behavior for app tools without per-app or per-tool overrides. |
| `apps._default.destructive_enabled` | `boolean` | Default allow/deny for app tools with `destructive_hint = true`. |
| `apps._default.enabled` | `boolean` | Default app enabled state for all apps unless overridden per app. |
| `apps._default.open_world_enabled` | `boolean` | Default allow/deny for app tools with `open_world_hint = true`. |
| `apps.<id>.approvals_reviewer` | `user \| auto_review` | Reviewer for this app's tool approval prompts. Overrides `apps._default.approvals_reviewer`. |
| `apps.<id>.default_tools_approval_mode` | `auto \| prompt \| writes \| approve` | Default approval behavior for tools in this app unless a per-tool override exists. |
| `apps.<id>.default_tools_enabled` | `boolean` | Default enabled state for tools in this app unless a per-tool override exists. |
| `apps.<id>.destructive_enabled` | `boolean` | Allow or block tools in this app that advertise `destructive_hint = true`. |
| `apps.<id>.enabled` | `boolean` | Enable or disable a specific app/connector by id (default: true). |
| `apps.<id>.open_world_enabled` | `boolean` | Allow or block tools in this app that advertise `open_world_hint = true`. |
| `apps.<id>.tools.<tool>.approval_mode` | `auto \| prompt \| writes \| approve` | Per-tool approval behavior override for a single app tool. |
| `apps.<id>.tools.<tool>.enabled` | `boolean` | Per-tool enabled override for an app tool (for example `repos/list`). |
| `auto_review.policy` | `string` | Local Markdown policy instructions for automatic review. Managed `guardian_policy_config` takes precedence. Blank values are ignored. |
| `background_terminal_max_timeout` | `number` | Maximum poll window in milliseconds for empty `write_stdin` polls (background terminal polling). Default: `300000` (5 minutes). Replaces the older `background_terminal_timeout` key. |
| `chatgpt_base_url` | `string` | Override the base URL used during the ChatGPT login flow. |
| `check_for_update_on_startup` | `boolean` | Check for Codex updates on startup (set to false only when updates are centrally managed). |
| `cli_auth_credentials_store` | `file \| keyring \| auto` | Control where the CLI stores cached credentials (file-based auth.json vs OS keychain). |
| `compact_prompt` | `string` | Inline override for the history compaction prompt. |
| `computer_use.windows.always_allowed_app_ids` | `array<string>` | Windows app identifiers that Computer Use can open without prompting. Apps not in the list require approval; remove saved entries from the ChatGPT desktop app's Computer Use settings. |
| `default_permissions` | `string` | Name of the default permissions profile to apply to sandboxed tool calls. Built-ins are `:read-only`, `:workspace`, and `:danger-full-access`; custom profile names require matching `[permissions.<name>]` tables. Don't combine with `sandbox_mode` or `[sandbox_workspace_write]`. |
| `developer_instructions` | `string` | Additional developer instructions injected into the session (optional). |
| `disable_paste_burst` | `boolean` | Disable burst-paste detection in the TUI. |
| `experimental_compact_prompt_file` | `string (path)` | Load the compaction prompt override from a file (experimental). |
| `experimental_use_unified_exec_tool` | `boolean` | Legacy name for enabling unified exec; prefer `[features].unified_exec` or `codex --enable unified_exec`. |
| `features.apps` | `boolean` | Enable app (connector) integrations (stable; on by default). |
| `features.code_mode.direct_only_tool_namespaces` | `array<string>` | Tool namespaces code mode can use only through direct tool calls. |
| `features.code_mode.enabled` | `boolean` | Enable code mode feature configuration. This feature is under development and off by default. |
| `features.code_mode.excluded_tool_namespaces` | `array<string>` | Tool namespaces code mode excludes from nested code-mode tool guidance and executor exposure. |
| `features.enable_request_compression` | `boolean` | Compress streaming request bodies with zstd when supported (stable; on by default). |
| `features.fast_mode` | `boolean` | Enable model-catalog service tier selection in the TUI, including Fast-tier commands when the active model advertises them (stable; on by default). |
| `features.goals` | `boolean` | Enable persisted goals and automatic continuation (stable; on by default). |
| `features.hooks` | `boolean` | Enable lifecycle hooks loaded from `hooks.json` or inline `[hooks]` config. `features.codex_hooks` is a deprecated alias. |
| `features.memories` | `boolean` | Enable [Memories](/codex/customization/memories) (off by default). |
| `features.multi_agent` | `boolean` | Enable multi-agent collaboration tools (`spawn_agent`, `send_input`, `resume_agent`, `wait_agent`, and `close_agent`) (stable; on by default). |
| `features.network_proxy` | `boolean \| table` | Enable sandboxed networking. Use a table form when setting network policy options such as `domains` (experimental; off by default). |
| `features.network_proxy.allow_local_binding` | `boolean` | Allow broader local/private-network access. Defaults to `false`; exact local IP literal or `localhost` allow rules can still permit specific local targets. |
| `features.network_proxy.allow_upstream_proxy` | `boolean` | Allow chaining through an upstream proxy from the environment. Defaults to `true`. |
| `features.network_proxy.dangerously_allow_all_unix_sockets` | `boolean` | Permit arbitrary Unix socket destinations instead of allowlist-only access. Defaults to `false`; use only in tightly controlled environments. |
| `features.network_proxy.dangerously_allow_non_loopback_proxy` | `boolean` | Permit non-loopback listener addresses. Defaults to `false`; enabling it can expose proxy listeners beyond localhost. |
| `features.network_proxy.domains` | `map<string, allow \| deny>` | Domain policy for sandboxed networking. Unset by default, which means no external destinations are allowed until you add `allow` rules. Supports exact hosts, `*.example.com` for subdomains only, `**.example.com` for apex plus subdomains, and global `*` allow rules; prefer scoped rules because `*` broadly opens public outbound access. Add `deny` rules for blocked destinations; `deny` wins on conflicts. |
| `features.network_proxy.enable_socks5` | `boolean` | Expose SOCKS5 support. Defaults to `true`. |
| `features.network_proxy.enable_socks5_udp` | `boolean` | Allow UDP over SOCKS5. Defaults to `true`. |
| `features.network_proxy.enabled` | `boolean` | Enable sandboxed networking. Defaults to `false`. |
| `features.network_proxy.proxy_url` | `string` | HTTP listener URL for sandboxed networking. Defaults to `"http://127.0.0.1:3128"`. |
| `features.network_proxy.socks_url` | `string` | SOCKS5 listener URL. Defaults to `"http://127.0.0.1:8081"`. |
| `features.network_proxy.unix_sockets` | `map<string, allow \| deny>` | Unix socket policy for sandboxed networking. Unset by default; add `allow` entries for permitted sockets. |
| `features.personality` | `boolean` | Enable personality selection controls (stable; on by default). |
| `features.prevent_idle_sleep` | `boolean` | Prevent the machine from sleeping while a turn is actively running (experimental; off by default). |
| `features.remote_plugin` | `boolean` | Enable the remote plugin catalog (stable; on by default). |
| `features.rollout_budget.enabled` | `boolean` | Enable rollout budget tracking. This feature is under development and off by default. When enabled, `features.rollout_budget.limit_tokens` is required. |
| `features.rollout_budget.limit_tokens` | `integer` | Positive token limit for rollout budget tracking. Required when rollout budget is enabled. |
| `features.rollout_budget.prefill_token_weight` | `number` | Finite non-negative multiplier for prefill tokens in rollout budget accounting. Defaults to `1.0`. |
| `features.rollout_budget.reminder_interval_tokens` | `integer` | Positive token interval between rollout budget reminders. Defaults to 10% of `limit_tokens`, with a minimum of 1 token. |
| `features.rollout_budget.sampling_token_weight` | `number` | Finite non-negative multiplier for sampled tokens in rollout budget accounting. Defaults to `1.0`. |
| `features.shell_snapshot` | `boolean` | Snapshot shell environment to speed up repeated commands (stable; on by default). |
| `features.shell_tool` | `boolean` | Enable the default `shell` tool for running commands (stable; on by default). |
| `features.skill_mcp_dependency_install` | `boolean` | Allow prompting and installing missing MCP dependencies for skills (stable; on by default). |
| `features.unified_exec` | `boolean` | Use the unified PTY-backed exec tool (stable; enabled by default except on Windows). |
| `features.web_search` | `boolean` | Deprecated legacy toggle; prefer the top-level `web_search` setting. |
| `features.web_search_cached` | `boolean` | Deprecated legacy toggle. When `web_search` is unset, true maps to `web_search = "cached"`. |
| `features.web_search_request` | `boolean` | Deprecated legacy toggle. When `web_search` is unset, true maps to `web_search = "live"`. |
| `feedback.enabled` | `boolean` | Enable feedback submission via `/feedback` across local clients (default: true). |
| `file_opener` | `vscode \| vscode-insiders \| windsurf \| cursor \| none` | URI scheme used to open citations from Codex output (default: `vscode`). |
| `forced_chatgpt_workspace_id` | `string (uuid)` | Limit ChatGPT logins to a specific workspace identifier. |
| `forced_login_method` | `chatgpt \| api` | Restrict Codex to a specific authentication method. |
| `hide_agent_reasoning` | `boolean` | Suppress reasoning events in both the TUI and `codex exec` output. |
| `history.max_bytes` | `number` | If set, caps the history file size in bytes by dropping oldest entries. |
| `history.persistence` | `save-all \| none` | Control whether Codex saves session transcripts to history.jsonl. |
| `hooks` | `table` | Lifecycle hooks configured inline in `config.toml`. Uses the same event schema as `hooks.json`; see the Hooks guide for examples and supported events. |
| `hooks.<Event>` | `array<table>` | Matcher groups for hook events such as `PreToolUse`, `PermissionRequest`, `PostToolUse`, `PreCompact`, `PostCompact`, `SessionStart`, `SubagentStart`, `SubagentStop`, `UserPromptSubmit`, or `Stop`. |
| `hooks.<Event>[].hooks` | `array<table>` | Hook handlers for a matcher group. Command hooks are currently supported; prompt and agent hook handlers are parsed but skipped. |
| `hooks.<Event>[].hooks[].commandWindows` | `string` | Windows-only command override for command hooks. The TOML alias `command_windows` is also accepted. |
| `instructions` | `string` | Reserved for future use; prefer `model_instructions_file` or `AGENTS.md`. |
| `log_dir` | `string (path)` | Directory where Codex writes log files; defaults to `$CODEX_HOME/log`. Setting this explicitly also enables the opt-in plaintext TUI log, `codex-tui.log`, in that directory. |
| `mcp_oauth_callback_port` | `integer` | Optional fixed port for the local HTTP callback server used during MCP OAuth login. When unset, Codex binds to an ephemeral port chosen by the OS. |
| `mcp_oauth_callback_url` | `string` | Optional base callback URL override for MCP OAuth login (for example, a devbox ingress URL). Codex appends a server-specific callback ID before sending the final OAuth `redirect_uri`, so register the full derived URI with your provider. `mcp_oauth_callback_port` still controls the callback listener port. |
| `mcp_oauth_credentials_store` | `auto \| file \| keyring` | Preferred store for MCP OAuth credentials. |
| `mcp_servers.<id>.args` | `array<string>` | Arguments passed to the MCP stdio server command. |
| `mcp_servers.<id>.auth` | `oauth \| chatgpt` | Authentication fallback for an MCP HTTP server after configured bearer tokens and authorization headers. `oauth` (default) uses stored MCP OAuth credentials when available. `chatgpt` uses the current ChatGPT session for the trusted first-party ChatGPT origin, then falls back to stored OAuth. Both modes can connect without authentication if no credential source resolves. |
| `mcp_servers.<id>.bearer_token_env_var` | `string` | Environment variable sourcing the bearer token for an MCP HTTP server. |
| `mcp_servers.<id>.command` | `string` | Launcher command for an MCP stdio server. |
| `mcp_servers.<id>.cwd` | `string` | Working directory for the MCP stdio server process. |
| `mcp_servers.<id>.default_tools_approval_mode` | `auto \| prompt \| writes \| approve` | Default approval behavior for MCP tools on this server unless a per-tool override exists. |
| `mcp_servers.<id>.disabled_tools` | `array<string>` | Deny list applied after `enabled_tools` for the MCP server. |
| `mcp_servers.<id>.enabled` | `boolean` | Disable an MCP server without removing its configuration. |
| `mcp_servers.<id>.enabled_tools` | `array<string>` | Allow list of tool names exposed by the MCP server. |
| `mcp_servers.<id>.env` | `map<string,string>` | Environment variables forwarded to the MCP stdio server. |
| `mcp_servers.<id>.env_http_headers` | `map<string,string>` | HTTP headers populated from environment variables for an MCP HTTP server. |
| `mcp_servers.<id>.env_vars` | `array<string \| { name = string, source = "local" \| "remote" }>` | Additional environment variables to whitelist for an MCP stdio server. String entries default to `source = "local"`; use `source = "remote"` only with executor-backed remote stdio. |
| `mcp_servers.<id>.experimental_environment` | `local \| remote` | Experimental placement for an MCP server. `remote` starts stdio servers through a remote executor environment; streamable HTTP remote placement is not implemented. |
| `mcp_servers.<id>.http_headers` | `map<string,string>` | Static HTTP headers included with each MCP HTTP request. |
| `mcp_servers.<id>.oauth_resource` | `string` | Optional RFC 8707 OAuth resource parameter to include during MCP login. |
| `mcp_servers.<id>.required` | `boolean` | When true, fail startup/resume if this enabled MCP server cannot initialize. |
| `mcp_servers.<id>.scopes` | `array<string>` | OAuth scopes to request when authenticating to that MCP server. |
| `mcp_servers.<id>.startup_timeout_ms` | `number` | Alias for `startup_timeout_sec` in milliseconds. |
| `mcp_servers.<id>.startup_timeout_sec` | `number` | Override the default 10s startup timeout for an MCP server. |
| `mcp_servers.<id>.tool_timeout_sec` | `number` | Override the default 60s per-tool timeout for an MCP server. |
| `mcp_servers.<id>.tools.<tool>.approval_mode` | `auto \| prompt \| writes \| approve` | Per-tool approval behavior override for one MCP tool on this server. |
| `mcp_servers.<id>.url` | `string` | Endpoint for an MCP streamable HTTP server. |
| `memories.consolidation_model` | `string` | Optional model override for global memory consolidation. |
| `memories.disable_on_external_context` | `boolean` | When `true`, threads that use external context such as MCP tool calls, web search, or tool search are kept out of memory generation. Defaults to `false`. Legacy alias: `memories.no_memories_if_mcp_or_web_search`. |
| `memories.extract_model` | `string` | Optional model override for per-thread memory extraction. |
| `memories.generate_memories` | `boolean` | When `false`, newly created threads are not stored as memory-generation inputs. Defaults to `true`. |
| `memories.max_raw_memories_for_consolidation` | `number` | Maximum recent raw memories retained for global consolidation. Defaults to `256` and is capped at `4096`. |
| `memories.max_rollout_age_days` | `number` | Maximum age of threads considered for memory generation. Defaults to `30` and is clamped to `0`-`90`. |
| `memories.max_rollouts_per_startup` | `number` | Maximum rollout candidates processed per startup pass. Defaults to `16` and is capped at `128`. |
| `memories.max_unused_days` | `number` | Maximum days since a memory was last used before it becomes ineligible for consolidation. Defaults to `30` and is clamped to `0`-`365`. |
| `memories.min_rate_limit_remaining_percent` | `number` | Minimum remaining percentage required in Codex rate-limit windows before memory generation starts. Defaults to `25` and is clamped to `0`-`100`. |
| `memories.min_rollout_idle_hours` | `number` | Minimum idle time before a thread is considered for memory generation. Defaults to `6` and is clamped to `1`-`48`. |
| `memories.use_memories` | `boolean` | When `false`, Codex skips injecting existing memories into future sessions. Defaults to `true`. |
| `model` | `string` | Model to use (e.g., `gpt-5.5`). |
| `model_auto_compact_token_limit` | `number` | Token threshold that triggers automatic history compaction (unset uses model defaults). |
| `model_auto_compact_token_limit_scope` | `total \| body_after_prefix` | Controls whether the auto-compaction threshold counts the full active context (`total`, the default) or only growth after the carried compaction-window prefix (`body_after_prefix`). |
| `model_catalog_json` | `string (path)` | Optional path to a JSON model catalog loaded on startup. A selected `$CODEX_HOME/profile-name.config.toml` profile file can override this per profile. |
| `model_context_window` | `number` | Context window tokens available to the active model. |
| `model_instructions_file` | `string (path)` | Replacement for built-in instructions instead of `AGENTS.md`. |
| `model_provider` | `string` | Provider id from `model_providers` (default: `openai`). |
| `model_providers.<id>` | `table` | Custom provider definition. Built-in provider IDs (`openai`, `ollama`, and `lmstudio`) are reserved and cannot be overridden. |
| `model_providers.<id>.auth` | `table` | Command-backed bearer token configuration for a custom provider. Do not combine with `env_key`, `experimental_bearer_token`, or `requires_openai_auth`. |
| `model_providers.<id>.auth.args` | `array<string>` | Arguments passed to the token command. |
| `model_providers.<id>.auth.command` | `string` | Command to run when Codex needs a bearer token. The command must print the token to stdout. |
| `model_providers.<id>.auth.cwd` | `string (path)` | Working directory for the token command. |
| `model_providers.<id>.auth.refresh_interval_ms` | `number` | How often Codex proactively refreshes the token in milliseconds (default: 300000). Set to `0` to refresh only after an authentication retry. |
| `model_providers.<id>.auth.timeout_ms` | `number` | Maximum token command runtime in milliseconds (default: 5000). |
| `model_providers.<id>.base_url` | `string` | API base URL for the model provider. |
| `model_providers.<id>.env_http_headers` | `map<string,string>` | HTTP headers populated from environment variables when present. |
| `model_providers.<id>.env_key` | `string` | Environment variable supplying the provider API key. |
| `model_providers.<id>.env_key_instructions` | `string` | Optional setup guidance for the provider API key. |
| `model_providers.<id>.experimental_bearer_token` | `string` | Direct bearer token for the provider (discouraged; use `env_key`). |
| `model_providers.<id>.http_headers` | `map<string,string>` | Static HTTP headers added to provider requests. |
| `model_providers.<id>.name` | `string` | Display name for a custom model provider. |
| `model_providers.<id>.query_params` | `map<string,string>` | Extra query parameters appended to provider requests. |
| `model_providers.<id>.request_max_retries` | `number` | Retry count for HTTP requests to the provider (default: 4). |
| `model_providers.<id>.requires_openai_auth` | `boolean` | The provider uses OpenAI authentication (defaults to false). |
| `model_providers.<id>.stream_idle_timeout_ms` | `number` | Idle timeout for SSE streams in milliseconds (default: 300000). |
| `model_providers.<id>.stream_max_retries` | `number` | Retry count for SSE streaming interruptions (default: 5). |
| `model_providers.<id>.supports_websockets` | `boolean` | Whether that provider supports the Responses API WebSocket transport. |
| `model_providers.<id>.wire_api` | `responses` | Protocol used by the provider. `responses` is the only supported value, and it is the default when omitted. |
| `model_providers.amazon-bedrock.aws.profile` | `string` | AWS profile name used by the built-in `amazon-bedrock` provider. |
| `model_providers.amazon-bedrock.aws.region` | `string` | AWS region used by the built-in `amazon-bedrock` provider. |
| `model_reasoning_effort` | `minimal \| low \| medium \| high \| xhigh` | Adjust reasoning effort for supported models (Responses API only; `xhigh` is model-dependent). |
| `model_reasoning_summary` | `auto \| concise \| detailed \| none` | Select reasoning summary detail or disable summaries entirely. |
| `model_supports_reasoning_summaries` | `boolean` | Force Codex to send or not send reasoning metadata. |
| `model_verbosity` | `low \| medium \| high` | Optional GPT-5 Responses API verbosity override; when unset, the selected model/preset default is used. |
| `notice.hide_full_access_warning` | `boolean` | Track acknowledgement of the full access warning prompt. |
| `notice.hide_gpt-5.1-codex-max_migration_prompt` | `boolean` | Track acknowledgement of the gpt-5.1-codex-max migration prompt. |
| `notice.hide_gpt5_1_migration_prompt` | `boolean` | Track acknowledgement of the GPT-5.1 migration prompt. |
| `notice.hide_rate_limit_model_nudge` | `boolean` | Track opt-out of the rate limit model switch reminder. |
| `notice.hide_world_writable_warning` | `boolean` | Track acknowledgement of the Windows world-writable directories warning. |
| `notice.model_migrations` | `map<string,string>` | Track acknowledged model migrations as old->new mappings. |
| `notify` | `array<string>` | Command invoked for notifications; receives a JSON payload from Codex. |
| `openai_base_url` | `string` | Base URL override for the built-in `openai` model provider. |
| `oss_provider` | `lmstudio \| ollama` | Default local provider used when running with `--oss` (defaults to prompting if unset). |
| `otel.environment` | `string` | Environment tag applied to emitted OpenTelemetry events (default: `dev`). |
| `otel.exporter` | `none \| otlp-http \| otlp-grpc` | Select the OpenTelemetry exporter and provide any endpoint metadata. |
| `otel.exporter.<id>.endpoint` | `string` | Exporter endpoint for OTEL logs. |
| `otel.exporter.<id>.headers` | `map<string,string>` | Static headers included with OTEL exporter requests. |
| `otel.exporter.<id>.protocol` | `binary \| json` | Protocol used by the OTLP/HTTP exporter. |
| `otel.exporter.<id>.tls.ca-certificate` | `string` | CA certificate path for OTEL exporter TLS. |
| `otel.exporter.<id>.tls.client-certificate` | `string` | Client certificate path for OTEL exporter TLS. |
| `otel.exporter.<id>.tls.client-private-key` | `string` | Client private key path for OTEL exporter TLS. |
| `otel.log_user_prompt` | `boolean` | Opt in to exporting raw user prompts with OpenTelemetry logs. |
| `otel.metrics_exporter` | `none \| statsig \| otlp-http \| otlp-grpc` | Select the OpenTelemetry metrics exporter (defaults to `statsig`). |
| `otel.trace_exporter` | `none \| otlp-http \| otlp-grpc` | Select the OpenTelemetry trace exporter and provide any endpoint metadata. |
| `otel.trace_exporter.<id>.endpoint` | `string` | Trace exporter endpoint for OTEL logs. |
| `otel.trace_exporter.<id>.headers` | `map<string,string>` | Static headers included with OTEL trace exporter requests. |
| `otel.trace_exporter.<id>.protocol` | `binary \| json` | Protocol used by the OTLP/HTTP trace exporter. |
| `otel.trace_exporter.<id>.tls.ca-certificate` | `string` | CA certificate path for OTEL trace exporter TLS. |
| `otel.trace_exporter.<id>.tls.client-certificate` | `string` | Client certificate path for OTEL trace exporter TLS. |
| `otel.trace_exporter.<id>.tls.client-private-key` | `string` | Client private key path for OTEL trace exporter TLS. |
| `permissions.<name>.description` | `string` | Human-readable description for this named profile. A profile does not inherit its parent's description through `extends`. |
| `permissions.<name>.extends` | `string` | Optional parent profile applied before this named profile. Set it to another named profile, `:read-only`, or `:workspace`; `:danger-full-access`, undefined parents, and cycles are rejected. |
| `permissions.<name>.filesystem` | `table` | Named filesystem permission profile. Each key is an absolute path or special token such as `:minimal` or `:workspace_roots`. |
| `permissions.<name>.filesystem.":workspace_roots".<subpath-or-glob>` | `"read" \| "write" \| "deny"` | Scoped filesystem access relative to each effective workspace root. Use `"."` for the root itself; glob subpaths such as `"**/*.env"` can deny reads with `"deny"`. |
| `permissions.<name>.filesystem.<path-or-glob>` | `"read" \| "write" \| "deny" \| table` | Grant direct access for a path, glob pattern, or special token, or scope nested entries under that root. Use `"deny"` to deny reads for matching paths. |
| `permissions.<name>.filesystem.glob_scan_max_depth` | `number` | Maximum depth for expanding deny-read glob patterns on platforms that snapshot matches before sandbox startup. Must be at least `1` when set. |
| `permissions.<name>.network.allow_local_binding` | `boolean` | Permit broader local/private-network access through sandboxed networking. Exact local IP literal or `localhost` allow rules can still permit specific local targets when this stays `false`. |
| `permissions.<name>.network.allow_upstream_proxy` | `boolean` | Allow sandboxed networking to chain through another upstream proxy. |
| `permissions.<name>.network.dangerously_allow_all_unix_sockets` | `boolean` | Allow arbitrary Unix socket destinations instead of the default restricted set. Use only in tightly controlled environments. |
| `permissions.<name>.network.dangerously_allow_non_loopback_proxy` | `boolean` | Permit non-loopback bind addresses for sandboxed networking listeners. Enabling it can expose listeners beyond localhost. |
| `permissions.<name>.network.domains` | `table` | Domain rules for sandboxed networking. Supports exact hosts, `*.example.com` for subdomains only, `**.example.com` for apex plus subdomains, and global `*` allow rules. `deny` wins on conflicts. |
| `permissions.<name>.network.domains.<pattern>` | `allow \| deny` | Allow or deny an exact host or scoped wildcard pattern such as `*.example.com` or `**.example.com`. |
| `permissions.<name>.network.enable_socks5` | `boolean` | Expose SOCKS5 support when this permissions profile enables sandboxed networking. |
| `permissions.<name>.network.enable_socks5_udp` | `boolean` | Allow UDP over the SOCKS5 listener when enabled. |
| `permissions.<name>.network.enabled` | `boolean` | Enable network access for this named permissions profile. This changes the sandbox network policy; it does not start the network proxy by itself. |
| `permissions.<name>.network.mode` | `limited \| full` | Network proxy mode used for subprocess traffic. |
| `permissions.<name>.network.proxy_url` | `string` | HTTP listener URL used when this permissions profile enables sandboxed networking. |
| `permissions.<name>.network.socks_url` | `string` | SOCKS5 proxy endpoint used by this permissions profile. |
| `permissions.<name>.network.unix_sockets` | `table` | Unix socket allowlist overrides for sandboxed networking. Use socket paths as keys; `allow` adds a path, and `deny` rejects it. |
| `permissions.<name>.network.unix_sockets.<path>` | `allow \| deny` | Add an absolute Unix socket path to the effective allowlist with `allow`, or reject it with `deny`. Denied entries are omitted from the effective allowlist. |
| `permissions.<name>.workspace_roots` | `table` | Profile-defined workspace roots that receive `:workspace_roots` filesystem rules alongside the session's runtime workspace roots. |
| `permissions.<name>.workspace_roots.<path>` | `boolean` | Opt a path into the profile's workspace root set when `true`. Disabled entries remain inactive. |
| `personality` | `none \| friendly \| pragmatic` | Default communication style for models that advertise `supportsPersonality`; can be overridden per thread/turn or via `/personality`. |
| `plan_mode_reasoning_effort` | `none \| minimal \| low \| medium \| high \| xhigh` | Plan-mode-specific reasoning override. When unset, Plan mode uses its built-in preset default. |
| `plugins.<plugin>.mcp_servers.<server>.default_tools_approval_mode` | `auto \| prompt \| writes \| approve` | Default approval behavior for tools on a plugin-provided MCP server. |
| `plugins.<plugin>.mcp_servers.<server>.disabled_tools` | `array<string>` | Deny list applied after `enabled_tools` for a plugin-provided MCP server. |
| `plugins.<plugin>.mcp_servers.<server>.enabled` | `boolean` | Enable or disable an MCP server bundled by an installed plugin without changing the plugin manifest. |
| `plugins.<plugin>.mcp_servers.<server>.enabled_tools` | `array<string>` | Allow list of tools exposed from a plugin-provided MCP server. |
| `plugins.<plugin>.mcp_servers.<server>.tools.<tool>.approval_mode` | `auto \| prompt \| writes \| approve` | Per-tool approval behavior override for a plugin-provided MCP tool. |
| `project_doc_fallback_filenames` | `array<string>` | Additional filenames to try when `AGENTS.md` is missing. |
| `project_doc_max_bytes` | `number` | Maximum bytes read from `AGENTS.md` when building project instructions. |
| `project_root_markers` | `array<string>` | List of project root marker filenames; used when searching parent directories for the project root. |
| `projects.<path>.trust_level` | `string` | Mark a project or worktree as trusted or untrusted (`"trusted"` \| `"untrusted"`). Untrusted projects skip project-scoped `.codex/` layers, including project-local config, hooks, and rules. |
| `review_model` | `string` | Optional model override used by `/review` (defaults to the current session model). |
| `sandbox_mode` | `read-only \| workspace-write \| danger-full-access` | Sandbox policy for filesystem and network access during command execution. |
| `sandbox_workspace_write.exclude_slash_tmp` | `boolean` | Exclude `/tmp` from writable roots in workspace-write mode. |
| `sandbox_workspace_write.exclude_tmpdir_env_var` | `boolean` | Exclude `$TMPDIR` from writable roots in workspace-write mode. |
| `sandbox_workspace_write.network_access` | `boolean` | Allow outbound network access inside the workspace-write sandbox. |
| `sandbox_workspace_write.writable_roots` | `array<string>` | Additional writable roots when `sandbox_mode = "workspace-write"`. |
| `service_tier` | `string` | Preferred service tier for new turns. Built-in values include `flex` and `fast`; legacy `fast` config maps to the request value `priority`, and catalog-provided tier IDs can also be stored. |
| `shell_environment_policy.exclude` | `array<string>` | Glob patterns for removing environment variables after the defaults. |
| `shell_environment_policy.experimental_use_profile` | `boolean` | Use the user shell profile when spawning subprocesses. |
| `shell_environment_policy.ignore_default_excludes` | `boolean` | Keep variables containing KEY/SECRET/TOKEN before other filters run. |
| `shell_environment_policy.include_only` | `array<string>` | Whitelist of patterns; when set only matching variables are kept. |
| `shell_environment_policy.inherit` | `all \| core \| none` | Baseline environment inheritance when spawning subprocesses. |
| `shell_environment_policy.set` | `map<string,string>` | Explicit environment overrides injected into every subprocess. |
| `show_raw_agent_reasoning` | `boolean` | Surface raw reasoning content when the active model emits it. |
| `skills.config` | `array<object>` | Per-skill enablement overrides stored in config.toml. |
| `skills.config.<index>.enabled` | `boolean` | Enable or disable the referenced skill. |
| `skills.config.<index>.path` | `string (path)` | Path to a skill folder containing `SKILL.md`. |
| `sqlite_home` | `string (path)` | Directory where Codex stores the SQLite-backed state DB used by agent jobs and other resumable runtime state. |
| `suppress_unstable_features_warning` | `boolean` | Suppress the warning that appears when under-development feature flags are enabled. |
| `tool_output_token_limit` | `number` | Token budget for storing individual tool/function outputs in history. |
| `tool_suggest.disabled_tools` | `array<table>` | Disable suggestions for specific discoverable connectors or plugins. Each entry uses `type = "connector"` or `"plugin"` and an `id`. |
| `tool_suggest.discoverables` | `array<table>` | Allow tool suggestions for additional discoverable connectors or plugins. Each entry uses `type = "connector"` or `"plugin"` and an `id`. |
| `tools.view_image` | `boolean` | Enable the local-image attachment tool `view_image`. |
| `tools.web_search` | `boolean \| { context_size = "low\|medium\|high", allowed_domains = [string], location = { country, region, city, timezone } }` | Optional web search tool configuration. The legacy boolean form is still accepted, but the object form lets you set search context size, allowed domains, and approximate user location. |
| `tui` | `table` | TUI-specific options such as enabling inline desktop notifications. |
| `tui.alternate_screen` | `auto \| always \| never` | Control alternate screen usage for the TUI (default: auto; auto skips it in Zellij to preserve scrollback). |
| `tui.animations` | `boolean` | Enable terminal animations (welcome screen, shimmer, spinner) (default: true). |
| `tui.keymap.<context>.<action>` | `string \| array<string>` | Keyboard shortcut binding for a TUI action. Supported contexts include `global`, `chat`, `composer`, `editor`, `vim_normal`, `vim_operator`, `vim_text_object`, `pager`, `list`, and `approval`. Selected composer actions fall back to matching `tui.keymap.global` bindings; context-specific bindings take precedence when supported. |
| `tui.keymap.<context>.<action> = []` | `empty array` | Unbind the action in that keymap context. Key names use normalized strings such as `ctrl-a`, `shift-enter`, `page-down`, or `minus`. |
| `tui.model_availability_nux.<model>` | `integer` | Internal startup-tooltip state keyed by model slug. |
| `tui.notification_condition` | `unfocused \| always` | Control whether TUI notifications fire only when the terminal is unfocused or regardless of focus. Defaults to `unfocused`. |
| `tui.notification_method` | `auto \| osc9 \| bel` | Notification method for terminal notifications (default: auto). |
| `tui.notifications` | `boolean \| array<string>` | Enable TUI notifications; optionally restrict to specific event types. |
| `tui.raw_output_mode` | `boolean` | Start the TUI in raw scrollback mode for copy-friendly terminal selection (default: false). You can toggle it with `/raw` or the default `alt-r` key binding. |
| `tui.show_tooltips` | `boolean` | Show onboarding tooltips in the TUI welcome screen (default: true). |
| `tui.status_line` | `array<string> \| null` | Ordered list of TUI footer status-line item identifiers. `null` disables the status line. |
| `tui.terminal_title` | `array<string> \| null` | Ordered list of terminal window/tab title item identifiers. Defaults to `["spinner", "project"]`; `null` disables title updates. |
| `tui.theme` | `string` | Syntax-highlighting theme override (kebab-case theme name). |
| `tui.vim_mode_default` | `boolean` | Start the composer in Vim normal mode instead of insert mode (default: false). You can still toggle it per session with `/vim`. |
| `web_search` | `disabled \| cached \| indexed \| live` | Web search mode (default: `"cached"`; cached uses an OpenAI-maintained index without external web access; indexed permits external access only when gated by the search index; if you use `--yolo` or another full access sandbox setting, it defaults to `"live"`). Use `"live"` for unrestricted live retrieval, or `"disabled"` to remove the tool. |
| `windows_wsl_setup_acknowledged` | `boolean` | Track Windows onboarding acknowledgement (Windows only). |
| `windows.sandbox` | `unelevated \| elevated` | Windows-only native sandbox mode when running Codex natively on Windows. |
| `windows.sandbox_private_desktop` | `boolean` | Run the final sandboxed child process on a private desktop by default on native Windows. Set `false` only for compatibility with the older `Winsta0\\Default` behavior. |

You can find the latest JSON schema for `config.toml` [here](/codex/config-schema.json).

To get autocompletion and diagnostics when editing `config.toml` in VS Code or Cursor, you can install the [Even Better TOML](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml) extension and add this line to the top of your `config.toml`:

```toml
#:schema https://developers.openai.com/codex/config-schema.json
```

Note: Rename `experimental_instructions_file` to `model_instructions_file`. Codex deprecates the old key; update existing configs to the new name.

### `requirements.toml`

`requirements.toml` is an admin-enforced configuration file that constrains security-sensitive settings users can’t override. For details, locations, and examples, see [Admin-enforced requirements](/codex/enterprise/managed-configuration#admin-enforced-requirements-requirementstoml).

For ChatGPT Business and Enterprise users, Codex can also apply cloud-fetched
requirements. See the security page for precedence details.

Use `[features]` in `requirements.toml` to pin runtime feature flags by the same
canonical keys that `config.toml` uses. Requirements can also include documented
app-only keys that don’t belong in `config.toml`. Omitted keys remain
unconstrained.

Managed permission-profile allowlists require Codex 0.138.0 or later. Codex
0.137.0 and earlier ignore `allowed_permission_profiles` and managed
`default_permissions`.

Use `allowed_sandbox_modes` with `sandbox_mode`. For permission-profile
deployments, use `allowed_permission_profiles` with managed
`default_permissions`.

The `[models.new_thread]` table supplies managed defaults, not enforcement.
Explicit launch choices from dedicated CLI flags or `--config` overrides take
precedence. An explicit model or reasoning-effort override skips both managed
model fields; `service_tier` is independent.

| Key | Type / Values | Details |
| --- | --- | --- |
| `allow_appshots` | `boolean` | Set to `false` to disable Appshots for managed users. If omitted, Appshots remain unconstrained by requirements and follow normal product availability. |
| `allow_managed_hooks_only` | `boolean` | When `true`, Codex skips user, project, session, and plugin hooks while still allowing managed hooks from `requirements.toml` and other managed config layers. |
| `allow_remote_control` | `boolean` | Set to `false` to disable device remote control for managed users. If omitted, device remote control remains unconstrained by requirements and follows normal product availability. |
| `allowed_approval_policies` | `array<string>` | Allowed values for `approval_policy` (for example `untrusted`, `on-request`, `never`, and `granular`). |
| `allowed_approvals_reviewers` | `array<string>` | Allowed values for `approvals_reviewer`, such as `user` and `auto_review`. |
| `allowed_permission_profiles` | `table<boolean>` | Complete list of allowed permission profiles. Profiles set to `true` are allowed. Profiles that are omitted or set to `false` are denied, including profiles added in future versions. When requirements sources are combined, entries are matched by profile name. |
| `allowed_permission_profiles.<name>` | `boolean` | Allow or deny a built-in or custom permission profile defined in a loaded config or requirements source. A later, higher-precedence requirements source can use `false` to turn off a profile allowed by an earlier, lower-precedence source. |
| `allowed_sandbox_modes` | `array<string>` | Allowed values for `sandbox_mode`. |
| `allowed_web_search_modes` | `array<string>` | Allowed values for `web_search` (`disabled`, `cached`, `indexed`, `live`). `disabled` is always allowed; an empty list effectively allows only `disabled`. |
| `apps` | `table` | Managed app requirements keyed by app identifier. Requirements can disable an app or constrain approval behavior for individual tools. |
| `apps.<id>.enabled` | `boolean` | Set to `false` to disable an app. A disabled requirement remains restrictive when multiple requirements sources are merged. |
| `apps.<id>.tools.<tool>.approval_mode` | `auto \| prompt \| writes \| approve` | Set the managed approval mode for one app tool. |
| `computer_use` | `table` | Computer Use requirements enforced from `requirements.toml`. |
| `computer_use.allow_locked_computer_use` | `boolean` | Set to `false` to prevent Computer Use from operating after a managed macOS device locks. If omitted, locked use remains unconstrained by requirements. |
| `default_permissions` | `string` | Managed default permission profile. The profile must be allowed by `allowed_permission_profiles`. Set this explicitly for predictable behavior; if omitted, Codex defaults to `:workspace` only when both `:workspace` and `:read-only` are explicitly allowed. |
| `enforce_residency` | `string` | Require Codex service traffic to use a supported data residency. Currently accepts `us`. |
| `experimental_network` | `table` | Network access requirements enforced from `requirements.toml`. These constraints are separate from `features.network_proxy` and can configure sandboxed networking without the user feature flag. |
| `experimental_network.allow_local_binding` | `boolean` | Permit broader local/private-network access for sandboxed networking. Exact local IP literal or `localhost` allow rules can still permit specific local targets when this stays `false`. |
| `experimental_network.allow_upstream_proxy` | `boolean` | Allow sandboxed networking to chain through an upstream proxy from the environment. |
| `experimental_network.allowed_domains` | `array<string>` | List-shaped administrator allow rules for sandboxed networking. Do not combine this with `experimental_network.domains`. |
| `experimental_network.dangerously_allow_all_unix_sockets` | `boolean` | Permit arbitrary Unix socket destinations instead of allowlist-only access. Use only in tightly controlled environments. |
| `experimental_network.dangerously_allow_non_loopback_proxy` | `boolean` | Permit non-loopback listener addresses for `[experimental_network]` requirements. Enabling it can expose listeners beyond localhost. |
| `experimental_network.denied_domains` | `array<string>` | List-shaped administrator deny rules for sandboxed networking. Do not combine this with `experimental_network.domains`. |
| `experimental_network.domains` | `map<string, allow \| deny>` | Map-shaped administrator domain policy for sandboxed networking. Supports exact hosts, `*.example.com` for subdomains only, `**.example.com` for apex plus subdomains, and global `*` allow rules; prefer scoped rules because `*` broadly opens public outbound access. `deny` wins on conflicts. Do not combine this with `experimental_network.allowed_domains` or `experimental_network.denied_domains`. |
| `experimental_network.enabled` | `boolean` | Enable sandboxed networking requirements. This does not grant network access when the active sandbox keeps command networking off. |
| `experimental_network.http_port` | `integer` | Loopback HTTP listener port to use for `[experimental_network]` requirements. |
| `experimental_network.managed_allowed_domains_only` | `boolean` | When `true`, only administrator-managed allow rules remain effective while sandboxed networking requirements are active; user allowlist additions are ignored. Without managed allow rules, user-added domain allow rules do not remain effective. |
| `experimental_network.socks_port` | `integer` | Loopback SOCKS5 listener port to use for `[experimental_network]` requirements. |
| `experimental_network.unix_sockets` | `map<string, allow \| deny>` | Administrator-managed Unix socket policy for sandboxed networking. |
| `features` | `table` | Pinned feature values. Use canonical names from `config.toml` for runtime features; documented app-only requirement keys are also supported here. |
| `features.<name>` | `boolean` | Require a documented runtime or app feature to stay enabled or disabled. |
| `features.apps` | `boolean` | Pin Apps integration availability on or off for managed users. |
| `features.browser_use` | `boolean` | Set to `false` in `requirements.toml` to disable Computer Use in browsers and Browser Agent availability. |
| `features.browser_use_external` | `boolean` | Set to `false` in `requirements.toml` to disable Computer Use in external browsers. |
| `features.browser_use_full_cdp_access` | `boolean` | Set to `false` in `requirements.toml` to disable full Chrome DevTools Protocol access in the local runtime, including Browser Developer mode, and prevent the ChatGPT desktop app from enabling the corresponding setting. If omitted, normal product availability applies. |
| `features.computer_use` | `boolean` | Set to `false` in `requirements.toml` to disable Computer Use, Record & Replay, and related install or enablement flows. |
| `features.fast_mode` | `boolean` | Pin the canonical `fast_mode` feature on or off for managed users. |
| `features.guardian_approval` | `boolean` | Pin Guardian approval availability on or off for managed users. |
| `features.in_app_browser` | `boolean` | Set to `false` in `requirements.toml` to disable the built-in browser pane. |
| `features.memories` | `boolean` | Pin Memories availability on or off for managed users. |
| `features.multi_agent` | `boolean` | Pin multi-agent availability on or off for managed users. |
| `features.plugin_sharing` | `boolean` | Set to `false` in cloud-managed `requirements.toml` to disable workspace sharing for locally built plugins. |
| `features.plugins` | `boolean` | Pin plugin availability on or off for managed users. |
| `features.remote_plugin` | `boolean` | Pin remote plugin catalog availability on or off for managed users. |
| `features.workspace_dependencies` | `boolean` | Pin bundled workspace-dependency runtime availability on or off for managed users. |
| `guardian_policy_config` | `string` | Managed Markdown policy instructions for automatic review. This takes precedence over local `[auto_review].policy`. Blank values are ignored. |
| `hooks` | `table` | Admin-enforced managed lifecycle hooks. Requires a managed hook directory and uses the same event schema as inline `[hooks]` in `config.toml`. |
| `hooks.<Event>` | `array<table>` | Matcher groups for a hook event such as `PreToolUse`, `PermissionRequest`, `PostToolUse`, `PreCompact`, `PostCompact`, `SessionStart`, `SubagentStart`, `SubagentStop`, `UserPromptSubmit`, or `Stop`. |
| `hooks.<Event>[].hooks` | `array<table>` | Hook handlers for a matcher group. Command hooks are currently supported; prompt and agent hook handlers are parsed but skipped. |
| `hooks.<Event>[].hooks[].commandWindows` | `string` | Windows-only command override for command hooks. The TOML alias `command_windows` is also accepted. |
| `hooks.managed_dir` | `string (absolute path)` | Directory containing managed hook scripts on macOS and Linux. Codex validates that it is absolute and exists before loading managed hooks. |
| `hooks.windows_managed_dir` | `string (absolute path)` | Directory containing managed hook scripts on Windows. Codex validates that it is absolute and exists before loading managed hooks. |
| `marketplaces` | `table` | Admin requirements for plugin marketplace sources. Rules take effect when `restrict_to_allowed_sources` is `true`. |
| `marketplaces.allowed_sources` | `table` | Allowed marketplace sources keyed by administrator-chosen rule name. Distinct names accumulate across requirements layers; fields under the same name use normal layer precedence. |
| `marketplaces.allowed_sources.<name>` | `table` | One allowed source rule. The final `source` value after requirements merge determines which sibling fields Codex interprets. |
| `marketplaces.allowed_sources.<name>.host_pattern` | `string` | Regular expression required when `source = "host_pattern"`. Codex matches it against the lowercase hostname parsed from an HTTPS, SSH, or SCP-style Git source. Use `^` and `$` to require a whole-host match. |
| `marketplaces.allowed_sources.<name>.path` | `string (absolute path)` | Local marketplace directory required when `source = "local"`. Codex requires an absolute path and compares paths after normalization. |
| `marketplaces.allowed_sources.<name>.ref` | `string` | Optional exact Git ref for a `git` rule. When omitted, the rule allows any ref for the matching repository. |
| `marketplaces.allowed_sources.<name>.source` | `git \| host_pattern \| local` | Marketplace source matcher type. Use `git` for one repository, `host_pattern` for Git hosts matched by regular expression, or `local` for one directory. |
| `marketplaces.allowed_sources.<name>.url` | `string` | Git repository URL required when `source = "git"`. Codex normalizes the configured and allowed URLs before requiring an exact repository match. |
| `marketplaces.restrict_to_allowed_sources` | `boolean` | When `true`, require user-configured marketplace sources to match `allowed_sources` for marketplace add, plugin install, and configured Git marketplace refresh operations. Codex-managed OpenAI marketplaces remain allowed when their reserved source and name match. This doesn't filter already configured user marketplaces at runtime. |
| `mcp_servers` | `table` | Allowlist of MCP servers that may be enabled. Both the server name (`<id>`) and its identity must match for the MCP server to be enabled. Any configured MCP server not in the allowlist (or with a mismatched identity) is disabled. |
| `mcp_servers.<id>.identity` | `table` | Identity rule for a single MCP server. Set either `command` (stdio) or `url` (streamable HTTP). |
| `mcp_servers.<id>.identity.command` | `string \| table` | Allow an MCP stdio server by exact command string, or use a matcher table to require an exact executable and ordered argument matchers. The string form doesn't inspect arguments, `cwd`, `env`, or `env_vars`. |
| `mcp_servers.<id>.identity.command.args` | `array<table>` | Ordered argument matchers for a stdio server. The configured argument list must have the same length, and every position must match. Command matchers don't inspect `cwd`, `env`, or `env_vars`. |
| `mcp_servers.<id>.identity.command.args[].expression` | `string` | Regular expression used by a `regex` argument matcher. The expression must be valid and match the complete argument value. |
| `mcp_servers.<id>.identity.command.args[].match` | `exact \| prefix \| regex` | Match operation for this argument position. |
| `mcp_servers.<id>.identity.command.args[].value` | `string` | Value used by an `exact` or `prefix` argument matcher. |
| `mcp_servers.<id>.identity.command.executable` | `string` | Executable that the stdio server's configured `command` must match exactly. |
| `mcp_servers.<id>.identity.url` | `string \| table` | Allow an MCP streamable HTTP server by exact URL string, or use an `exact`, `prefix`, or `regex` value matcher table. |
| `mcp_servers.<id>.identity.url.expression` | `string` | Regular expression used by a `regex` URL matcher. The expression must be valid and match the complete URL value. |
| `mcp_servers.<id>.identity.url.match` | `exact \| prefix \| regex` | Match operation for the configured MCP server URL. |
| `mcp_servers.<id>.identity.url.value` | `string` | Value used by an `exact` or `prefix` URL matcher. |
| `models` | `table` | Managed model defaults for new threads. These values take priority over user and project defaults, but an explicit selection for the new thread can override them. |
| `models.new_thread` | `table` | Defaults to apply when a new local thread starts. Each model setting is optional. |
| `models.new_thread.model` | `string` | Default model for new threads. An explicit `--model` or model/reasoning `--config` override takes precedence. |
| `models.new_thread.model_reasoning_effort` | `string` | Default reasoning effort for new threads. An explicit model or reasoning-effort override skips both managed model fields. |
| `models.new_thread.service_tier` | `string` | Default service tier for new threads. An explicit service-tier override takes precedence independently of the model fields. |
| `permissions` | `table` | Admin-defined permission profiles keyed by profile name. Uses the same profile fields as `config.toml`. |
| `permissions.<name>` | `table` | Admin-defined permission profile. The name can't start with `:`, use the reserved name `filesystem`, or duplicate a profile from a loaded config. Uses the same profile fields as `config.toml`; see the Permissions guide for the complete profile schema. |
| `permissions.filesystem.deny_read` | `array<string>` | Admin-enforced filesystem read denials. Entries can be paths or glob patterns, and users cannot weaken them with local config. |
| `plugins` | `table` | Plugin-specific MCP server allowlists keyed by plugin identifier. When this table is present, plugin-bundled servers without a matching plugin and server entry are disabled. |
| `plugins.<plugin>.mcp_servers` | `table` | Allowlist for MCP servers bundled with one plugin. Plugin server requirements use the same exact identity and matcher forms as top-level `mcp_servers` requirements. |
| `plugins.<plugin>.mcp_servers.<server>.identity` | `table` | Identity rule for one plugin-bundled MCP server. Set either `command` (stdio) or `url` (streamable HTTP). |
| `plugins.<plugin>.mcp_servers.<server>.identity.command` | `string \| table` | Allow a plugin's stdio MCP server by exact command string, or use a matcher table to require an exact executable and ordered argument matchers. |
| `plugins.<plugin>.mcp_servers.<server>.identity.command.args` | `array<table>` | Ordered argument matchers for a plugin-bundled stdio server. The configured argument list must have the same length, and every position must match. |
| `plugins.<plugin>.mcp_servers.<server>.identity.command.args[].expression` | `string` | Regular expression used by a `regex` argument matcher. The expression must match the complete argument value. |
| `plugins.<plugin>.mcp_servers.<server>.identity.command.args[].match` | `exact \| prefix \| regex` | Match operation for this argument position. |
| `plugins.<plugin>.mcp_servers.<server>.identity.command.args[].value` | `string` | Value used by an `exact` or `prefix` argument matcher. |
| `plugins.<plugin>.mcp_servers.<server>.identity.command.executable` | `string` | Executable that the plugin-bundled stdio server's configured command must match exactly. |
| `plugins.<plugin>.mcp_servers.<server>.identity.url` | `string \| table` | Allow a plugin's streamable HTTP MCP server by exact URL string, or use an `exact`, `prefix`, or `regex` value matcher table. |
| `plugins.<plugin>.mcp_servers.<server>.identity.url.expression` | `string` | Regular expression used by a `regex` URL matcher. The expression must match the complete URL value. |
| `plugins.<plugin>.mcp_servers.<server>.identity.url.match` | `exact \| prefix \| regex` | Match operation for the plugin-bundled MCP server URL. |
| `plugins.<plugin>.mcp_servers.<server>.identity.url.value` | `string` | Value used by an `exact` or `prefix` URL matcher. |
| `remote_sandbox_config` | `array<table>` | Host-specific sandbox requirements. The first entry whose `hostname_patterns` match the resolved host name overrides top-level `allowed_sandbox_modes` for that requirements source. Host-specific entries currently override sandbox modes only. |
| `remote_sandbox_config[].allowed_sandbox_modes` | `array<string>` | Allowed sandbox modes to apply when this host-specific entry matches. |
| `remote_sandbox_config[].hostname_patterns` | `array<string>` | Case-insensitive host name patterns. Supports `*` for any sequence of characters and `?` for one character. |
| `rules` | `table` | Admin-enforced command rules merged with `.rules` files. Requirements rules must be restrictive. |
| `rules.prefix_rules` | `array<table>` | List of enforced prefix rules. Each rule must include `pattern` and `decision`. |
| `rules.prefix_rules[].decision` | `prompt \| forbidden` | Required. Requirements rules can only prompt or forbid (not allow). |
| `rules.prefix_rules[].justification` | `string` | Optional non-empty rationale surfaced in approval prompts or rejection messages. |
| `rules.prefix_rules[].pattern` | `array<table>` | Command prefix expressed as pattern tokens. Each token sets either `token` or `any_of`. |
| `rules.prefix_rules[].pattern[].any_of` | `array<string>` | A list of allowed alternative tokens at this position. |
| `rules.prefix_rules[].pattern[].token` | `string` | A single literal token at this position. |
| `windows` | `table` | Native Windows sandbox requirements. |
| `windows.allowed_sandbox_implementations` | `array<string>` | Allowed native Windows sandbox implementations for `windows.sandbox` (`elevated` and `unelevated`). The list must not be empty. When both are allowed and no mode is selected, Codex prefers `elevated`. |

---

## Environment variables

Environment variables supported by Codex

Codex uses `config.toml` for durable settings. Use environment variables for
shell-scoped overrides, automation secrets, installer behavior, or diagnostics.

This page lists stable public environment variables that Codex reads directly.
It does not list internal development variables, test variables, or
provider-specific secret names you choose yourself with
[`env_key`](/codex/config-file/config-advanced#custom-model-providers).

### Core locations

| Variable | Used by | Default | Description |
| --- | --- | --- | --- |
| `CODEX_HOME` | CLI, IDE extension, app-server, installers | `~/.codex` | Sets the root for Codex state, including config, auth, logs, sessions, skills, and standalone package metadata. If you set it, the directory must already exist. |
| `CODEX_SQLITE_HOME` | CLI and app-server state | `CODEX_HOME` | Sets where SQLite-backed state is stored. The `sqlite_home` config option takes precedence. Relative paths resolve from the current working directory. |

For more about the files stored under `CODEX_HOME`, see
[Config and state locations](/codex/config-file/config-advanced#config-and-state-locations).

### Installer variables

These variables apply to the standalone install scripts served from
`https://chatgpt.com/codex/install.sh` and
`https://chatgpt.com/codex/install.ps1`.

| Variable | Default | Description |
| --- | --- | --- |
| `CODEX_NON_INTERACTIVE` | `false` | Set to `1`, `true`, or `yes` to skip installer prompts. Prompts use their default response, so use this for scripted installs and updates, not first-run setup. |
| `CODEX_INSTALL_DIR` | `~/.local/bin` on macOS/Linux; `%LOCALAPPDATA%\Programs\OpenAI\Codex\bin` on Windows | Changes where the visible `codex` command is installed. The standalone package cache still lives under `CODEX_HOME/packages/standalone`. |

For unattended installs, set `CODEX_NON_INTERACTIVE=1` on the shell that runs
the downloaded installer:

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | CODEX_NON_INTERACTIVE=1 sh
```

```powershell
$env:CODEX_NON_INTERACTIVE=1; irm https://chatgpt.com/codex/install.ps1 | iex
```

### Authentication and network

| Variable | Used by | Description |
| --- | --- | --- |
| `CODEX_API_KEY` | `codex exec` | Provides an API key for a single non-interactive run. This is only supported in `codex exec`; set it inline rather than job-wide when running repository-controlled code. |
| `CODEX_ACCESS_TOKEN` | CLI, app-server, trusted automation | Provides a ChatGPT or Codex access token for trusted automation. For persisted login, pipe it to `codex login --with-access-token`. |
| `CODEX_CA_CERTIFICATE` | HTTPS, login, and WebSocket clients | Points to a PEM CA bundle for environments with corporate TLS interception or private root CAs. Takes precedence over `SSL_CERT_FILE`. |
| `SSL_CERT_FILE` | HTTPS, login, and WebSocket clients | Fallback PEM CA bundle path when `CODEX_CA_CERTIFICATE` is unset. |

For provider API keys, set
[`env_key`](/codex/config-file/config-advanced#custom-model-providers) in the model provider
configuration. Codex reads the variable named by that config, so the variable
name itself is not a fixed Codex environment variable.

For automation secret handling, see
[Use API key auth](/codex/non-interactive-mode#use-api-key-auth).
For access token setup, see [Access tokens](/codex/enterprise/access-tokens).

### Diagnostics

| Variable | Used by | Description |
| --- | --- | --- |
| `RUST_LOG` | CLI and app-server | Controls Rust log filtering and verbosity. `codex exec` defaults to `error` output unless you set a more verbose value. |

`RUST_LOG` accepts values such as `error`, `warn`, `info`, `debug`, and
`trace`. It also accepts more targeted Rust logging filters, such as
`codex_core=debug,codex_tui=debug`.

The interactive CLI records diagnostics in bounded local stores by default, but
the plaintext `codex-tui.log` file is opt-in. Set `log_dir` explicitly when you
need a plaintext log for troubleshooting:

```bash
RUST_LOG=debug codex -c log_dir=./.codex-log
tail -F ./.codex-log/codex-tui.log
```

In non-interactive mode, `codex exec` prints messages inline instead of writing
to a separate TUI log file.

---

## Sample Configuration

A complete example config.toml you can copy and adapt

Use this example configuration as a starting point. It includes most keys Codex reads from `config.toml`, along with default behaviors, recommended values where helpful, and short notes.

For explanations and guidance, see:

- [Config basics](/codex/config-file/config-basic)
- [Advanced Config](/codex/config-file/config-advanced)
- [Config Reference](/codex/config-file/config-reference)
- [Sandbox and approvals](/codex/agent-approvals-security#sandbox-and-approvals)
- [Managed configuration](/codex/enterprise/managed-configuration)

Use the snippet below as a reference. Copy only the keys and sections you need into `~/.codex/config.toml` (or into a project-scoped `.codex/config.toml`), then adjust values for your setup.

```toml
# Codex example configuration (config.toml)
#
# This file lists the main keys Codex reads from config.toml, along with default
# behaviors, recommended examples, and concise explanations. Adjust as needed.
#
# Notes
# - Root keys must appear before tables in TOML.
# - Optional keys that default to "unset" are shown commented out with notes.
# - MCP servers, profile files, and model providers are examples; remove or edit.

################################################################################

# Core Model Selection

################################################################################

# Primary model used by Codex. Recommended example for most users: "gpt-5.6".

model = "gpt-5.6"

# Communication style for supported models. Allowed values: none | friendly | pragmatic

# personality = "pragmatic"

# Optional model override for /review. Default: unset (uses current session model).

# review_model = "gpt-5.6"

# Provider id selected from [model_providers]. Default: "openai".

model_provider = "openai"

# Default OSS provider for --oss sessions. When unset, Codex prompts. Default: unset.

# oss_provider = "ollama"

# Preferred service tier. Built-in examples: fast | flex; model catalogs can add more.

# service_tier = "flex"

# Optional manual model metadata. When unset, Codex uses model or preset defaults.

# model_context_window = 128000 # tokens; default: auto for model

# model_auto_compact_token_limit = 64000 # tokens; unset uses model defaults

# model_auto_compact_token_limit_scope = "total" # total | body_after_prefix; default: total

# tool_output_token_limit = 12000 # tokens stored per tool output

# model_catalog_json = "/absolute/path/to/models.json" # optional startup-only model catalog override

# background_terminal_max_timeout = 300000 # ms; max empty write_stdin poll window (default 5m)

# log_dir = "/absolute/path/to/codex-logs" # log directory; setting explicitly enables codex-tui.log; default: "$CODEX_HOME/log"

# sqlite_home = "/absolute/path/to/codex-state" # optional SQLite-backed runtime state directory

################################################################################

# Reasoning & Verbosity (Responses API capable models)

################################################################################

# Reasoning effort: minimal | low | medium | high | xhigh

# model_reasoning_effort = "medium"

# Optional override used when Codex runs in plan mode: none | minimal | low | medium | high | xhigh

# plan_mode_reasoning_effort = "high"

# Reasoning summary: auto | concise | detailed | none

# model_reasoning_summary = "auto"

# Text verbosity for GPT-5 family (Responses API): low | medium | high

# model_verbosity = "medium"

# Force enable or disable reasoning summaries for current model.

# model_supports_reasoning_summaries = true

################################################################################

# Instruction Overrides

################################################################################

# Additional user instructions are injected before AGENTS.md. Default: unset.

# developer_instructions = ""

# Inline override for the history compaction prompt. Default: unset.

# compact_prompt = ""

# Override built-in base instructions with a file path. Default: unset.

# model_instructions_file = "/absolute/or/relative/path/to/instructions.txt"

# Load the compact prompt override from a file. Default: unset.

# experimental_compact_prompt_file = "/absolute/or/relative/path/to/compact_prompt.txt"

################################################################################

# Notifications

################################################################################

# External notifier program (argv array). When unset: disabled.

# notify = ["notify-send", "Codex"]

################################################################################

# Approval & Sandbox

################################################################################

# When to ask for command approval:

# - untrusted: only known-safe read-only commands auto-run; others prompt

# - on-request: model decides when to ask (default)

# - never: never prompt (risky)

# - { granular = { ... } }: allow or auto-reject selected prompt categories

approval_policy = "on-request"

# Who reviews eligible approval prompts: user (default) | auto_review

# approvals_reviewer = "user"

# Example granular policy:

# approval_policy = { granular = {

# sandbox_approval = true,

# rules = true,

# mcp_elicitations = true,

# request_permissions = false,

# skill_approval = false

# } }

# Allow login-shell semantics for shell-based tools when they request `login = true`.

# Default: true. Set false to force non-login shells and reject explicit login-shell requests.

allow_login_shell = true

# Filesystem/network sandbox policy for tool calls:

# - read-only (default)

# - workspace-write

# - danger-full-access (no sandbox; extremely risky)

sandbox_mode = "read-only"

# Named permissions profile to apply by default. Built-ins:

# :read-only | :workspace | :danger-full-access

# Use a custom name such as "workspace" only when you also define [permissions.workspace].

# default_permissions = ":workspace"

################################################################################

# Authentication & Login

################################################################################

# Where to persist CLI login credentials: file (default) | keyring | auto

cli_auth_credentials_store = "file"

# Base URL for ChatGPT auth flow (not OpenAI API).

chatgpt_base_url = "https://chatgpt.com/backend-api/"

# Optional base URL override for the built-in OpenAI provider.

# openai_base_url = "https://us.api.openai.com/v1"

# Restrict ChatGPT login to a specific workspace id. Default: unset.

# forced_chatgpt_workspace_id = "00000000-0000-0000-0000-000000000000"

# Force login mechanism when Codex would normally auto-select. Default: unset.

# Allowed values: chatgpt | api

# forced_login_method = "chatgpt"

# Preferred store for MCP OAuth credentials: auto (default) | file | keyring

mcp_oauth_credentials_store = "auto"

# Optional fixed port for MCP OAuth callback: 1-65535. Default: unset.

# mcp_oauth_callback_port = 4321

# Optional redirect URI override for MCP OAuth login (for example, remote devbox ingress).

# Codex appends a server-specific callback ID before OAuth login.

# Register the full derived URI with your provider, not just the base host or unsuffixed path.

# Custom callback paths are supported. `mcp_oauth_callback_port` still controls the listener port.

# mcp_oauth_callback_url = "https://devbox.example.internal/callback"

################################################################################

# Project Documentation Controls

################################################################################

# Max bytes from AGENTS.md to embed into first-turn instructions. Default: 32768

project_doc_max_bytes = 32768

# Ordered fallbacks when AGENTS.md is missing at a directory level. Default: []

project_doc_fallback_filenames = []

# Project root marker filenames used when searching parent directories. Default: [".git"]

# project_root_markers = [".git"]

################################################################################

# History & File Opener

################################################################################

# URI scheme for clickable citations: vscode (default) | vscode-insiders | windsurf | cursor | none

file_opener = "vscode"

################################################################################

# UI, Notifications, and Misc

################################################################################

# Suppress internal reasoning events from output. Default: false

hide_agent_reasoning = false

# Show raw reasoning content when available. Default: false

show_raw_agent_reasoning = false

# Disable burst-paste detection in the TUI. Default: false

disable_paste_burst = false

# Track Windows onboarding acknowledgement (Windows only). Default: false

windows_wsl_setup_acknowledged = false

# Check for updates on startup. Default: true

check_for_update_on_startup = true

################################################################################

# Web Search

################################################################################

# Web search mode: disabled | cached | indexed | live. Default: "cached"

# cached serves results from a web search cache (an OpenAI-maintained index).

# cached returns pre-indexed results; indexed gates external web access through

# the search index; live fetches the most recent data.

# If you use --yolo or another full access sandbox setting, web search defaults to live.

web_search = "cached"

# Config profiles are separate files under CODEX_HOME.

# Example: ~/.codex/ci.config.toml, selected with codex --profile ci.

# Suppress the warning shown when under-development feature flags are enabled.

# suppress_unstable_features_warning = true

################################################################################

# Agents (multi-agent roles and limits)

################################################################################

[agents]

# Maximum concurrently open agent threads. Default: 6

# max_threads = 6

# Maximum nested spawn depth. Root session starts at depth 0. Default: 1

# max_depth = 1

# Default timeout per worker for spawn_agents_on_csv jobs. When unset, the tool defaults to 1800 seconds.

# job_max_runtime_seconds = 1800

# Record a model-visible message when an agent turn is interrupted. Default: true

# interrupt_message = true

# [agents.reviewer]

# description = "Find correctness, security, and test risks in code."

# config_file = "./agents/reviewer.toml" # relative to the config.toml that defines it

# nickname_candidates = ["Athena", "Ada"]

################################################################################

# Skills (per-skill overrides)

################################################################################

# Disable or re-enable a specific skill without deleting it.

[[skills.config]]

# path = "/path/to/skill/SKILL.md"

# enabled = false

################################################################################

# Sandbox settings (tables)

################################################################################

# Extra settings used only when sandbox_mode = "workspace-write".

[sandbox_workspace_write]

# Additional writable roots beyond the workspace (cwd). Default: []

writable_roots = []

# Allow outbound network access inside the sandbox. Default: false

network_access = false

# Exclude $TMPDIR from writable roots. Default: false

exclude_tmpdir_env_var = false

# Exclude /tmp from writable roots. Default: false

exclude_slash_tmp = false

################################################################################

# Shell Environment Policy for spawned processes (table)

################################################################################

[shell_environment_policy]

# inherit: all (default) | core | none

inherit = "all"

# Skip default excludes for names containing KEY/SECRET/TOKEN (case-insensitive). Default: false

ignore_default_excludes = false

# Case-insensitive glob patterns to remove (e.g., "AWS**", "AZURE**"). Default: []

exclude = []

# Explicit key/value overrides (always win). Default: {}

set = {}

# Whitelist; if non-empty, keep only matching vars. Default: []

include_only = []

# Experimental: run via user shell profile. Default: false

experimental_use_profile = false

################################################################################

# Sandboxed networking settings

################################################################################

# Enable the feature before configuring sandboxed networking rules.

# [features.network_proxy]

# enabled = true

# domains = { "api.openai.com" = "allow", "example.com" = "deny" }

#

# Exact hosts match only themselves.

# "*.example.com" matches subdomains only; "**.example.com" matches the apex plus subdomains.

# "*" allows any public host that is not denied, so prefer scoped rules when possible.

# `allow_local_binding = false` blocks loopback and private destinations by default.

# Add an exact local IP literal or `localhost` allow rule for one target, or set it to true only when broader local access is required.

#

# Set `default_permissions = "workspace"` before enabling this profile.

# Example additional workspace roots that inherit this profile's

# `:workspace_roots` filesystem rules.

# [permissions.workspace.workspace_roots]

# "~/code/app" = true

# "~/code/shared-lib" = true

#

# Example filesystem profile. Use `"deny"` to deny reads for exact paths or

# glob patterns. On platforms that need pre-expanded glob matches, set

# glob_scan_max_depth when using unbounded patterns such as `**`.

# [permissions.workspace.filesystem]

# glob_scan_max_depth = 3

# ":workspace_roots" = { "." = "write", "**/*.env" = "deny" }

# "/absolute/path/to/secrets" = "deny"

#

# [permissions.workspace.network]

# enabled = true

# proxy_url = "http://127.0.0.1:43128"

# admin_url = "http://127.0.0.1:43129"

# enable_socks5 = false

# socks_url = "http://127.0.0.1:43130"

# enable_socks5_udp = false

# allow_upstream_proxy = false

# dangerously_allow_non_loopback_proxy = false

# dangerously_allow_non_loopback_admin = false

# dangerously_allow_all_unix_sockets = false

# mode = "limited" # limited | full

# allow_local_binding = false

#

# [permissions.workspace.network.domains]

# "api.openai.com" = "allow"

# "example.com" = "deny"

#

# [permissions.workspace.network.unix_sockets]

# "/var/run/docker.sock" = "allow"

################################################################################

# History (table)

################################################################################

[history]

# save-all (default) | none

persistence = "save-all"

# Maximum bytes for history file; oldest entries are trimmed when exceeded. Example: 5242880

# max_bytes = 5242880

################################################################################

# UI, Notifications, and Misc (tables)

################################################################################

[tui]

# Desktop notifications from the TUI: boolean or filtered list. Default: true

# Examples: false | ["agent-turn-complete", "approval-requested"]

notifications = false

# Notification mechanism for terminal alerts: auto | osc9 | bel. Default: "auto"

# notification_method = "auto"

# When notifications fire: unfocused (default) | always

# notification_condition = "unfocused"

# Enables welcome/status/spinner animations. Default: true

animations = true

# Show onboarding tooltips in the welcome screen. Default: true

show_tooltips = true

# Control alternate screen usage (auto skips it in Zellij to preserve scrollback).

# alternate_screen = "auto"

# Ordered list of footer status-line item IDs. When unset, Codex uses:

# ["model-with-reasoning", "context-remaining", "current-dir"].

# Set to [] to hide the footer.

# status_line = ["model", "context-remaining", "git-branch"]

# Ordered list of terminal window/tab title item IDs. When unset, Codex uses:

# ["spinner", "project"]. Set to [] to clear the title.

# Available IDs include app-name, project, spinner, status, thread, git-branch, model,

# and task-progress.

# terminal_title = ["spinner", "project"]

# Syntax-highlighting theme (kebab-case). Use /theme in the TUI to preview and save.

# You can also add custom .tmTheme files under $CODEX_HOME/themes.

# theme = "catppuccin-mocha"

# Custom key bindings. Selected composer actions fall back to matching [tui.keymap.global] bindings.

# Use [] to unbind an action.

# [tui.keymap.global]

# open_transcript = "ctrl-t"

# open_external_editor = []

#

# [tui.keymap.composer]

# submit = ["enter", "ctrl-m"]

# [tui.keymap.chat]

# interrupt_turn = "f12"

# Internal tooltip state keyed by model slug. Usually managed by Codex.

# [tui.model_availability_nux]

# "gpt-5.4" = 1

# Enable or disable analytics for this machine. When unset, Codex uses its default behavior.

[analytics]
enabled = true

# Control whether users can submit feedback from `/feedback`. Default: true

[feedback]
enabled = true

# In-product notices (mostly set automatically by Codex).

[notice]

# hide_full_access_warning = true

# hide_world_writable_warning = true

# hide_rate_limit_model_nudge = true

# hide_gpt5_1_migration_prompt = true

# "hide_gpt-5.1-codex-max_migration_prompt" = true

# model_migrations = { "gpt-5.3-codex" = "gpt-5.4" }

################################################################################

# Centralized Feature Flags (preferred)

################################################################################

[features]

# Leave this table empty to accept defaults. Set explicit booleans to opt in/out.

# shell_tool = true

# apps = true

# hooks = false

# unified_exec = true

# shell_snapshot = true

# multi_agent = true

# remote_plugin = true

# personality = true

# network_proxy = false

# fast_mode = true

# enable_request_compression = true

# skill_mcp_dependency_install = true

# prevent_idle_sleep = false

# Code mode namespaces. This feature is under development and off by default.

# [features.code_mode]

# enabled = true

# excluded_tool_namespaces = ["mcp__codex_apps"]

# direct_only_tool_namespaces = ["mcp__history"]

# Rollout budget tracking. This feature is under development and off by default.

# limit_tokens is required when enabled.

# Optional reminder_interval_tokens defaults to 10% of limit_tokens.

# Token weights default to 1.0.

# [features.rollout_budget]

# enabled = true

# limit_tokens = 100000

# reminder_interval_tokens = 10000

# sampling_token_weight = 1.0

# prefill_token_weight = 1.0

################################################################################

# Memories (table)

################################################################################

# Enable memories with [features].memories, then tune memory behavior here.

# [memories]

# generate_memories = true

# use_memories = true

# disable_on_external_context = false # legacy alias: no_memories_if_mcp_or_web_search

################################################################################

# Lifecycle hooks can be configured here inline or in a sibling hooks.json.

################################################################################

# [hooks]

# [[hooks.PreToolUse]]

# matcher = "^Bash$"

#

# [[hooks.PreToolUse.hooks]]

# type = "command"

# command = 'python3 "/absolute/path/to/pre_tool_use_policy.py"'

# timeout = 30

# statusMessage = "Checking Bash command"

################################################################################

# Define MCP servers under this table. Leave empty to disable.

################################################################################

[mcp_servers]

# --- Example: STDIO transport ---

# [mcp_servers.docs]

# enabled = true # optional; default true

# required = true # optional; fail startup/resume if this server cannot initialize

# command = "docs-server" # required

# args = ["--port", "4000"] # optional

# env = { "API_KEY" = "value" } # optional key/value pairs copied as-is

# env_vars = ["ANOTHER_SECRET"] # optional: forward local parent env vars

# env_vars = ["LOCAL_TOKEN", { name = "REMOTE_TOKEN", source = "remote" }]

# cwd = "/path/to/server" # optional working directory override

# experimental_environment = "remote" # experimental: run stdio via a remote executor

# startup_timeout_sec = 10.0 # optional; default 10.0 seconds

# # startup_timeout_ms = 10000 # optional alias for startup timeout (milliseconds)

# tool_timeout_sec = 60.0 # optional; default 60.0 seconds

# enabled_tools = ["search", "summarize"] # optional allow-list

# disabled_tools = ["slow-tool"] # optional deny-list (applied after allow-list)

# scopes = ["read:docs"] # optional OAuth scopes

# oauth_resource = "https://docs.example.com/" # optional OAuth resource

# --- Example: Streamable HTTP transport ---

# [mcp_servers.github]

# enabled = true # optional; default true

# required = true # optional; fail startup/resume if this server cannot initialize

# url = "https://github-mcp.example.com/mcp" # required

# bearer_token_env_var = "GITHUB_TOKEN" # optional; Authorization: Bearer <token>

# http_headers = { "X-Example" = "value" } # optional static headers

# env_http_headers = { "X-Auth" = "AUTH_ENV" } # optional headers populated from env vars

# startup_timeout_sec = 10.0 # optional

# tool_timeout_sec = 60.0 # optional

# enabled_tools = ["list_issues"] # optional allow-list

# disabled_tools = ["delete_issue"] # optional deny-list

# scopes = ["repo"] # optional OAuth scopes

################################################################################

# Model Providers

################################################################################

# Built-ins include:

# - openai

# - ollama

# - lmstudio

# - amazon-bedrock

# These IDs are reserved. Use a different ID for custom providers.

[model_providers]

# --- Example: built-in Amazon Bedrock provider options ---

# model_provider = "amazon-bedrock"

# model = "<bedrock-model-id>"

# [model_providers.amazon-bedrock.aws]

# profile = "default"

# region = "eu-central-1"

# --- Example: OpenAI data residency with explicit base URL or headers ---

# [model_providers.openaidr]

# name = "OpenAI Data Residency"

# base_url = "https://us.api.openai.com/v1" # example with 'us' domain prefix

# wire_api = "responses" # only supported value

# # requires_openai_auth = true # use only for providers backed by OpenAI auth

# # request_max_retries = 4 # default 4; max 100

# # stream_max_retries = 5 # default 5; max 100

# # stream_idle_timeout_ms = 300000 # default 300_000 (5m)

# # supports_websockets = true # optional

# # experimental_bearer_token = "sk-example" # optional dev-only direct bearer token

# # http_headers = { "X-Example" = "value" }

# # env_http_headers = { "OpenAI-Organization" = "OPENAI_ORGANIZATION", "OpenAI-Project" = "OPENAI_PROJECT" }

# --- Example: Azure/OpenAI-compatible provider ---

# [model_providers.azure]

# name = "Azure"

# base_url = "https://YOUR_PROJECT_NAME.openai.azure.com/openai"

# wire_api = "responses"

# query_params = { api-version = "2025-04-01-preview" }

# env_key = "AZURE_OPENAI_API_KEY"

# env_key_instructions = "Set AZURE_OPENAI_API_KEY in your environment"

# # supports_websockets = false

# --- Example: command-backed bearer token auth ---

# [model_providers.proxy]

# name = "OpenAI using LLM proxy"

# base_url = "https://proxy.example.com/v1"

# wire_api = "responses"

#

# [model_providers.proxy.auth]

# command = "/usr/local/bin/fetch-codex-token"

# args = ["--audience", "codex"]

# timeout_ms = 5000

# refresh_interval_ms = 300000

# --- Example: Local OSS (e.g., Ollama-compatible) ---

# [model_providers.local_ollama]

# name = "Ollama"

# base_url = "http://localhost:11434/v1"

# wire_api = "responses"

################################################################################

# Apps / Connectors

################################################################################

# Optional per-app controls.

[apps]

# [_default] applies to all apps unless overridden per app.

# [apps._default]

# enabled = true

# destructive_enabled = true

# open_world_enabled = true

# approvals_reviewer = "user" # user | auto_review

# default_tools_approval_mode = "auto" # auto | prompt | writes | approve

#

# [apps.google_drive]

# enabled = false

# destructive_enabled = false # block destructive-hint tools for this app

# default_tools_enabled = true

# approvals_reviewer = "auto_review"

# default_tools_approval_mode = "prompt" # auto | prompt | writes | approve

#

# [apps.google_drive.tools."files/delete"]

# enabled = false

# approval_mode = "approve"

# Optional tool suggestion allowlist for connectors or plugins Codex can offer to install.

# [tool_suggest]

# discoverables = [

# { type = "connector", id = "gmail" },

# { type = "plugin", id = "figma@openai-curated" },

# ]

# disabled_tools = [

# { type = "plugin", id = "slack@openai-curated" },

# { type = "connector", id = "connector_googlecalendar" },

# ]

################################################################################

# Config Profiles (separate files)

################################################################################

# To create a config profile, put overrides in a separate profile file under $CODEX_HOME.

# Select it with codex --profile ci.

# For example, a CI profile could live at $CODEX_HOME/ci.config.toml:

# model = "gpt-5.4"

# approval_policy = "on-request"

# sandbox_mode = "read-only"

# service_tier = "flex" # or another supported service tier id

# oss_provider = "ollama"

# model_reasoning_effort = "medium"

# plan_mode_reasoning_effort = "high"

# model_reasoning_summary = "auto"

# model_verbosity = "medium"

# personality = "pragmatic" # or "friendly" or "none"

# chatgpt_base_url = "https://chatgpt.com/backend-api/"

# model_catalog_json = "./models.json"

# model_instructions_file = "/absolute/or/relative/path/to/instructions.txt"

# experimental_compact_prompt_file = "./compact_prompt.txt"

# tools_view_image = true

# features = { unified_exec = false }

################################################################################

# Projects (trust levels)

################################################################################

[projects]

# Mark specific worktrees as trusted or untrusted.

# [projects."/absolute/path/to/project"]

# trust_level = "trusted" # or "untrusted"

################################################################################

# Tools

################################################################################

[tools]

# view_image = true

################################################################################

# OpenTelemetry (OTEL) - disabled by default

################################################################################

[otel]

# Include user prompt text in logs. Default: false

log_user_prompt = false

# Environment label applied to telemetry. Default: "dev"

environment = "dev"

# Exporter: none (default) | otlp-http | otlp-grpc

exporter = "none"

# Trace exporter: none (default) | otlp-http | otlp-grpc

trace_exporter = "none"

# Metrics exporter: none | statsig | otlp-http | otlp-grpc

metrics_exporter = "statsig"

# Example OTLP/HTTP exporter configuration

# [otel.exporter."otlp-http"]

# endpoint = "https://otel.example.com/v1/logs"

# protocol = "binary" # "binary" | "json"

# [otel.exporter."otlp-http".headers]

# "x-otlp-api-key" = "${OTLP_TOKEN}"

# [otel.exporter."otlp-http".tls]

# ca-certificate = "certs/otel-ca.pem"

# client-certificate = "/etc/codex/certs/client.pem"

# client-private-key = "/etc/codex/certs/client-key.pem"

# Example OTLP/gRPC trace exporter configuration

# [otel.trace_exporter."otlp-grpc"]

# endpoint = "https://otel.example.com:4317"

# headers = { "x-otlp-meta" = "abc123" }

################################################################################

# Windows

################################################################################

[windows]

# Native Windows sandbox mode (Windows only): unelevated | elevated

sandbox = "unelevated"
```

---

## Custom instructions with AGENTS.md

Give Codex extra instructions and context for your project

Codex reads `AGENTS.md` files before doing any work. By layering global guidance with project-specific overrides, you can start each task with consistent expectations, no matter which repository you open.

### How Codex discovers guidance

Codex builds an instruction chain when it starts (once per run; in the TUI this usually means once per launched session). Discovery follows this precedence order:

1. **Global scope:** In your Codex home directory (defaults to `~/.codex`, unless you set `CODEX_HOME`), Codex reads `AGENTS.override.md` if it exists. Otherwise, Codex reads `AGENTS.md`. Codex uses only the first non-empty file at this level.
2. **Project scope:** Starting at the project root (typically the Git root), Codex walks down to your current working directory. If Codex cannot find a project root, it only checks the current directory. In each directory along the path, it checks for `AGENTS.override.md`, then `AGENTS.md`, then any fallback names in `project_doc_fallback_filenames`. Codex includes at most one file per directory.
3. **Merge order:** Codex concatenates files from the root down, joining them with blank lines. Files closer to your current directory override earlier guidance because they appear later in the combined prompt.

Codex skips empty files and stops adding files once the combined size reaches the limit defined by `project_doc_max_bytes` (32 KiB by default). For details on these knobs, see Project instructions discovery. Raise the limit or split instructions across nested directories when you hit the cap.

### Create global guidance

Create persistent defaults in your Codex home directory so every repository inherits your working agreements.

1. Ensure the directory exists:

```bash
mkdir -p ~/.codex
```

2. Create `~/.codex/AGENTS.md` with reusable preferences:

```markdown
# ~/.codex/AGENTS.md

## Working agreements

- Always run `npm test` after modifying JavaScript files.
- Prefer `pnpm` when installing dependencies.
- Ask for confirmation before adding new production dependencies.
```

3. Run Codex anywhere to confirm it loads the file:

```bash
codex --ask-for-approval never "Summarize the current instructions."
```

Expected: Codex quotes the items from `~/.codex/AGENTS.md` before proposing work.

Use `~/.codex/AGENTS.override.md` when you need a temporary global override without deleting the base file. Remove the override to restore the shared guidance.

### Layer project instructions

Repository-level files keep Codex aware of project norms while still inheriting your global defaults.

1. In your repository root, add an `AGENTS.md` that covers basic setup:

```markdown
# AGENTS.md

## Repository expectations

- Run `npm run lint` before opening a pull request.
- Document public utilities in `docs/` when you change behavior.
```

2. Add overrides in nested directories when specific teams need different rules. For example, inside `services/payments/` create `AGENTS.override.md`:

```markdown
# services/payments/AGENTS.override.md

## Payments service rules

- Use `make test-payments` instead of `npm test`.
- Never rotate API keys without notifying the security channel.
```

3. Start Codex from the payments directory:

```bash
codex --cd services/payments --ask-for-approval never "List the instruction sources you loaded."
```

Expected: Codex reports the global file first, the repository root `AGENTS.md` second, and the payments override last.

Codex stops searching once it reaches your current directory, so place overrides as close to specialized work as possible.

Here is a sample repository after you add a global file and a payments-specific override:

- AGENTS.md Repository expectations
- services/
  - payments/
    - AGENTS.md Ignored because an override exists
    - AGENTS.override.md Payments service rules
    - README.md
  - search/
    - AGENTS.md
    - …

### Customize fallback filenames

If your repository already uses a different filename (for example `TEAM_GUIDE.md`), add it to the fallback list so Codex treats it like an instructions file.

1. Edit your Codex configuration:

```toml
# ~/.codex/config.toml
project_doc_fallback_filenames = ["TEAM_GUIDE.md", ".agents.md"]
project_doc_max_bytes = 65536
```

2. Restart Codex or run a new command so the updated configuration loads.

Now Codex checks each directory in this order: `AGENTS.override.md`, `AGENTS.md`, `TEAM_GUIDE.md`, `.agents.md`. Filenames not on this list are ignored for instruction discovery. The larger byte limit allows more combined guidance before truncation.

With the fallback list in place, Codex treats the alternate files as instructions:

- TEAM_GUIDE.md Detected via fallback list
- .agents.md Fallback file in root
- support/
  - AGENTS.override.md Overrides fallback guidance
  - playbooks/
    - …

Set the `CODEX_HOME` environment variable when you want a different profile, such as a project-specific automation user:

```bash
CODEX_HOME=$(pwd)/.codex codex exec "List active instruction sources"
```

Expected: The output lists files relative to the custom `.codex` directory.

### Verify your setup

- Run `codex --ask-for-approval never "Summarize the current instructions."` from a repository root. Codex should echo guidance from global and project files in precedence order.
- Use `codex --cd subdir --ask-for-approval never "Show which instruction files are active."` to confirm nested overrides replace broader rules.
- To audit which instruction files Codex loaded, opt into a plaintext TUI log with `codex -c log_dir=./.codex-log` and check `./.codex-log/codex-tui.log`, or inspect the most recent `session-*.jsonl` file if you enabled session logging.
- If instructions look stale, restart Codex in the target directory. Codex rebuilds the instruction chain on every run (and at the start of each TUI session), so there is no cache to clear manually.

### Troubleshoot discovery issues

- **Nothing loads:** Verify you are in the intended repository and that `codex status` reports the workspace root you expect. Ensure instruction files contain content; Codex ignores empty files.
- **Wrong guidance appears:** Look for an `AGENTS.override.md` higher in the directory tree or under your Codex home. Rename or remove the override to fall back to the regular file.
- **Codex ignores fallback names:** Confirm you listed the names in `project_doc_fallback_filenames` without typos, then restart Codex so the updated configuration takes effect.
- **Instructions truncated:** Raise `project_doc_max_bytes` or split large files across nested directories to keep critical guidance intact.
- **Profile confusion:** Run `echo $CODEX_HOME` before launching Codex. A non-default value points Codex at a different home directory than the one you edited.

### Next steps

- Visit the official AGENTS.md website for more information.
- Review Prompting Codex for conversational patterns that pair well with persistent guidance.
</content>

---

## Subagents

Use subagents in ChatGPT and Codex, and configure custom Codex agents

ChatGPT Work and Codex can run subagent workflows by spawning specialized agents in parallel and then collecting their results in one response. This can be particularly helpful for complex tasks that are highly parallel, such as codebase exploration or implementing a multi-step feature plan.

In local Codex clients, you can also define custom agents with different model configurations and instructions for different tasks.

### Availability

ChatGPT Work exposes subagent workflows and activity to eligible accounts.

Current Codex releases enable subagent workflows by default. Subagent activity appears in the ChatGPT desktop app, Codex CLI, and the IDE extension.

Because each subagent does its own model and tool work, subagent workflows consume more tokens than comparable single-agent runs.

In ChatGPT Work, ask ChatGPT to delegate independent work to subagents. The agents run in ChatGPT's hosted environment, and the task shows their activity and results. At most intelligence levels, ask for delegation explicitly. With Ultra, ChatGPT can proactively delegate work when parallel agents would materially improve speed or quality.

Ask Codex in an app task to delegate independent parts of the work to subagents. Current local Codex releases delegate when you ask directly or when applicable `AGENTS.md` or skill instructions request it. The app surfaces each subagent thread so you can inspect its work and the summary returned to the main task.

Ask Codex in an interactive CLI session to use subagents. Codex can also follow applicable `AGENTS.md` or skill instructions that request delegation. Use `/agent` to inspect and switch between agent threads while they run. The main thread collects the subagent results into its final response.

Ask Codex in an IDE task to delegate independent parts of the work to subagents. Codex can also follow applicable `AGENTS.md` or skill instructions that request delegation. When the background-agent UI is available, active subagents appear above the composer. Expand the panel to see their status, stop all active subagents, or open an individual subagent thread.

### Why subagent workflows help

Even with large context windows, models have limits. If you flood the main conversation (where you're defining requirements, constraints, and decisions) with noisy intermediate output such as exploration notes, test logs, stack traces, and command output, the session can become less reliable over time.

This is often described as:

- **Context pollution**: useful information gets buried under noisy intermediate output.
- **Context rot**: performance degrades as the conversation fills up with less relevant details.

For background, see the Chroma writeup on context rot.

Subagent workflows help by moving noisy work off the main thread:

- Keep the **main agent** focused on requirements, decisions, and final outputs.
- Run specialized **subagents** in parallel for exploration, tests, or log analysis.
- Return **summaries** from subagents instead of raw intermediate output.

They can also save time when the work can run independently in parallel, and they make larger-shaped tasks more tractable by breaking them into bounded pieces. For example, Codex can split analysis of a multi-million-token document into smaller problems and return distilled takeaways to the main thread.

As a starting point, use parallel agents for read-heavy tasks such as exploration, tests, triage, and summarization. Be more careful with parallel write-heavy workflows, because agents editing code at once can create conflicts and increase coordination overhead.

### Core terms

Codex uses a few related terms in subagent workflows:

- **Subagent workflow**: A workflow where Codex runs parallel agents and combines their results.
- **Subagent**: A delegated agent that Codex starts to handle a specific task.
- **Agent thread**: The thread where a subagent does its work. Supported clients let you open these threads to inspect progress or results.

### Triggering subagent workflows

At most intelligence levels, ask for subagents or parallel agent work directly. Ultra enables proactive delegation, so ChatGPT can delegate suitable independent work without a separate request.

Ask for subagents or parallel agent work directly. Codex can also delegate when applicable project or skill instructions request it.

In practice, manual triggering means using direct instructions such as "spawn two agents," "delegate this work in parallel," or "use one agent per point." Subagent workflows consume more tokens than comparable single-agent runs because each subagent does its own model and tool work.

A good subagent prompt should explain how to divide the work, whether Codex should wait for all agents before continuing, and what summary or output to return.

```
Review this branch with parallel subagents. Spawn one subagent for security risks, one for test gaps, and one for maintainability. Wait for all three, then summarize the findings by category with file references.
```

### Choosing models and reasoning

Different agents need different model and reasoning settings.

In ChatGPT Work, choose a model and an intelligence level from the composer. Available intelligence levels can include **Light**, **Medium**, **High**, **Extra High**, and **Max**, depending on the selected model. **Ultra** is available only to eligible accounts and supported models. It uses maximum reasoning and lets ChatGPT proactively delegate suitable work to subagents.

At other intelligence levels, ask for subagents explicitly when you want work delegated in parallel.

If you don't pin a model or `model_reasoning_effort`, Codex can choose a setup that balances intelligence, speed, and price for the task. It may favor `gpt-5.6-terra` for fast scans or a higher-effort `gpt-5.6` configuration for more demanding reasoning. When you want finer control, steer that choice in your prompt or set `model` and `model_reasoning_effort` directly in the agent file.

For most tasks in Codex, start with `gpt-5.6`. Use `gpt-5.6-terra` when you want a faster, lower-cost option for lighter subagent work. If you have ChatGPT Pro and want near-instant text-only iteration, `gpt-5.3-codex-spark` remains available in research preview.

#### Model choice

- **`gpt-5.6`**: Start here for demanding agents. It's strongest for ambiguous, multi-step work that needs planning, tool use, validation, and follow-through across a larger context.
- **`gpt-5.4`**: Use this when a workflow is pinned to GPT-5.4. It combines strong coding, reasoning, tool use, and broader workflows.
- **`gpt-5.6-terra`**: Use for agents that favor speed and efficiency over depth, such as exploration, read-heavy scans, large-file review, or processing supporting documents. It works well for parallel workers that return distilled results to the main agent.
- **`gpt-5.3-codex-spark`**: If you have ChatGPT Pro, use this research preview model for near-instant, text-only iteration when latency matters more than broader capability.

#### Reasoning effort (`model_reasoning_effort`)

- **`ultra`**: Use for the deepest reasoning when the selected model supports it.
- **`max`** and **`xhigh`**: Use for especially demanding reasoning when the selected model supports these levels.
- **`high`**: Use when an agent needs to trace complex logic, check assumptions, or work through edge cases (for example, reviewer or security-focused agents).
- **`medium`**: A balanced default for most agents.
- **`low`**: Use when the task is straightforward and speed matters most.
- **`minimal`** and **`none`**: Use when the selected model supports these lower-latency levels and the task needs little or no reasoning.

Higher reasoning effort increases response time and token usage, but it can improve quality for complex work. For details, see [Models](/codex/models), [Config basics](/codex/config-file/config-basic), and [Configuration Reference](/codex/config-file/config-reference).

`agents.max_depth` controls nesting and defaults to `1`, which lets the root thread spawn direct children but prevents those children from spawning deeper descendants.

### Orchestration and thread controls

ChatGPT or Codex handles orchestration across agents, including spawning new subagents, routing follow-up instructions, waiting for results, and closing agent threads.

When many agents are running, Codex waits until all requested results are available, then returns a consolidated response.

At most intelligence levels, ChatGPT spawns agents after a direct request. With Ultra, ChatGPT can also delegate proactively when parallel work is useful.

Current local Codex releases spawn agents after a direct request or applicable project or skill instruction.

To see it in action, try the following prompt on your project:

```
I would like to review the following points on the current PR (this branch vs main). Spawn one agent per point, wait for all of them, and summarize the result for each point.
1. Security issue
2. Code quality
3. Bugs
4. Race
5. Test flakiness
6. Maintainability of the code
```

### Managing subagents

Open **Subagents** to see read-only **Active** and **Done** lists. Select a completed subagent to inspect its details and result. The web sidebar reports subagent activity; it doesn't provide controls to stop or steer an individual subagent.

- Open a subagent thread from the activity shown in the main thread to inspect its work.
- Ask Codex directly to steer a running subagent, stop it, or close completed subagent threads.

#### Active

No active subagents

#### Done · 3

- **Model api audit** (2m): Audit complete; no endpoint compatibility issues were found.
- **Package audit** (3m): Safest workflow is a focused dependency update.
- **Landing audit** (4m): Audit complete; no final layout blockers remain.

- Use `/agent` in the CLI to switch between active agent threads and inspect the ongoing thread.
- Ask Codex directly to steer a running subagent, stop it, or close completed agent threads.

- When the background-agent panel is available, expand it to inspect status, stop active subagents, or open a subagent thread.
- Ask Codex directly to steer a running subagent, stop it, or close completed subagent threads.

### Approvals and sandbox controls

Subagents inherit your current sandbox policy.

ChatGPT Work runs subagents in its hosted environment and doesn't expose a local Codex sandbox or approval-mode control. Subagents use the tools available to the parent task. Website and connector permissions remain tool-specific.

Subagents inherit the permission mode selected beneath the composer. Choose the permission mode for the parent turn before you ask Codex to delegate work.

In interactive CLI sessions, approval requests can surface from inactive agent threads even while you are looking at the main thread. The approval overlay shows the source thread label, and you can press `o` to open that thread before you approve, reject, or answer the request.

In non-interactive flows, or whenever a run can't surface a fresh approval, an action that needs new approval fails and Codex surfaces the error back to the parent workflow.

Codex also reapplies the parent turn's live runtime overrides when it spawns a child. That includes sandbox and approval choices you set interactively during the session, such as `/permissions` changes or `--yolo`, even if the selected custom agent file sets different defaults.

Subagents inherit the permission mode selected beneath the composer. Choose the permission mode for the parent turn before you ask Codex to delegate work.

You can also override the sandbox configuration for individual custom agents, such as explicitly marking one to work in read-only mode.

### Custom agents

Codex ships with built-in agents:

- `default`: general-purpose fallback agent.
- `worker`: execution-focused agent for implementation and fixes.
- `explorer`: read-heavy codebase exploration agent.

To define your own custom agents, add standalone TOML files under `~/.codex/agents/` for personal agents or `.codex/agents/` for project-scoped agents.

Each file defines one custom agent. Codex loads these files as configuration layers for spawned sessions, so custom agents can override the same settings as a normal Codex session config. That can feel heavier than a dedicated agent manifest, and the format may evolve as authoring and sharing mature.

Every standalone custom agent file must define:

- `name`
- `description`
- `developer_instructions`

Optional fields such as `nickname_candidates`, `model`, `model_reasoning_effort`, `sandbox_mode`, `mcp_servers`, and `skills.config` inherit from the parent session when you omit them.

#### Global settings

Global subagent settings still live under `[agents]` in your configuration.

| Field | Type | Required | Purpose |
|-------|------|----------|---------|
| `agents.max_threads` | number | No | Concurrent open agent thread cap. |
| `agents.max_depth` | number | No | Spawned agent nesting depth (root session starts at 0). |
| `agents.job_max_runtime_seconds` | number | No | Default timeout per worker for `spawn_agents_on_csv` jobs. |
| `agents.interrupt_message` | boolean | No | Record a model-visible message when an agent turn is interrupted. |

**Notes:**

- `agents.max_threads` defaults to `6` when you leave it unset.
- `agents.max_depth` defaults to `1`, which lets the root thread spawn direct children but prevents those children from spawning deeper descendants. Keep the default unless you specifically need recursive delegation. Raising this value can turn broad delegation instructions into repeated fan-out, which increases token usage, latency, and local resource consumption. `agents.max_threads` still caps concurrent open threads, but it doesn't remove the cost and predictability risks of deeper recursion.
- `agents.job_max_runtime_seconds` is optional. When you leave it unset, `spawn_agents_on_csv` falls back to its per-call default timeout of 1800 seconds per worker.
- `agents.interrupt_message` defaults to `true`. Set it to `false` to omit the model-visible interruption message from the agent's context.
- If a custom agent name matches a built-in agent such as `explorer`, your custom agent takes precedence.

#### Custom agent file schema

| Field | Type | Required | Purpose |
|-------|------|----------|---------|
| `name` | string | Yes | Agent name Codex uses when spawning or referring to this agent. |
| `description` | string | Yes | Human-facing guidance for when Codex should use this agent. |
| `developer_instructions` | string | Yes | Core instructions that define the agent's behavior. |
| `nickname_candidates` | string[] | No | Optional pool of display nicknames for spawned agents. |

You can also include other supported `config.toml` keys in a custom agent file, such as `model`, `model_reasoning_effort`, `sandbox_mode`, `mcp_servers`, and `skills.config`.

Codex identifies the custom agent by its `name` field. Matching the filename to the agent name is the simplest convention, but the `name` field is the source of truth.

#### Display nicknames

Use `nickname_candidates` when you want Codex to assign more readable display names to spawned agents. This is especially helpful when you run many instances of the same custom agent and want the UI to show distinct labels instead of repeating the same agent name.

Nicknames are presentation-only. Codex still identifies and spawns the agent by its `name`.

Nickname candidates must be a non-empty list of unique names. Each nickname can use ASCII letters, digits, spaces, hyphens, and underscores.

Example:

```toml
name = "reviewer"
description = "PR reviewer focused on correctness, security, and missing tests."
developer_instructions = """
Review code like an owner.
Prioritize correctness, security, behavior regressions, and missing test coverage.
"""
nickname_candidates = ["Atlas", "Delta", "Echo"]
```

In practice, the ChatGPT desktop app, Codex CLI, and IDE extension can show the nicknames where agent activity appears, while the underlying agent type stays `reviewer`.

#### Example custom agents

The best custom agents are narrow and opinionated. Give each one clear job, a tool surface that matches that job, and instructions that keep it from drifting into adjacent work.

##### Example 1: PR review

This pattern splits review across three focused custom agents:

- `pr_explorer` maps the codebase and gathers evidence.
- `reviewer` looks for correctness, security, and test risks.
- `docs_researcher` checks framework or API documentation through a dedicated MCP server.

Project config (`.codex/config.toml`):

```toml
[agents]
max_threads = 6
max_depth = 1
```

`.codex/agents/pr-explorer.toml`:

```toml
name = "pr_explorer"
description = "Read-only codebase explorer for gathering evidence before changes are proposed."
model = "gpt-5.3-codex-spark"
model_reasoning_effort = "medium"
sandbox_mode = "read-only"
developer_instructions = """
Stay in exploration mode.
Trace the real execution path, cite files and symbols, and avoid proposing fixes unless the parent agent asks for them.
Prefer fast search and targeted file reads over broad scans.
"""
```

`.codex/agents/reviewer.toml`:

```toml
name = "reviewer"
description = "PR reviewer focused on correctness, security, and missing tests."
model = "gpt-5.4"
model_reasoning_effort = "high"
sandbox_mode = "read-only"
developer_instructions = """
Review code like an owner.
Prioritize correctness, security, behavior regressions, and missing test coverage.
Lead with concrete findings, include reproduction steps when possible, and avoid style-only comments unless they hide a real bug.
"""
```

`.codex/agents/docs-researcher.toml`:

```toml
name = "docs_researcher"
description = "Documentation specialist that uses the docs MCP server to verify APIs and framework behavior."
model = "gpt-5.4-mini"
model_reasoning_effort = "medium"
sandbox_mode = "read-only"
developer_instructions = """
Use the docs MCP server to confirm APIs, options, and version-specific behavior.
Return concise answers with links or exact references when available.
Do not make code changes.
"""

[mcp_servers.openaiDeveloperDocs]
url = "https://developers.openai.com/mcp"
```

This setup works well for prompts like:

```
Review this branch against main. Have pr_explorer map the affected code paths, reviewer find real risks, and docs_researcher verify the framework APIs that the patch relies on.
```

### Process CSV batches with subagents (experimental)

This workflow is experimental and may change as subagent support evolves. Use `spawn_agents_on_csv` when you have many similar tasks that map to one row per work item. Codex reads the CSV, spawns one worker subagent per row, waits for the full batch to finish, and exports the combined results to CSV.

This works well for repeated audits such as:

- reviewing one file, package, or service per row
- checking a list of incidents, PRs, or migration targets
- generating structured summaries for many similar inputs

The tool accepts:

- `csv_path` for the source CSV
- `instruction` for the worker prompt template, using `{column_name}` placeholders
- `id_column` when you want stable item ids from a specific column
- `output_schema` when each worker should return a JSON object with a fixed shape
- `output_csv_path`, `max_concurrency`, and `max_runtime_seconds` for job control

Each worker must call `report_agent_job_result` exactly once. If a worker exits without reporting a result, Codex marks that row with an error in the exported CSV.

Example prompt:

```
Create /tmp/components.csv with columns path,owner and one row per frontend component.

Then call spawn_agents_on_csv with:
- csv_path: /tmp/components.csv
- id_column: path
- instruction: "Review {path} owned by {owner}. Return JSON with keys path, risk, summary, and follow_up via report_agent_job_result."
- output_csv_path: /tmp/components-review.csv
- output_schema: an object with required string fields path, risk, summary, and follow_up
```

When you run this through `codex exec`, Codex shows a single-line progress update on `stderr` while the batch is running. The exported CSV includes the original row data plus metadata such as `job_id`, `item_id`, `status`, `last_error`, and `result_json`.

Related runtime settings:

- `agents.max_threads` caps how many agent threads can stay open concurrently.
- `agents.job_max_runtime_seconds` sets the default per-worker timeout for CSV fan-out jobs. A per-call `max_runtime_seconds` override takes precedence.
- `sqlite_home` controls where Codex stores the SQLite-backed state used for agent jobs and their exported results.

##### Example 2: Frontend integration debugging

This pattern is useful for UI regressions, flaky browser flows, or integration bugs that cross application code and the running product.

Project config (`.codex/config.toml`):

```toml
[agents]
max_threads = 6
max_depth = 1
```

`.codex/agents/code-mapper.toml`:

```toml
name = "code_mapper"
description = "Read-only codebase explorer for locating the relevant frontend and backend code paths."
model = "gpt-5.4-mini"
model_reasoning_effort = "medium"
sandbox_mode = "read-only"
developer_instructions = """
Map the code that owns the failing UI flow.
Identify entry points, state transitions, and likely files before the worker starts editing.
"""
```

`.codex/agents/browser-debugger.toml`:

```toml
name = "browser_debugger"
description = "UI debugger that uses browser tooling to reproduce issues and capture evidence."
model = "gpt-5.4"
model_reasoning_effort = "high"
sandbox_mode = "workspace-write"
developer_instructions = """
Reproduce the issue in the browser, capture exact steps, and report what the UI actually does.
Use browser tooling for screenshots, console output, and network evidence.
Do not edit application code.
"""

[mcp_servers.chrome_devtools]
url = "http://localhost:3000/mcp"
startup_timeout_sec = 20
```

`.codex/agents/ui-fixer.toml`:

```toml
name = "ui_fixer"
description = "Implementation-focused agent for small, targeted fixes after the issue is understood."
model = "gpt-5.3-codex-spark"
model_reasoning_effort = "medium"
developer_instructions = """
Own the fix once the issue is reproduced.
Make the smallest defensible change, keep unrelated files untouched, and validate only the behavior you changed.
"""

[[skills.config]]
path = "/Users/me/.agents/skills/docs-editor/SKILL.md"
enabled = false
```

This setup works well for prompts like:

```
Investigate why the settings modal fails to save. Have browser_debugger reproduce it, code_mapper trace the responsible code path, and ui_fixer implement the smallest fix once the failure mode is clear.
```
</content>

---

## Speed

### Increase speed without sacrificing intelligence

**ChatGPT Work and Codex share usage.** Work usage inside ChatGPT uses the same pricing, credits, and usage limits as Codex. See [Codex pricing](/codex/pricing) for details.

### Fast mode

Codex offers the ability to increase the speed of the model for increased credit consumption.

Fast mode increases supported model speed by 1.5x and consumes credits at a higher rate than Standard mode. It currently supports GPT-5.6, GPT-5.5, and GPT-5.4. GPT-5.6 and GPT-5.5 consume credits at 2.5x the Standard rate; GPT-5.4 consumes credits at 2x the Standard rate.

Use `/fast on`, `/fast off`, or `/fast status` in the CLI to change or inspect the current setting. You can also persist the default with `service_tier = "fast"` plus `[features].fast_mode = true` in `config.toml`. Fast mode is available in the ChatGPT desktop app, Codex CLI, and IDE extension when you sign in with ChatGPT. Fast mode is a ChatGPT credit feature. With an API key, Codex uses API token pricing instead, and ChatGPT credit multipliers don't apply. API Priority processing has its own billing rate; for GPT-5.6, it costs 2x the Standard API token rate.

### Codex-Spark

GPT-5.3-Codex-Spark is a separate fast, less-capable Codex model optimized for near-instant, real-time coding iteration. Unlike fast mode, which speeds up a supported model at a higher credit rate, Codex-Spark is its own model choice and has its own usage limits.

During research preview Codex-Spark is only available for ChatGPT Pro subscribers.
</content>

---

## Rules

Control which commands Codex can run outside the sandbox

Use rules to control which commands Codex can run outside the sandbox.

Rules are experimental and may change.

### Create a rules file

1. Create a `.rules` file under a `rules/` folder next to an active config layer (for example, `~/.codex/rules/default.rules`).

2. Add a rule. This example prompts before allowing `gh pr view` to run outside the sandbox.

```
# Prompt before running commands with the prefix `gh pr view` outside the sandbox.
prefix_rule(
    # The prefix to match.
    pattern = ["gh", "pr", "view"],

    # The action to take when Codex requests to run a matching command.
    decision = "prompt",

    # Optional rationale for why this rule exists.
    justification = "Viewing PRs is allowed with approval",

    # `match` and `not_match` are optional "inline unit tests" where you can
    # provide examples of commands that should (or should not) match this rule.
    match = [
        "gh pr view 7888",
        "gh pr view --repo openai/codex",
        "gh pr view 7888 --json title,body,comments",
    ],
    not_match = [
        # Does not match because the `pattern` must be an exact prefix.
        "gh pr --repo openai/codex view 7888",
    ],
)
```

3. Restart Codex.

Codex scans `rules/` under every active config layer at startup, including Team Config locations and the user layer at `~/.codex/rules/`. Project-local rules under `<repo>/.codex/rules/` load only when the project `.codex/` layer is trusted.

When you add a command to the allow list in the TUI, Codex writes to the user layer at `~/.codex/rules/default.rules` so future runs can skip the prompt.

When Smart approvals are enabled (the default), Codex may propose a `prefix_rule` for you during escalation requests. Review the suggested prefix carefully before accepting it.

Admins can also enforce restrictive `prefix_rule` entries from `requirements.toml`.

### Understand rule fields

`prefix_rule()` supports these fields:

- **`pattern`** (required): A non-empty list defining the command prefix to match. Each element is either a literal string (for example, `"pr"`) or a union of literals (for example, `["view", "list"]`) to match alternatives at that argument position.

- **`decision`** (defaults to `"allow"`): The action to take when the rule matches. Codex applies the most restrictive decision when more than one rule matches (`forbidden` > `prompt` > `allow`).
  - `allow`: Run the command outside the sandbox without prompting.
  - `prompt`: Prompt before each matching invocation.
  - `forbidden`: Block the request without prompting.

- **`justification`** (optional): A non-empty, human-readable reason for the rule. Codex may surface it in approval prompts or rejection messages. When using `forbidden`, include a recommended alternative when appropriate.

- **`match` and `not_match`** (defaults to `[]`): Examples that Codex validates when loading your rules. Use these to catch mistakes before a rule takes effect.

When Codex considers a command to run, it compares the command's argument list to `pattern`. Internally, Codex treats the command as a list of arguments (like what `execvp(3)` receives).

### Shell wrappers and compound commands

Some tools wrap several shell commands into a single invocation, for example:

```
["bash", "-lc", "git add . && rm -rf /"]
```

Because this kind of command can hide multiple actions inside one string, Codex treats `bash -lc`, `bash -c`, and their `zsh` / `sh` equivalents specially.

#### When Codex can safely split the script

If the shell script is a linear chain of commands made only of:

- plain words (no variable expansion, no `VAR=...`, `$FOO`, `*`, etc.)
- joined by safe operators (`&&`, `||`, `;`, or `|`)

then Codex parses it (using tree-sitter) and splits it into individual commands before applying your rules.

The script above is treated as two separate commands:

- `["git", "add", "."]`
- `["rm", "-rf", "/"]`

Codex then evaluates each command against your rules, and the most restrictive result wins.

Even if you allow `pattern=["git", "add"]`, Codex won't auto allow `git add . && rm -rf /`, because the `rm -rf /` portion is evaluated separately and prevents the whole invocation from being auto allowed.

This prevents dangerous commands from being smuggled in alongside safe ones.

#### When Codex does not split the script

If the script uses more advanced shell features, such as:

- redirection (`>`, `>>`, `<`)
- substitutions (`$(...)`, `...`)
- environment variables (`FOO=bar`)
- wildcard patterns (`*`, `?`)
- control flow (`if`, `for`, `&&` with assignments, etc.)

then Codex doesn't try to interpret or split it.

In those cases, the entire invocation is treated as:

```
["bash", "-lc", "<full script>"]
```

and your rules are applied to that single invocation.

With this handling, you get the security of per-command evaluation when it's safe to do so, and conservative behavior when it isn't.

### Test a rule file

Use `codex execpolicy check` to test how your rules apply to a command:

```
codex execpolicy check --pretty \
  --rules ~/.codex/rules/default.rules \
  -- gh pr view 7888 --json title,body,comments
```

The command emits JSON showing the strictest decision and any matching rules, including any `justification` values from matched rules. Use more than one `--rules` flag to combine files, and add `--pretty` to format the output.

### Understand the rules language

The `.rules` file format uses Starlark (see the language specification). Its syntax resembles Python, but it's engineered for safe execution: the rules engine can run it without side effects (for example, avoiding filesystem operations).
</content>

---

## Record & Replay

Show Codex a workflow once and turn it into a reusable skill

Record & Replay is available on macOS. Initial availability excludes the European Economic Area, the United Kingdom, and Switzerland. Computer Use must also be available and enabled.

Record & Replay lets you demonstrate a workflow on your Mac and turn it into a reusable skill. Use it when the workflow is repetitive, depends on your preferences, or is easier to show than to describe in a prompt.

For example, you might record how you file an expense, book a parking space, create a correctly configured issue, publish a video, or download a recurring report. Codex can package the pattern into a skill that you can use again with Computer Use, browser actions, connected plugins, or a combination of them.

### Before you start

Pick a workflow that you already know how to complete. Record & Replay works best when the steps are stable and the success criteria are clear.

### Start a recording

1. In the ChatGPT desktop app, select ChatGPT Work or Codex and open **Plugins**.
2. Open the **+** menu.
3. Select **Record a skill**.
4. Review the suggested prompt, give Codex any helpful context, and submit it.
5. When Codex asks for permission to record your actions, approve the request once you are ready to demonstrate the workflow.
6. Perform the workflow on your Mac.
7. When you are done, stop recording from the menu bar, overlay, or tell Codex that you are done.

During recording, Codex observes the actions and window content needed to learn the workflow. Recording continues until you stop it. Keep the recording focused on the task you want Codex to learn.

After you stop recording, Codex inspects the captured workflow and drafts a skill. The skill explains when to use the workflow, what inputs it needs, what steps to follow, and how to verify the result. You can also ask Codex to refine the skill further.

### Replay the workflow

Start a new task and ask Codex to use the generated skill. Give it the values that are different this time, such as the file to upload, the issue to create, or the date range for the report.

Codex uses the skill as reusable context for the task. It can then complete the workflow with the tools available in the current environment, including Computer Use, browser actions, and installed plugins.

### Tips for better recordings

- Keep the demonstration short and complete.
- Let Codex know your goal and any specific inputs that might vary between skill uses before you start recording.
- Use realistic inputs, but avoid secrets and sensitive data.
- Refine the skill after recording to call out hidden preferences that matter, such as naming conventions, field defaults, or decision points.
- Stop recording when the workflow is complete instead of continuing into unrelated cleanup.

### When to build another plugin

Record & Replay is a fast way to create a skill from a demonstrated workflow. If you want to distribute a separate stable package across a team, bundle multiple skills, include connectors, add MCP servers, or manage install metadata, package that workflow as its own plugin. See [Build plugins](/codex/build-plugins).

### Troubleshooting

#### I don't see Record & Replay

If your organization manages Codex with `requirements.toml`, the `[features].computer_use` requirement controls Record & Replay too. Setting `computer_use = false` makes both features unavailable.
</content>

---

## Model Context Protocol

Give Codex access to third-party tools and context

Model Context Protocol (MCP) connects models to tools and context. Use it to give ChatGPT or Codex access to third-party documentation, or to let it interact with developer tools like your browser or Figma.

ChatGPT web can use remote MCP-backed tools supplied by plugins. Local Codex clients can also connect directly to MCP servers and share their configuration.

The ChatGPT desktop app, Codex CLI, and IDE extension support MCP servers and share MCP configuration for the same Codex host.

The supported server features below apply to MCP servers configured on a Codex host. Hosted plugin tools can have different capabilities.

### Supported MCP features

- **STDIO servers**: Servers that run as a local process (started by a command).
  - Environment variables
- **Streamable HTTP servers**: Servers that you access at an address.
  - Bearer token authentication
  - OAuth authentication
  - ChatGPT session authentication for trusted first-party servers
- **Server instructions**: Codex reads the MCP `instructions` field returned during initialization and uses it as server-wide guidance alongside the server's tools.

If you build or maintain an MCP server for Codex, use `instructions` for cross-tool workflows, constraints, and rate limits that apply across the server. Keep the first 512 characters self-contained so the most important guidance is available when Codex is deciding how to use the server.

### Connect Codex to an MCP server

Codex stores MCP configuration in `config.toml` alongside other Codex configuration settings. By default this is `~/.codex/config.toml`, but you can also scope MCP servers to a project with `.codex/config.toml` (trusted projects only).

The ChatGPT desktop app, Codex CLI, and IDE extension share this configuration. Once you configure your MCP servers, you can switch among those clients without redoing setup.

#### Configure in the ChatGPT desktop app

1. Open **Settings**, then select **MCP servers**.
2. Select **Add server**.
3. Enter a name, choose **STDIO** or **Streamable HTTP**, and provide the server's command or URL.
4. Save the server, then select **Restart**.

The server list shows which servers are enabled and which require OAuth. Select **Authenticate** when an OAuth server requires sign-in. In the composer, type `/mcp` to view connected servers.

### Use MCP-backed tools in ChatGPT web

In a hosted ChatGPT Work conversation, install a plugin to use its bundled connectors and remote MCP tools. Workspace administrators can control which plugins and tools are available.

ChatGPT web doesn't read local Codex configuration files or expose the local Codex command menu. Browse and manage available tools through **Plugins** in ChatGPT Work.

#### Configure with the CLI

##### Add an MCP server

```
codex mcp add <server-name> --env VAR1=VALUE1 --env VAR2=VALUE2 -- <stdio server-command>
```

For example, to add Context7 (a free MCP server for developer documentation), you can run the following command:

```
codex mcp add context7 -- npx -y @upstash/context7-mcp
```

##### Other CLI commands

Run `codex mcp list` to see configured servers. To see all available MCP commands, run `codex mcp --help`. For a server that supports OAuth, run `codex mcp login <server-name>`.

##### Terminal UI (TUI)

In the `codex` TUI, use `/mcp` to see your active MCP servers.

#### Configure in the IDE extension

1. Open the gear menu, then select **MCP servers**.
2. Select **Add server**.
3. Enter a name, choose **STDIO** or **Streamable HTTP**, and provide the server's command or URL.
4. Save the server, then select **Restart extension**.

The MCP server list shows which servers are enabled and which require OAuth. Select **Authenticate** when an OAuth server requires sign-in.

#### Configure with config.toml

For more fine-grained control, edit `~/.codex/config.toml` or a project-scoped `.codex/config.toml`. See the configuration reference for a searchable list of every supported MCP option.

Configure each MCP server with a `[mcp_servers.<server-name>]` table in the configuration file.

##### STDIO servers

- `command` (required): The command that starts the server.
- `args` (optional): Arguments to pass to the server.
- `env` (optional): Environment variables to set for the server.
- `env_vars` (optional): Environment variables to allow and forward.
- `cwd` (optional): Working directory to start the server from.
- `experimental_environment` (optional): Set to `remote` to start the stdio server through a remote executor environment when one is available.

`env_vars` can contain plain variable names or objects with a source:

```
env_vars = ["LOCAL_TOKEN", { name = "REMOTE_TOKEN", source = "remote" }]
```

String entries and `source = "local"` read from Codex's local environment. `source = "remote"` reads from the remote executor environment and requires remote MCP stdio.

##### Streamable HTTP servers

- `url` (required): The server address.
- `auth` (optional): Authentication to try after configured bearer tokens and authorization headers. Use `oauth` (the default) for stored MCP OAuth credentials. Use `chatgpt` to use the current ChatGPT session for the trusted first-party ChatGPT origin, with stored OAuth as a fallback.
- `bearer_token_env_var` (optional): Environment variable name for a bearer token to send in `Authorization`.
- `http_headers` (optional): Map of header names to static values.
- `env_http_headers` (optional): Map of header names to environment variable names (values pulled from the environment).

If no credential source resolves, Codex can connect to the server without authentication. Run `codex mcp login <server-name>` separately to start an MCP OAuth login.

##### Other configuration options

- `startup_timeout_sec` (optional): Timeout (seconds) for the server to start. Default: `10`.
- `tool_timeout_sec` (optional): Timeout (seconds) for the server to run a tool. Default: `60`.
- `enabled` (optional): Set `false` to disable a server without deleting it.
- `required` (optional): Set `true` to make startup fail if this enabled server can't initialize.
- `enabled_tools` (optional): Tool allow list.
- `disabled_tools` (optional): Tool deny list (applied after `enabled_tools`).
- `default_tools_approval_mode` (optional): Default approval behavior for tools from this server. Supported values are `auto`, `prompt`, `writes`, and `approve`. The `writes` mode prompts for tools that aren't marked read-only.
- `tools.<tool>.approval_mode` (optional): Per-tool approval behavior override.

If your OAuth provider requires a fixed callback port, set the top-level `mcp_oauth_callback_port` in `config.toml`. If unset, Codex binds to an ephemeral port.

If your MCP OAuth flow must use a specific callback URL (for example, a remote Devbox ingress URL or a custom callback path), set `mcp_oauth_callback_url`. Codex uses this value as the base callback URL, then appends a server-specific callback ID to produce the OAuth `redirect_uri` it sends during login. Register the full derived `redirect_uri` with your OAuth provider, including the appended callback ID and any configured path, query, or port, rather than registering only the base host or path without that suffix. Local callback URLs (for example `localhost`) bind on the local interface; non-local callback URLs bind on `0.0.0.0` so the callback can reach the host.

If the MCP server advertises `scopes_supported`, Codex prefers those server-advertised scopes during OAuth login. Otherwise, Codex falls back to the scopes configured in `config.toml`.

##### config.toml examples

```
[mcp_servers.context7]
command = "npx"
args = ["-y", "@upstash/context7-mcp"]
env_vars = ["LOCAL_TOKEN"]

[mcp_servers.context7.env]
MY_ENV_VAR = "MY_ENV_VALUE"

# Optional MCP OAuth callback overrides (used by `codex mcp login`)
mcp_oauth_callback_port = 5555
mcp_oauth_callback_url = "https://devbox.example.internal/callback"

[mcp_servers.figma]
url = "https://mcp.figma.com/mcp"
bearer_token_env_var = "FIGMA_OAUTH_TOKEN"
http_headers = { "X-Figma-Region" = "us-east-1" }

[mcp_servers.chrome_devtools]
url = "http://localhost:3000/mcp"
enabled_tools = ["open", "screenshot"]
disabled_tools = ["screenshot"] # applied after enabled_tools
default_tools_approval_mode = "prompt"
startup_timeout_sec = 20
tool_timeout_sec = 45
enabled = true

[mcp_servers.chrome_devtools.tools.open]
approval_mode = "approve"
```

#### Plugin-provided MCP servers

Installed plugins can bundle MCP servers in their plugin manifest. Those servers are launched from the plugin, so user config doesn't set their transport command. User config can still control on/off state and tool policy under `plugins.<plugin>.mcp_servers.<server>`.

```
[plugins."sample@test".mcp_servers.sample]
enabled = true
default_tools_approval_mode = "prompt"
enabled_tools = ["read", "search"]

[plugins."sample@test".mcp_servers.sample.tools.search]
approval_mode = "approve"
```

### Examples of useful MCP servers

The list of MCP servers keeps growing. Here are a few common ones:

- OpenAI Docs MCP: Search and read OpenAI developer docs.
- Context7: Connect to up-to-date developer documentation.
- Figma Local and Remote: Access your Figma designs.
- Playwright: Control and inspect a browser using Playwright.
- Chrome Developer Tools: Control and inspect Chrome.
- Sentry: Access Sentry logs.
- GitHub: Manage GitHub beyond what `git` supports (for example, pull requests and issues).

---

## ChatGPT desktop app for Windows

Use the ChatGPT desktop app on Windows with native sandbox and PowerShell support

The [ChatGPT desktop app for Windows](https://get.microsoft.com/installer/download/9PLM9XGG6VKS?cid=website_cta_psi) gives you one interface for working across projects, running parallel tasks, and reviewing results. The Windows app supports core workflows such as worktrees, scheduled tasks, Git functionality, the built-in browser, file previews, plugins, and skills. It runs natively on Windows using PowerShell and the Windows sandbox, or you can configure it to run in Windows Subsystem for Linux 2 (WSL2).

### Download the ChatGPT desktop app

Download the [ChatGPT desktop app](https://get.microsoft.com/installer/download/9PLM9XGG6VKS?cid=website_cta_psi) for Windows.

Then follow the quickstart to get started.

For enterprise installation and update options, see Deploy the Windows app.

If you prefer a command-line install path, run:

```
winget install --id 9PLM9XGG6VKS -s msstore
```

### Native sandbox

The ChatGPT desktop app on Windows supports a native Windows sandbox when the agent runs in PowerShell, and uses Linux sandboxing when you run the agent in Windows Subsystem for Linux 2 (WSL2). To apply sandbox protections in either mode, select **Ask for approval** beneath the composer before sending messages to Codex.

Running Codex in full access mode means Codex is not limited to your project directory and might perform unintentional destructive actions that can lead to data loss. Keep sandbox boundaries in place and use rules for targeted exceptions, or set your approval policy to never to have Codex attempt to solve problems without asking for escalated permissions, based on your approval and security setup.

### Customize for your dev setup

#### Preferred editor

Choose a default app for **Open**, such as Visual Studio, VS Code, or another editor. You can override that choice per project. If you already picked a different app from the **Open** menu for a project, that project-specific choice takes precedence.

#### Integrated terminal

You can also choose the default integrated terminal. Depending on what you have installed, options include:

* PowerShell
* Command Prompt
* Git Bash
* WSL

This change applies only to new terminal sessions. If you already have an integrated terminal open, restart the app or start a new task before expecting the new default terminal to appear.

### Windows Subsystem for Linux (WSL)

By default, the ChatGPT desktop app uses the Windows-native Codex agent. That means the agent runs commands in PowerShell. The app can still work with projects that live in Windows Subsystem for Linux 2 (WSL2) by using the `wsl` CLI when needed.

If you want to add a project from the WSL filesystem, click **Add new project** or press Ctrl+O, then type `\\wsl$\` into the File Explorer window. From there, choose your Linux distribution and the folder you want to open.

If you plan to keep using the Windows-native agent, prefer storing projects on your Windows filesystem and accessing them from WSL through `/mnt/<drive>/...`. This setup is more reliable than opening projects directly from the WSL filesystem.

If you want the agent itself to run in WSL2, open **Settings**, switch the agent from Windows native to WSL, and **restart the app**. The change doesn't take effect until you restart. Your projects should remain in place after restart.

WSL1 was supported through Codex `0.114`. Starting in Codex `0.115`, the Linux sandbox moved to `bubblewrap`, so WSL1 is no longer supported.

You configure the integrated terminal independently from the agent. You can keep the agent in WSL and still use PowerShell in the terminal, or use WSL for both, depending on your workflow.

### Useful developer tools

Codex works best when a few common developer tools are already installed:

* **Git**: Powers the review panel in the ChatGPT desktop app and lets you inspect or revert changes.
* **Node.js**: A common tool that the agent uses to perform tasks more efficiently.
* **Python**: A common tool that the agent uses to perform tasks more efficiently.
* **.NET SDK**: Useful when you want to build native Windows apps.
* **GitHub CLI**: Powers GitHub-specific functionality in the ChatGPT desktop app.

Install them with the default Windows package manager `winget` by pasting this into the integrated terminal or asking Codex to install them:

```
winget install --id Git.Git
winget install --id OpenJS.NodeJS.LTS
winget install --id Python.Python.3.14
winget install --id Microsoft.DotNet.SDK.10
winget install --id GitHub.cli
```

After installing GitHub CLI, run `gh auth login` to enable GitHub features in the app.

If you need a different Python or .NET version, change the package IDs to the version you want.

### Troubleshooting and FAQ

#### Run commands with elevated permissions

If you need Codex to run commands with elevated permissions, start the ChatGPT desktop app itself as an administrator. After installation, open the Start menu, find the app, and choose **Run as administrator**. The Codex agent inherits that permission level.

#### PowerShell execution policy blocks commands

If you have never used tools such as Node.js or `npm` in PowerShell before, the Codex agent or integrated terminal may hit execution policy errors.

This can also happen if Codex creates PowerShell scripts for you. In that case, you may need a less restrictive execution policy before PowerShell will run them.

An error may look something like this:

```
npm.ps1 cannot be loaded because running scripts is disabled on this system.
```

A common fix is to set the execution policy to `RemoteSigned`:

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

For details and other options, check Microsoft's execution policy guide before changing the policy.

#### Local environment scripts on Windows

If your local environment uses cross-platform commands such as `npm` scripts, you can keep one shared setup script or set of actions for every platform.

If you need Windows-specific behavior, create Windows-specific setup scripts or Windows-specific actions.

Actions run in the environment used by your integrated terminal.

Local setup scripts run in the agent environment: WSL if the agent uses WSL, and PowerShell otherwise.

#### Share config, auth, and sessions with WSL

The Windows app uses the same Codex home directory as native Codex on Windows: `%USERPROFILE%\.codex`.

If you also run the Codex CLI inside WSL, the CLI uses the Linux home directory by default, so it doesn't automatically share configuration, cached auth, or session history with the Windows app.

To share them, use one of these approaches:

* Sync WSL `~/.codex` with `%USERPROFILE%\.codex` on your file system.
* Point WSL at the Windows Codex home directory by setting `CODEX_HOME`:

```
export CODEX_HOME=/mnt/c/Users/<windows-user>/.codex
```

If you want that setting in every shell, add it to your WSL shell profile, such as `~/.bashrc` or `~/.zshrc`.

#### Git features are unavailable

If you don't have Git installed natively on Windows, the app can't use some features. Install it with `winget install Git.Git` from PowerShell or `cmd.exe`.

#### Git isn't detected for projects opened from `\\wsl$`

For now, if you want to use the Windows-native agent with a project also accessible from WSL, the most reliable workaround is to store the project on the native Windows drive and access it in WSL through `/mnt/<drive>/...`.

#### `Cmder` isn't listed in the open dialog

If `Cmder` is installed but doesn't show in Codex's open dialog, add it to the Windows Start Menu: right-click `Cmder` and choose **Add to Start**, then restart Codex or reboot.
</content>

---

## Windows sandbox

Configure and troubleshoot the native Codex sandbox on Windows

### Overview

Use Codex on Windows with the native ChatGPT desktop app, the CLI, or the IDE extension. The ChatGPT desktop app supports core workflows including parallel tasks, worktrees, scheduled tasks, Git functionality, the built-in browser, file previews, plugins, and skills.

The app can run natively in PowerShell with a Windows sandbox instead of requiring WSL or a virtual machine. "This keeps Codex in Windows-native workflows while enforcing bounded filesystem and network permissions."

### Native Windows Sandbox Modes

The native Windows sandbox has two modes:

- natively on Windows with the stronger `elevated` sandbox
- natively on Windows with the fallback `unelevated` sandbox

### Configure the Windows Sandbox

When running Codex natively on Windows, agent mode uses a Windows sandbox to block filesystem writes outside the working folder and prevent network access without explicit approval.

Configure in `config.toml`:

```toml
[windows]
sandbox = "elevated" # or "unelevated"
```

**Elevated Mode**: The preferred native Windows sandbox using dedicated lower-privilege sandbox users, filesystem permission boundaries, firewall rules, and local policy changes for sandboxed commands.

**Unelevated Mode**: The fallback sandbox running commands with a restricted Windows token, applying ACL-based filesystem boundaries and environment-level offline controls. Weaker than elevated but useful when administrator setup is blocked.

Enterprise administrators can constrain sandbox implementations through `requirements.toml`:

```toml
[windows]
allowed_sandbox_implementations = ["elevated"]
```

By default, both modes use a private desktop for stronger UI isolation. Set `windows.sandbox_private_desktop = false` only for compatibility with older `Winsta0\\Default` behavior.

#### Sandbox Permissions

"Running Codex in full access mode means Codex is not limited to your project directory and might perform unintentional destructive actions." For safer automation, maintain sandbox boundaries and use rules for specific exceptions.

#### Windows Version Matrix

| Windows version | Support level | Notes |
|---|---|---|
| Windows 11 | Recommended | Best baseline for Codex on Windows |
| Recent, fully updated Windows 10 | Best effort | Can work but less reliable; version 1809+ required |
| Older Windows 10 builds | Not recommended | More likely to fail |

Environment assumptions:

- `winget` should be available
- The recommended native sandbox depends on administrator-approved setup
- Some enterprise-managed devices block required setup steps

#### Grant Sandbox Read Access

When a command fails because the Windows sandbox cannot read a directory:

```
/sandbox-add-read-dir C:\absolute\directory\path
```

The path must be an existing absolute directory. After success, later commands can read that directory during the current session.

Use the native Windows sandbox by default. Choose WSL when you need Linux-native tooling, your workflow already lives in WSL2, or neither native Windows sandbox mode meets your needs.

### Troubleshooting and FAQ

#### My native sandbox setup failed

Common causes:

- the Windows UAC or administrator prompt was declined
- the machine does not allow local user or group creation
- the machine does not allow firewall rule changes
- the machine blocks logon rights needed by sandbox users
- enterprise policy blocks part of the setup flow

**What to try:**

1. Try the `elevated` sandbox setup again and approve the administrator prompt
2. If blocked, ask your IT team about administrator-approved setup for local user/group creation and firewall configuration
3. If default setup fails, use the `unelevated` sandbox to continue working

#### Codex switched me to the unelevated sandbox

This means Codex could not finish the stronger `elevated` sandbox setup.

- Codex still runs in sandboxed mode
- It applies ACL-based filesystem boundaries but without the separate sandbox-user boundary
- This is a useful fallback, not the preferred long-term enterprise configuration

The best long-term fix is usually getting the `elevated` sandbox working with IT team help.

#### I see Windows error 1385

If sandboxed commands fail with error 1385, Windows is denying the logon type the sandbox user needs.

**What to do:**

1. Ask your IT team whether device policy grants required logon rights to Codex-created sandbox users
2. Compare group policy or OU differences if the issue affects only some machines
3. If you need to keep working, use the `unelevated` sandbox while investigating
4. Send `CODEX_HOME/.sandbox/sandbox.log` with Windows version and failure description

#### Codex warns that some folders are writable by Everyone

Windows permissions on those folders are too broad for sandbox protection.

**What to do:**

1. Review the folders Codex lists in the warning
2. Remove `Everyone` write access from those folders if appropriate
3. Restart Codex or re-run sandbox setup after correcting permissions

#### Sandboxed commands cannot reach the network

Some Codex tasks intentionally run without outbound network access depending on the permissions mode.

**If a task fails:**

1. Check whether the task was supposed to run with network disabled
2. If you expected network access, restart Codex and try again
3. If the issue persists, collect the sandbox log

#### Sandboxing worked before and then stopped

This can happen after moving a repo, changing machine permissions, changing Windows policies, or other system configuration changes.

**What to try:**

1. Restart Codex
2. Try the `elevated` sandbox setup again
3. If that does not fix it, use the `unelevated` sandbox as a temporary fallback
4. Collect the sandbox log for review

#### I need to send diagnostics to OpenAI

Send:

- `CODEX_HOME/.sandbox/sandbox.log`

Also helpful:

- short description of what you were trying to do
- whether the `elevated` sandbox failed or `unelevated` was used
- any error message shown in the app
- whether you saw 1385 or another Windows/PowerShell error
- whether you are on Windows 11 or Windows 10

Do not send:

- the contents of `CODEX_HOME/.sandbox-secrets/`

#### The IDE extension is installed but unresponsive

Your system may be missing C++ development tools, which some native dependencies require:

- Visual Studio Build Tools (C++ workload)
- Microsoft Visual C++ Redistributable (x64)
- With `winget`, run: `winget install --id Microsoft.VisualStudio.2022.BuildTools -e`

Then fully restart VS Code after installation.
</content>

---

## WSL

Run and troubleshoot Codex in Windows Subsystem for Linux

When you use WSL2, Codex runs inside the Linux environment instead of using the native [Windows sandbox](/codex/windows/windows-sandbox). Choose WSL2 when you need Linux-native tooling, your repositories and developer workflow already live in WSL2, or neither native Windows sandbox mode works for your environment.

WSL1 was supported through Codex `0.114`. Starting in Codex `0.115`, the Linux sandbox moved to `bubblewrap`, so WSL1 is no longer supported.

### Launch VS Code from inside WSL

For step-by-step instructions, see the [official VS Code WSL tutorial](https://code.visualstudio.com/docs/remote/wsl-tutorial).

#### Prerequisites

*   Windows with WSL installed. To install WSL, open PowerShell as an administrator, then run `wsl --install` (Ubuntu is a common choice).
*   VS Code with the [WSL extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) installed.

#### Open VS Code from a WSL terminal

```
# From your WSL shell
cd ~/code/your-project
code .
```

This opens a WSL remote window, installs the VS Code Server if needed, and ensures integrated terminals run in Linux.

#### Confirm you're connected to WSL

*   Look for the green status bar that shows `WSL: <distro>`.

*   Integrated terminals should display Linux paths (such as `/home/...`) instead of `C:\`.

*   You can verify with:

    ```
    echo $WSL_DISTRO_NAME
    ```

    This prints your distribution name.

If you don't see "WSL: …" in the status bar, press `Ctrl+Shift+P`, pick `WSL: Reopen Folder in WSL`, and keep your repository under `/home/...` (not `C:\`) for best performance.

If the Windows app or project picker does not show your WSL repository, type `\wsl$` into the file picker or Explorer, then navigate to your distro's home directory.

### Use Codex CLI with WSL

Run these commands from an elevated PowerShell or Windows Terminal:

```
# Install default Linux distribution (like Ubuntu)
wsl --install

# Start a shell inside Windows Subsystem for Linux
wsl
```

Then run these commands from your WSL shell:

```
# Install and run Codex in WSL
curl -fsSL https://chatgpt.com/codex/install.sh | sh
codex
```

### Work on code inside WSL

*   Working in Windows-mounted paths like `/mnt/c/…` can be slower than working in Windows-native paths. Keep your repositories under your Linux home directory (like `~/code/my-app`) for faster I/O and fewer symlink and permission issues:

    ```
    mkdir -p ~/code && cd ~/code
    git clone https://github.com/your/repo.git
    cd repo
    ```

*   If you need Windows access to files, they're under `\wsl$\Ubuntu\home<user>` in Explorer.

### Troubleshooting and FAQ

#### Large repositories feel slow in WSL

*   Make sure you're not working under `/mnt/c`. Move the repository to WSL (for example, `~/code/…`).
*   Increase memory and CPU for WSL if needed; update WSL to the latest version:

    ```
    wsl --update
    wsl --shutdown
    ```

#### VS Code in WSL cannot find codex

Verify the binary exists and is on `PATH` inside WSL:

```
which codex || echo "codex not found"
```

If the binary isn't found, follow the [Codex CLI setup instructions](#use-codex-cli-with-wsl).
</content>
