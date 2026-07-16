# Models

Meet the AI models that power ChatGPT Work and Codex.

## Choose a model

In the ChatGPT desktop app, use the model and reasoning control beneath the composer to choose an available model and adjust its reasoning effort.

Higher reasoning effort can improve results for complex tasks, but it takes longer and uses more tokens. Start with the default effort and increase it when the task needs deeper planning or analysis.

Ultra mode goes beyond a single agent run. It uses subagents to accelerate complex work, making it useful for larger tasks that can be split across subagents.

### For ChatGPT Work on the web

These recommendations apply to ChatGPT Work in ChatGPT on the web. Use the model and reasoning control beneath the composer to choose an available model and adjust its reasoning effort.

### For Codex CLI

In an interactive CLI session, use `/model` to switch models or adjust reasoning effort. You can also choose a model when you launch Codex with `--model` or its `-m` alias:

```bash
codex --model gpt-5.6
```

The same option works with non interactive runs:

```bash
codex exec -m gpt-5.6 "Review the current changes"
```

## Recommended models

### 5.6 Sol

Flagship GPT-5.6 model with the strongest capability for complex coding, computer use, research, and cybersecurity.

```bash
codex -m gpt-5.6-sol
```

可用於: ChatGPT desktop app, ChatGPT web, Codex CLI, Codex IDE extension, Codex cloud, ChatGPT Credits, API Access

### 5.6 Terra

Balanced GPT-5.6 model for everyday work, with performance competitive with GPT-5.5 at a lower cost.

```bash
codex -m gpt-5.6-terra
```

可用於: ChatGPT desktop app, ChatGPT web, Codex CLI, Codex IDE extension, Codex cloud, ChatGPT Credits, API Access

### 5.6 Luna

Fast and affordable GPT-5.6 model that delivers strong capability at the lowest cost in the family.

```bash
codex -m gpt-5.6-luna
```

可用於: ChatGPT desktop app, ChatGPT web, Codex CLI, Codex IDE extension, Codex cloud, ChatGPT Credits, API Access

### 5.5

Previous generation frontier model for complex coding, computer use, knowledge work, and research workflows.

```bash
codex -m gpt-5.5
```

可用於: ChatGPT desktop app, ChatGPT web, Codex CLI, Codex IDE extension, Codex cloud, ChatGPT Credits, API Access

### 5.3 Codex Spark

Text only research preview model optimized for near instant, real time coding iteration. Available to ChatGPT Pro users.

```bash
codex -m gpt-5.3-codex-spark
```

可用於: ChatGPT desktop app, ChatGPT web, Codex CLI, Codex IDE extension, Codex cloud, ChatGPT Credits, API Access

## Choosing Sol, Terra, and Luna

Codex offers three GPT-5.6 models: Sol for detail and polish, Terra as the everyday workhorse, and Luna for clear, repeatable work. If you are unsure, start with Sol.

### Where each model shines

- Sol, for complex, open ended work. Choose Sol for ambiguous, difficult, or high value tasks that need extra analysis, judgment, or polish, such as complex code changes, deep research, or polished documents. For narrower tasks, define what done looks like to keep the work focused.
- Terra, the pragmatic all rounder. Choose Terra for everyday work that needs strong reasoning and tool use when you do not need Sol's full depth. It is a natural starting point for work you previously gave GPT-5.5.
- Luna, for clear, repeatable tasks. Choose Luna for specific, high volume tasks when you know what a good result looks like, such as extraction, classification, transformation, and structured summaries.

### Pick a reasoning effort

Use the lowest reasoning effort that produces the result you need. Increase it for tasks that need more planning, analysis, or checking.

- Light in the Codex app, ChatGPT Work, and IDE extension, or Low in the CLI, suits quick, well scoped tasks.
- Medium balances speed and depth for tasks that need more planning.
- High and Extra High suit difficult work with multiple steps, sources, or tradeoffs.

There is no exact mapping from GPT-5.5 reasoning efforts to GPT-5.6. Try a familiar task at a lower setting and adjust based on the result.

### Know when to use Max or Ultra

Max gives the selected model more time to reason about a single task. Use it for the hardest problems, when depth matters more than speed or usage. If you don't see Max in your options, you'll have to enable it in your app settings.

Ultra uses subagents to handle separate parts of a complex task in parallel. Choose it when you can divide the work into meaningful parts. Most tasks do not need Max or Ultra.

If Ultra doesn't appear in the desktop app's model slider, go to Settings > Configuration, then turn on Ultra in model picker slider.

## Other models

When you sign in with ChatGPT, Codex works best with the recommended models listed above.

### 5.4

Frontier model for professional work with strong coding, reasoning, tool use, and agentic workflow capabilities.

```bash
codex -m gpt-5.4
```

可用於: ChatGPT desktop app, ChatGPT web, Codex CLI, Codex IDE extension, Codex cloud, ChatGPT Credits, API Access

### 5.4 Mini

Fast, efficient mini model for responsive coding tasks and subagents.

```bash
codex -m gpt-5.4-mini
```

可用於: ChatGPT desktop app, ChatGPT web, Codex CLI, Codex IDE extension, Codex cloud, ChatGPT Credits, API Access

You can also point Codex at any model and provider that supports either the Chat Completions or Responses APIs to fit your specific use case.

Support for the Chat Completions API is deprecated and will be removed in future releases of Codex.

## Deprecated Codex models

The `gpt-5.2` and `gpt-5.3-codex` models are deprecated in Codex when you sign in with ChatGPT. If your scripts, configuration files, or `codex exec --model` commands still reference deprecated models, update them to the latest model listed above.

Some models that are deprecated for ChatGPT sign in may still be available in the API. If your workflow depends on one of those models, use API key authentication and check the API models page for current availability.

## Configure your default local model

The ChatGPT desktop app, Codex CLI, and IDE extension use the same `config.toml` configuration file. To specify a model, add a `model` entry to your configuration file. If you don't specify a model, the ChatGPT desktop app, Codex CLI, or IDE extension uses a recommended model.

```toml
model = "gpt-5.6"
```

## Choose a model for cloud tasks

Currently, you can't change the default model for Codex cloud tasks.
