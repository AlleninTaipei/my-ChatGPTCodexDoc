# Features

---

## Projects, chats, and tasks

Keep related chats, tasks, files, and sources together in ChatGPT

### Choose a project or start without one

Create a project when work will continue over time, produce more than one output, or depend on the same files and sources. Start a task without a project when the work is self-contained and doesn't need shared project context.

Use a project to keep related chats, files, instructions, and sources together. The same project can contain chats started in Chat and ChatGPT Work.

### Each project structure

Each project has a **Chats** section for conversations and a **Sources** section for uploaded files and connected context. Project instructions apply across its chats. A ChatGPT project doesn't provide direct access to a folder on your computer, so upload or connect the sources you want ChatGPT to use.

Chat and Work change how ChatGPT approaches a request, not what the conversation is called. In either experience, start a new chat from the project and find it again under **Chats**.

### Work in different environments

#### Codex CLI
Codex CLI treats the directory where you start it as the project for the task. Run `codex` from the directory you want Codex to work in, or pass `--cd <directory>` (`-C`) to set it explicitly. The CLI doesn't expose the ChatGPT Projects view.

#### IDE Extension
The IDE extension treats the folder or workspace open in your IDE as the local project. In a multi-root workspace, select the workspace root for the task. The extension doesn't expose the ChatGPT Projects view from the web or desktop app.

### Work in a project

The **Projects** view brings ChatGPT projects and local projects into one place. ChatGPT projects carry project files and context across related tasks. A local project gives tasks access to one or more folders on your computer, such as a collection of source files or a codebase.

Start a separate task for each distinct outcome so its messages and results stay focused while the project keeps related work organized.

### Organize projects and tasks

Keep active work visible and move finished work out of the way:

- **Pin a project** to keep it near the top of the sidebar. You can also pin it from the Projects view.
- **Pin a task** when you return to it often, even if newer tasks appear in the project.
- **Rename a task** with a short title that describes its outcome, such as "Q3 launch brief" or "Checkout accessibility review."
- **Search projects** from the Projects view. Press Cmd/Ctrl+G to search past tasks when you remember a phrase or branch name but not the title.
- **Archive a task** when you finish the work. From a project's menu, select **Archive tasks** to archive its tasks together.

Pinning doesn't add context or change what ChatGPT can access. It only changes where the project or task appears in the sidebar.

Restore archived tasks from **Settings > Archived tasks**.

### Use local projects for folders and codebases

Add a local project when ChatGPT needs to read or change files in a folder on your computer. For a codebase, the project folder becomes the working directory for Codex tasks.

If a repository contains more than one app or package, use distinct local projects when each task should access only one part of the repository. This keeps the working context focused. Use local environments to define setup actions and common commands for a project.

When you want to isolate code changes from your current checkout, select Codex and start the task in a worktree. Projects and worktrees organize work, but the sandbox enforces what local commands can read, change, or access over the network.

### Start a task without a project

Select **New task** when the work is self-contained and doesn't need shared project files, instructions, or folder access. Create a project first when several tasks will depend on the same context.

### Bring in other tools and context

- Reference open files or select code in the editor to add context for the current turn.
- Configure MCP servers when your organization or developer setup exposes tools through Model Context Protocol.
- Use memories from the connected Codex host, where available, to carry useful context into future tasks.

### Next steps

- Learn how to write and refine prompts
- Learn how to use ChatGPT
- Continue long-running work

---

## Sites

Build and share hosted sites in ChatGPT

### Overview

"Sites lets ChatGPT create, host, refine, and share websites, web apps, and games." Use Sites to transform prompts or existing projects into hosted experiences without separate deployment workflows.

Access Sites through the ChatGPT desktop app or web version at chatgpt.com/sites.

### Get started with Sites

1. **Describe the Site** - Outline audience, purpose, required behavior, and information needed
2. **Review the Site** - Verify generated content and data handling
3. **Refine the Site** - Request changes with relevant context
4. **Manage and share** - Return to Sites to reopen, refine, and share with chosen audience

### Prompt Sites for common tasks

For dashboards or internal tools, include audience and core requirements:

```
Build a project request dashboard for my operations team. Let team members
submit requests, see who owns each one, update the status, and filter the list.
Require people to sign in with their workspace account, and keep the request
data saved between visits.
```

For existing projects requesting deployment:

```
Deploy this project with Sites. Check whether it is compatible, make any
required changes, and give me the deployment URL.
```

### Understand projects, versions, and deployments

A Site represents persistent hosted output accessible from ChatGPT. Sites projects link local source to hosting, storing configuration in `.openai/hosting.json`. Example structure:

```json
{
  "project_id": "<project-id>",
  "d1": "DB",
  "r2": null
}
```

Publishing involves two stages: saving a deployable version and deploying it to production.

### Choose a supported site shape

| Need | Request |
|------|---------|
| Content-led website | Site with no persistent state unless required |
| Saved records/progress | D1 relational database |
| File uploads | R2 object storage |
| Files with metadata | D1 and R2 combined |
| Workspace authentication | Workspace-authenticated identity |
| External sign-in | Authentication-enabled Site |

### Control access and secrets

Sites aren't automatically public. Sharing options may include workspace-only, invite-only, or link-sharing access based on account settings.

Configure environment variables in Settings at chatgpt.com/sites, avoiding secrets in prompts or source files.

### Review before you share

Before sharing a Site:
- Review content and behavior in preview
- Verify proper information and file handling
- Confirm no confidential information exposure
- Choose narrowest appropriate sharing option
- Test access with intended audience

---

## Visualizations

Turn ideas and information into interactive visual explanations in ChatGPT

### Overview

Visualizations convert questions, ideas, and information into charts, maps, diagrams, calculators, simulations, and interactive explainers within ChatGPT conversations. They're useful when adjusting inputs or understanding relationships would clarify answers, enable comparisons, support practice, or facilitate action.

### Availability

The Visualizations preview is rolling out with availability depending on your plan, platform, account, and workspace settings.

#### Current availability by surface

| Surface | Status |
|---------|--------|
| ChatGPT on the web | Available to supported accounts in Chat and ChatGPT Work |
| ChatGPT desktop app | Rolling out in preview |
| ChatGPT mobile apps | Rolling out to eligible accounts; composer controls vary by app version |
| Codex CLI and IDE extension | Visualization rendering not supported |

### How to access

**Desktop app:** Type `@` in the composer, begin entering "Visualize", and select **Visualize** under **Plugins**. The composer adds a **Visualize** tag before your request.

**Web:** In supported Chat or ChatGPT Work conversations, type `@` in the composer, start entering "Visualize", and select **Visualize** under **Plugins** (description: "Create visualizations and interactive tools").

**Alternative:** Type `@Visualize` and select the matching suggestion.

**Note:** Codex CLI doesn't render Visualizations. Open content in ChatGPT on the web or desktop app, then tag `@Visualize`. The Codex IDE extension also doesn't support Visualization rendering.

### When to use visualizations

ChatGPT can choose visual formats when they materially improve answers. You can also tag `@Visualize` when specifically requesting interactive results.

Use the smallest format fitting the task:
- **Diagrams:** labeled relationships or processes
- **Charts/plots:** named numeric data and comparisons
- **Maps:** geographic information
- **Interactive visualizations:** when inputs, time, motion, or spatial relationships should change
- **Sites:** durable hosted applications with shareable URLs, permissions, or persistent data

### Effective prompting strategy

Strong requests identify the outcome, source material, question, and useful interactions. Include information source details (conversation content, pasted data, attached files, or connected sources) and consider higher reasoning settings for complex requests.

### Interactive examples

Three visualizations demonstrate focused prompts becoming interactive explanations:

#### Spirograph
Create an interactive spirograph explaining rolling circle mechanics. Adjust radii, pen distance, and animation speed to observe how a rolling circle traces curves using the equation: `x=(R-r)cos(t)+d cos(((R-r)/r)t)   y=(R-r)sin(t)-d sin(((R-r)/r)t)`

#### Wave interference
Interactive lab worksheet for adjusting wave sources and probing interference fields. Modify wavelength, source spacing, and phase offset while recording observations about nodal patterns and bright-band positions.

#### Tokenizer explainer
Interactive demonstration of GPT tokenization process: "text → UTF-8 bytes → ranked byte-pair merges → integer token IDs". Edit text and step through tokenization stages with a teaching vocabulary (actual models use different vocabularies and IDs).

### Refining results

Continue in the same conversation describing desired changes:
- Add or remove controls, filters, comparisons, or annotations
- Correct source data, units, labels, or assumptions
- Simplify slow results by aggregating, binning, or sampling
- Add text summaries and data tables
- Ensure keyboard accessibility with visible focus states
- Use labels/patterns alongside color; remove looping motion
- Convert to Sites for hosting and persistent access

### Sharing and reuse

Use the conversation's standard **Share** action when available. Review the entire shared conversation first, including source data and earlier messages. Visualizations typically represent information snapshots when created, not live dashboards synchronized with connected sources.

Generated download controls and export formats vary by result. If exports fail, request underlying data in simpler formats or ask ChatGPT to convert to a Site.

### Accessibility considerations

Generated visualizations aim for semantic controls, visible focus, readable contrast, and reduced motion, though results vary. Before sharing:
- Check the visualization quality
- Request text summaries and data tables
- Label axes and units
- Avoid color-only distinctions
- Ensure keyboard control functionality

### Troubleshooting failed results

Visualizations may take a minute or longer to generate. If results are blank or missing:
1. Wait for response completion
2. Reload the conversation once
3. Retry the request

If issues persist:
- Request simpler visualizations
- Aggregate, bin, downsample, or reduce precision in large datasets
- Remove generated controls or libraries not functioning
- Verify important values, geographic boundaries, and source assumptions
- Request charts, diagrams, tables, or Sites instead

### Data handling

Apply the same data-handling judgment used in standard ChatGPT conversations. Include sensitive information only when organizationally permitted, and review full conversations before sharing.

---

**Related documentation:** Sites, Projects/chats/tasks, Work with files, Image generation

---

## Scheduled tasks

Schedule recurring tasks to run in the background. Review active, paused, and completed tasks and recent runs in **Scheduled**. You can combine scheduled tasks with skills for more complex work.

In the ChatGPT desktop app, scheduled tasks can work with local projects and run in the project directory or an isolated worktree. Keep the computer on and the app running when a scheduled task needs local files.

When scheduled tasks are enabled for your workspace, create them from Chat or ChatGPT Work in ChatGPT web and manage their runs from **Scheduled**. Web tasks can use uploaded context and connected tools, but they can't work directly in a folder on your computer.

Codex CLI doesn't provide the Scheduled management interface. Use ChatGPT web or the desktop app to create and manage scheduled tasks. The CLI can help you prepare and test a prompt, skill, or script first.

The IDE extension doesn't provide the Scheduled management interface. Use ChatGPT web or the desktop app to create and manage scheduled tasks. The IDE extension can help you prepare and test a prompt, skill, or workspace change first.

### Manage scheduled tasks on the web

Open **Scheduled** to review task status and recent runs. Use a standalone task when each run should start from the saved prompt. Use a scheduled task in a chat when you want ChatGPT to return to the same conversation with its existing context.

Scheduled tasks on the web can use uploaded files, connected tools, skills, and plugins available to that conversation. They don't keep a local folder or worktree available between runs. Put durable instructions in the task prompt or an attached skill, and keep required source material in an accessible project, upload, or connected service.

Before you schedule a task, test its prompt in a regular web conversation. Review the first few runs, then adjust the prompt, tools, or cadence if the results are too broad or need additional context.

For example, schedule a task to evaluate telemetry errors and submit fixes, or to create reports about recent codebase changes. For ongoing work that should keep using the same context, schedule work from an existing task.

For project-scoped scheduled tasks, keep the machine powered on and the ChatGPT desktop app running. The selected project must still be available on disk when the task is scheduled to run.

In Git repositories, you can choose whether a scheduled task runs in your local project or on a new worktree. Both options run in the background. Worktrees keep changes from scheduled tasks separate from unfinished local work, while running in your local project can modify files you are still working on. In non-version-controlled projects, scheduled tasks run directly in the project directory.

You can also leave the model and reasoning effort on their default settings, or choose them explicitly if you want more control over how the scheduled task runs.

### Manage scheduled tasks

Find all scheduled tasks and their runs on **Scheduled** in the ChatGPT desktop app sidebar.

The **Scheduled** view acts as your inbox. Scheduled task runs with findings appear there, and an unread indicator shows when a run needs your attention.

#### Scheduled tasks

Ask ChatGPT to schedule tasks, set reminders, or monitor for updates

Scheduled tasks are listed with status:
- Daily inbox summary — Every weekday at 8:00 AM · Next run in 16 hours
- Weekly project update — Fridays at 4:00 PM · Next run in 2 days
- Meeting follow-up reminders — Daily at 5:00 PM · Next run in 4 hours

Standalone scheduled tasks start a new task for each scheduled run and report results in **Scheduled**. Use them when each run should be independent or when one scheduled task should run across one or more projects. If you need a custom cadence, use the custom schedule controls. For an advanced schedule, edit its RFC 5545 recurrence rule (RRULE), such as `RRULE:FREQ=MONTHLY;BYMONTHDAY=1;BYHOUR=9;BYMINUTE=0`.

For Git repositories, each scheduled task can run either in your local project or on a dedicated background worktree. Use worktrees when you want to isolate scheduled-task changes from unfinished local work. Use local mode when you want the scheduled task to work directly in your main checkout, keeping in mind that it can change files you are actively editing. In non-version-controlled projects, scheduled tasks run directly in the project directory. You can have the same scheduled task run on more than one project.

Scheduled tasks in ChatGPT Work on the web or in Work or Codex in the ChatGPT desktop app can use plugins. Scheduled tasks can also use skills. To keep scheduled tasks maintainable and shareable across teams, use skills to define the action and provide tools and context. Select or invoke a specific skill in the task prompt when the workflow shouldn't rely on automatic tool selection.

### Ask ChatGPT to create or update scheduled tasks

You can create and update scheduled tasks from a Work or Codex task in ChatGPT. Describe the work, the schedule, and whether each scheduled run should return to the current task or start a new task. ChatGPT can draft the prompt, choose the right destination, and update the scheduled task when its scope or cadence changes.

For example, ask ChatGPT to schedule a follow-up from the current task while a deployment finishes, or ask it to create a standalone scheduled task that checks a project on a recurring schedule.

Skills can also create or update scheduled tasks. For example, a skill for babysitting a pull request could set up a scheduled task that checks the PR status with the GitHub plugin and fixes new review feedback.

### Schedule work from a task

Schedule work from an existing task when you want ChatGPT to return to that task on a schedule. The scheduled work uses the task's existing context instead of starting from a new prompt each time.

Scheduled work in a task can use minute-based intervals for active follow-up loops, or daily and weekly schedules when you need a check-in at a specific time.

Schedule work from a task for:

- checking a long-running operation until it finishes
- polling Slack, GitHub, or another connected source when the results should stay in the same task
- reminding ChatGPT to continue a review loop at a fixed cadence
- running a skill-driven workflow that uses plugins, such as checking PR status and addressing new feedback
- continuing an ongoing research or triage task without losing its context

Use a standalone scheduled task when each run should be independent or when findings should appear as separate runs in **Scheduled**.

When you schedule work from a task, make the prompt durable. It should describe what ChatGPT should do on each scheduled run, how to decide whether there is anything important to report, and when to stop or ask you for input.

### Test scheduled tasks

Before you schedule a task, test the prompt manually in a regular task first. This helps you confirm:

- The prompt is clear and scoped correctly.
- The selected or default model, reasoning effort, and tools behave as expected.
- The resulting output is reviewable.

When you start scheduling runs, review the first few outputs and adjust the prompt or cadence as needed.

In the ChatGPT desktop app, you can explicitly trigger a skill in a scheduled task prompt by using `$skill-name`.

### Worktree cleanup for scheduled tasks

If you choose worktrees for Git repositories, frequent schedules can create many worktrees over time. Archive scheduled runs you no longer need, and avoid pinning runs unless you intend to keep their worktrees.

### Permissions and security model

Scheduled tasks run unattended and use your default sandbox settings.

For a plain-language explanation of these boundaries, see the sandboxing overview. For filesystem and network rules, see Permissions.

- If your sandbox mode is **read-only**, tool calls fail if they require modifying files, accessing network, or working with apps on your computer. Consider updating sandbox settings to workspace write.
- If your sandbox mode is **workspace-write**, tool calls fail if they require modifying files outside the workspace, accessing network, or working with apps on your computer. You can selectively allowlist commands to run outside the sandbox using rules.
- If your sandbox mode is **full access**, background scheduled tasks carry elevated risk, as ChatGPT may change files, run commands, and access network without asking. Consider updating sandbox settings to workspace write, and using rules to selectively define which commands the agent can run with full access.

If you are in a managed environment, admins can restrict these behaviors using admin-enforced requirements. For example, they can disallow `approval_policy = "never"` or constrain allowed sandbox modes. See Admin-enforced requirements.

Scheduled tasks use `approval_policy = "never"` when your organization policy allows it. If admin requirements disallow `approval_policy = "never"`, scheduled tasks fall back to the approval behavior of your selected permission mode.

### Examples

#### Automatically create new skills

```
Scan all of the `~/.codex/sessions` files from the past day and if there have been any issues using particular skills, update the skills to be more helpful. Personal skills only, no repo skills.

If there's anything we've been doing often and struggle with that we should save as a skill to speed up future work, let's do it.

Definitely don't feel like you need to update any- only if there's a good reason!

Let me know if you make any.
```

#### Stay up-to-date with your project

```
Look at the latest remote origin/master or origin/main . Then produce an exec briefing for the last 24 hours of commits that touch <DIRECTORY>

Formatting + structure:

- Use rich Markdown (H1 workstream sections, italics for the subtitle, horizontal rules as needed).
- Preamble can read something like "Here's the last 24h brief for <directory>:"
- Subtitle should read: "Narrative walkthrough with owners; grouped by workstream."
- Group by workstream rather than listing each commit. Workstream titles should be H1.
- Write a short narrative per workstream that explains the changes in plain language.
- Use bullet points and bolding when it makes things more readable
- Feel free to make bullets per person, but bold their name

Content requirements:

- Include PR links inline (e.g., [#123](...)) without a "PRs:" label.
- Do NOT include commit hashes or a "Key commits" section.
- It's fine if multiple PRs appear under one workstream, but avoid per‑commit bullet lists.

Scope rules:

- Only include changes within the current cwd (or main checkout equivalent)
- Only include the last 24h of commits.
- Use `gh` to fetch PR titles and descriptions if it helps.
  Also feel free to pull PR reviews and comments
```

#### Combining scheduled tasks with skills to fix your own bugs

Create a new skill that tries to fix a bug introduced by your own commits by creating a new `$recent-code-bugfix` and store it in your personal skills.

```yaml
---
name: recent-code-bugfix
description: Find and fix a bug introduced by the current author within the last week in the current working directory. Use when a user wants a proactive bugfix from their recent changes, when the prompt is empty, or when asked to triage/fix issues caused by their recent commits. Root cause must map directly to the author's own changes.
---

# Recent Code Bugfix

## Overview

Find a bug introduced by the current author in the last week, implement a fix, and verify it when possible. Operate in the current working directory, assume the code is local, and ensure the root cause is tied directly to the author's own edits.

## Workflow

### 1) Establish the recent-change scope

Use Git to identify the author and changed files from the last week.

- Determine the author from `git config user.name`/`user.email`. If unavailable, use the current user's name from the environment or ask once.
- Use `git log --since=1.week --author=<author>` to list recent commits and files. Focus on files touched by those commits.
- If the user's prompt is empty, proceed directly with this default scope.

### 2) Find a concrete failure tied to recent changes

Prioritize defects that are directly attributable to the author's edits.

- Look for recent failures (tests, lint, runtime errors) if logs or CI outputs are available locally.
- If no failures are provided, run the smallest relevant verification (single test, file-level lint, or targeted repro) that touches the edited files.
- Confirm the root cause is directly connected to the author's changes, not unrelated legacy issues. If only unrelated failures are found, stop and report that no qualifying bug was detected.

### 3) Implement the fix

Make a minimal fix that aligns with project conventions.

- Update only the files needed to resolve the issue.
- Avoid adding extra defensive checks or unrelated refactors.
- Keep changes consistent with local style and tests.

### 4) Verify

Attempt verification when possible.

- Prefer the smallest validation step (targeted test, focused lint, or direct repro command).
- If verification cannot be run, state what would be run and why it wasn't executed.

### 5) Report

Summarize the root cause, the fix, and the verification performed. Make it explicit how the root cause ties to the author's recent changes.
```

Afterward, create a new scheduled task:

```
Check my commits from the last 24h and submit a $recent-code-bugfix.
```

---

## Long-running work

Keep multi-step work focused with clear outcomes and completion criteria

For work that may take many steps, give ChatGPT a clear outcome, constraints, and definition of done. Keep related work in the same task or conversation so ChatGPT can use the same context to choose the next step and decide when the work is complete.

In the ChatGPT desktop app, enter `/goal` to start Goal mode. The progress row lets you pause, resume, edit, or clear the goal while ChatGPT works.

For hosted long-running work in ChatGPT web, use ChatGPT Work and put the outcome, constraints, and review criteria directly in your prompt.

Continue in the same web conversation to add context, change constraints, or ask for a status update. Use separate chats when independent tasks can run in parallel, and avoid giving two tasks write access to the same connected source. For related work, keep the chats and source files together in a project.

In an interactive Codex CLI session, enter `/goal` to start Goal mode. Continue the same session to steer the work or ask for a status update.

In the IDE extension chat, enter `/goal` to start Goal mode for the open workspace. Continue the same chat to steer the task while it runs.

### Start a goal

Type `/goal` in the ChatGPT desktop app, Codex CLI, or the IDE extension. The goal text becomes both the first prompt and the completion criteria for the task.

If the outcome is still unclear, start with `/plan`. Ask ChatGPT to interview you, identify constraints, and turn the result into a goal with measurable success criteria. Then start the refined goal with `/goal`.

### Define what done means

Write a goal that lets ChatGPT verify its own progress. Include three things when they apply:

| Goal element | What to include |
|---|---|
| **Outcome** | Describe the result you want, not only the activity ChatGPT should perform. |
| **Constraints** | Name required tools, boundaries, compatibility needs, or approaches to avoid. |
| **Verification** | Add tests, measurements, or review criteria that prove the work is complete. |

For example:

```
Migrate this codebase from JavaScript to TypeScript. Preserve existing behavior,
compile in strict mode without explicit `any` types, and make the full test suite pass.
```

### Steer a running goal

In the ChatGPT desktop app, the goal progress row appears above the composer. Use it to pause or resume work, edit the goal, or clear it. You can also send follow-up messages while the goal runs to add context or adjust constraints.

Use a side conversation when you want a status recap or an explanation without interrupting the main task. Pause the goal before you expect to lose connectivity, then resume it when you're ready for ChatGPT to continue.

### Steer a running task

Continue in the same conversation to add context, adjust constraints, or ask for a status recap. Start a separate chat when another task can run independently.

### Run goals in parallel

Each task keeps its own context, messages, results, and goal. Run tasks concurrently, but avoid letting two tasks change the same files. Use worktrees to give parallel coding tasks separate checkouts.

For local work, turn on **Prevent sleep while running** in settings so your Mac stays awake. Use Pets or system notifications to see when a task needs input or is ready for review.

### Related docs

- Projects, chats, and tasks
- Goal mode and prompting
- Git worktrees
- Scheduled tasks
- Sandbox and permissions

---

## Notifications

Choose how you are notified when work needs attention

ChatGPT desktop app

Notifications let you know when work needs attention. Their controls and delivery channels vary by surface.

### Configure desktop notifications

Open **Settings** to choose whether turn-completion alerts appear never, only while ChatGPT is in the background, or always. Separate controls let you turn permission and question notifications on or off. Your operating system may ask you to grant notification permission to the ChatGPT desktop app.

#### Follow task activity with a pet

In the ChatGPT desktop app, a floating pet is another way to follow task activity while you work in other apps. It can show when a task is **Running**, **Needs input**, **Ready**, or **Blocked**.

See [Pets](/codex/pets?surface=app) to choose a pet, understand its status, or create your own.

### Configure web notifications

Open **Settings > Notifications** to manage the notification categories and channels available to your account. Depending on the category and account, channels can include push, email, or SMS. Use **Manage tasks** from the task notification settings to open **Scheduled**.

### Configure CLI notifications

For terminal and external notifications, see [Notifications](/codex/config-file/config-advanced#notifications) in the advanced configuration guide. You can choose when the TUI emits a notification and whether Codex runs an external program when a turn completes.

### Follow task activity in the IDE

The IDE extension doesn't provide separate notification controls. Keep the task open to follow its activity. To run an external program when a turn completes, configure `notify` on the connected Codex host. See [Notifications](/codex/config-file/config-advanced#notifications) in the advanced configuration guide.

### Related docs

*   [Long-running work](/codex/long-running-work)
*   [Scheduled tasks](/codex/automations)
*   [Pets](/codex/pets)

---

## Pets

Choose an animated companion and follow task activity across supported interfaces

Pets are optional animated companions for following work. Where a pet appears and what it shows depend on the interface you use. Choosing a pet changes its appearance, not how ChatGPT completes tasks.

### Use a floating pet

In the ChatGPT desktop app, a pet can float above other app windows and help you follow activity across your tasks.

#### Choose and wake a pet

1. Open the profile menu at the bottom of the app and select Pets. You can also open Settings and go to Pets.
2. Choose a built-in or custom pet.
3. Enter `/pet`, or open the command menu and select Wake Pet.

Select Tuck Away Pet in Settings > Pets or the command menu, or enter `/pet` again, to hide the pet. Your selection and the pet's position persist when you reopen the app.

When you select a custom pet, it also appears in your Profile view.

#### Understand pet status

| Status | Meaning |
|--------|---------|
| Running | A task is actively working. |
| Needs input | A task needs your approval, answer, or another decision. |
| Ready | A task has completed and has unread activity. |
| Blocked | A task failed or encountered a system error. |

When more than one task has activity, the pet prioritizes tasks that need input, followed by blocked, ready, and running tasks. Open the activity tray to choose a task.

Select the pet to return to ChatGPT, or select an activity to open its task. The activity tray is separate from system notifications.

#### Follow Computer Use

On macOS, the Computer Use picture-in-picture window can attach to an awake pet. Move the pet, and the window follows.

#### Create a custom pet

1. Open Settings > Pets and select Create your own pet.
2. The app installs the bundled `hatch-pet` skill, reloads skills, and opens a new task.
3. Describe the pet you want and send the prompt.
4. When the task finishes, return to Settings > Pets, select Refresh, and choose your new pet.

Custom pets created in the desktop app are stored locally on your computer. They don't automatically sync to ChatGPT web.

#### Reduce animation

Pets respect your operating system's reduced motion setting. When reduced motion is enabled, the pet uses a still frame instead of sprite animation.

### Choose a pet on the web

If Pets are available for your account and workspace, open Settings > Personalization > Pet > Select pet. Choose a built-in pet, or choose Default to use ChatGPT without a pet.

A web pet appears inside supported ChatGPT Work tasks. It doesn't provide the desktop app's floating overlay, activity tray, or `/pet` command.

#### Upload a custom pet

Select Upload pet to add a custom sprite sheet. The file must be a transparent PNG or WebP, exactly 1536 x 1872 pixels, and no larger than 20 MiB. You can edit, download, refresh, or delete uploaded pets from the same setting.

### Choose a terminal pet

In an interactive Codex CLI session:

- Enter `/pets` or `/pet` to open the pet picker.
- Enter `/pets <name>` to choose a pet directly.
- Enter `/pets off` to disable terminal pets.

The picker includes built-in pets and compatible custom pets installed on your computer. A terminal pet reports activity for the current CLI session. It uses Running, Needs input, Ready, and Blocked states, but it doesn't provide the desktop app's multiple-task activity tray.

Terminal pets require iTerm2 3.6 or later, or a terminal with Kitty graphics or Sixel support. They are unavailable inside tmux and Zellij.

### Pets in the IDE extension

The Codex IDE extension doesn't provide a pet picker or floating pet overlay. Use the ChatGPT desktop app or Codex CLI when you want to use your own pet.

### Related docs

- Notifications
- Long-running work
- ChatGPT desktop app settings

---

## Codex Micro

Your command center for Codex tasks.

Codex Micro is a limited-run collaboration between Codex and Work Louder. It works with the ChatGPT desktop app, giving you a quick way to check on tasks, jump between them, use push-to-talk, and trigger common actions or skills without leaving the keyboard.

### Set up Codex Micro

1. Open the ChatGPT desktop app.
2. Connect Codex Micro to your computer with a USB-C cable or Bluetooth, then follow the setup that appears when ChatGPT detects it.
3. On macOS, allow Input Monitoring when prompted so ChatGPT can respond to key presses.
4. Open Settings > Codex Micro to choose which tasks the Agent Keys follow, assign actions to the Command Keys and analog directions, and adjust the lighting.

To open these settings again, press and hold the dial for 500 milliseconds or select the Codex Micro icon beside your account name at the bottom of ChatGPT.

You'll see Codex Micro in Settings after ChatGPT detects the device for the first time. If you want to use the device outside ChatGPT, customize those controls with Work Louder Input.

### Read and switch tasks with Agent Keys

Each of the six frosted Agent Keys can follow a task and light up to show its current status. Press an Agent Key once to switch to that task without bringing ChatGPT forward. Press it twice within 350 milliseconds to switch tasks and bring the ChatGPT window forward.

| Light | Status | Meaning |
|-------|--------|---------|
| White | Idle | The task is idle. |
| Blue | Thinking | ChatGPT is working. |
| Green | Complete | The task completed with an unread update. |
| Amber | Requires input | ChatGPT needs your approval or response. |
| Red | Error | Something went wrong. |
| Off | No assigned task | The key doesn't follow a task. |

The selected task's key pulses with its status light.

Out of the box, the keys follow your six most recently updated tasks, whether or not they're pinned. You can change Agent source in Settings > Codex Micro to use a different arrangement:

- Most recent tasks: Follow the six most recently updated tasks, pinned or unpinned.
- Pinned tasks: Follow the first six tasks in Pinned.
- Priority tasks: Put tasks waiting for input, unread tasks, and active tasks first.
- Custom assignments: Choose the task assigned to each Agent Key. Press an unassigned Agent Key to open a new task. When you start the task, ChatGPT assigns it to that key.

The status colors stay the same. You can decide which tasks the Agent Keys follow, but you can't turn them into extra Command Keys.

### Use and customize Command Keys

Codex Micro comes with six actions in its default layout:

| Key | Default action |
|-----|-----------------|
| (Lightning bolt) | Turn Fast mode on or off. |
| (Checkmark) | Approve the current request. |
| (X) | Decline the current request. |
| (Plus) | Continue the current task in a new task. |
| (Microphone) | Start push-to-talk. |
| (Send) | Send the message in the composer. |

The Mic key uses your computer's microphone. Codex Micro doesn't have a microphone of its own. Hold the key while you speak, then release it to stop. For hands-free recording, press it twice within 350 milliseconds to keep recording. Press it again to stop.

A sea-green light moves around the keyboard while you record. It changes to a moving white light while ChatGPT processes your speech, then turns solid white when the prompt is ready. Press the Codex key to send it.

In Settings > Codex Micro, select a Command Key, then choose its keycap and action. You can open the browser or terminal, review changes, commit with Git, create a pull request, attach files or photos, manage scheduled tasks, change reasoning effort, or open Skills. If you choose a keycap that's already used somewhere else, ChatGPT swaps the two instead of using one keycap twice.

After you remap a key, swap the physical keycap to match its new action.

### Use the analog stick and dial

The analog stick moves freely in any direction. When you push it far enough from the center, ChatGPT turns the movement into one of four directional actions. Codex Micro starts with the mappings shown here.

Choose any available ChatGPT desktop command or enabled skill for each direction in Settings > Codex Micro.

| Direction | Default action |
|-----------|-----------------|
| Up | Turn Plan mode on or off. |
| Right | Go forward in app history. |
| Down | Show or hide the sidebar. |
| Left | Go back in app history. |

The dial moves through the composer controls and options, with Reasoning selected by default. Turn the dial to change the selection, then press it to open or select the focused control. When a composer control or menu is open, the Agent Key immediately to the right of the dial lights red. Press that key to cancel.

In Settings > Codex Micro, choose whether the dial uses Composer navigation or Reasoning only. In Reasoning only mode, turning the dial opens and adjusts reasoning effort. Press the dial to open the slider or its advanced options.

### Adjust lighting

In Settings > Codex Micro, adjust the brightness and choose how long the lights stay on when you're not using Codex Micro. They come back on when you use the keyboard or an Agent Key changes status. By default, the lights turn off after three minutes.

When the keyboard reports its battery status, you can see it in Settings > Codex Micro and in the Codex Micro icon's sidebar tooltip.

### Add more layers

Codex uses layer 1. Use Work Louder Input to configure up to five more layers with shortcuts and actions for other apps.

### Troubleshoot Codex Micro

#### Pair the keyboard again

Use the bottom-left touch control to start pairing again. The rear button controls power and doesn't start pairing.

1. Hold the bottom-left touch control for three seconds to enter communication mode.
2. Tap the control to choose Bluetooth channel 1, 2, or 3.
3. Hold the control for three seconds on that channel. The channel light flashes while pairing and turns solid when paired.

#### Fix Input Monitoring on macOS

If Settings > Codex Micro shows that Input Monitoring isn't set up, select Open System Settings, then follow these steps:

1. Open System Settings > Privacy & Security > Input Monitoring.
2. Turn on access for ChatGPT if it's already listed. If it's missing, drag ChatGPT from Applications into the list, or select Add (+) and choose ChatGPT.
3. Quit and reopen ChatGPT, then confirm ChatGPT detects Codex Micro on layer 1.

For more about this macOS permission, see Apple's Input Monitoring guide.

#### Get more Work Louder help

For help with Bluetooth, cables, power, or resetting the keyboard, see the Creator Micro 2 setup guide from Work Louder. For direct support, email hello@worklouder.cc.

### Get Codex Micro

You can buy Codex Micro through OpenAI Supply Co while supplies last.

We expect orders to begin shipping shortly after purchase.

---

## Browser

### Overview

Let ChatGPT research and interact with websites

Browser isn't available in Codex CLI or the Codex IDE extension. Open the ChatGPT desktop app to use the built-in browser.

Browser lets ChatGPT open websites, gather current information, and take action while you stay in control. Use it to compare options, complete a multi-step task on a website, or review a page you're building.

Browser is available in ChatGPT on the web and in the ChatGPT desktop app.

Important: Treat page content as untrusted context. Review the site and proposed action before sharing sensitive information or allowing ChatGPT to act.

### Desktop App Browser Features

The built-in browser in the ChatGPT desktop app gives you and ChatGPT a shared view of websites and local web apps inside a task. Use it to preview a page, leave visual feedback, or let ChatGPT interact with a site on your behalf.

#### Browser Profile and Data

The built-in browser uses a browser profile that is separate from your regular browser. It doesn't automatically share your existing tabs or browser session. You can sign in directly when a task requires an account. Open Settings > Browser to manage browser data and any profile-import features available on your device.

Browser downloads go to your system Downloads folder by default. In Settings > Browser, you can choose another download location, reset it to the system default, or turn on Ask where to save downloads.

#### Opening the Browser

Use the Chrome extension instead when ChatGPT needs to work in an existing Chrome tab or use your regular Chrome profile.

Open the built-in browser from the toolbar, by clicking a URL, by navigating manually, or by pressing Cmd+Shift+B (Ctrl+Shift+B on Windows).

### Computer Use in the Browser

Computer Use lets ChatGPT operate the built-in browser directly. ChatGPT can open pages, click, type, inspect rendered state, take screenshots, and verify the result of its work in the page.

In the desktop app, open the Plugins Directory and install Browser. Then ask ChatGPT to use the browser in your task, or reference it directly with `@Browser`.

Example:

```
Use the browser to open http://localhost:3000/settings, reproduce the layout
bug, and fix only the overflowing controls.
```

#### Permissions and Safety

ChatGPT asks before it uses a website unless you have already allowed that site. Manage allowed and blocked sites in Settings > Browser. ChatGPT also asks for confirmation before sensitive actions such as submitting information, making a purchase, changing permissions, or deleting data. ChatGPT can't automate file uploads in the built-in browser.

Important: Instructions on a page can be misleading or malicious. A website permission lets ChatGPT interact with that site; it doesn't make the site's content trustworthy or approve every action.

### Preview a Page

1. Start your app's development server in the integrated terminal or with a local environment action.
2. Open the local route, file-backed page, or public page by clicking a URL or navigating manually in the browser.
3. Review the rendered state alongside the code diff.
4. Leave browser comments on the elements or areas that need changes.
5. Ask ChatGPT to address the comments and keep the scope narrow.

Example:

```
I left comments on the pricing page in the built-in browser. Address the mobile
layout issues and keep the card structure unchanged.
```

### Comment on the Page

When a bug is visible only in the rendered page, use browser comments to give ChatGPT precise feedback.

1. Turn on Annotation mode.
2. Click an element, or drag to select an area.
3. Write and save your comment.
4. Send a message in the task asking ChatGPT to address the comments.

Comments work best when you name the problem and the result you want:

```
This button overflows on mobile. Keep the label on one line if it fits,
otherwise wrap it without changing the card height.

This tooltip covers the data point under the cursor. Reposition the tooltip so
it stays inside the chart bounds.
```

#### Styling Feedback

When you add an annotation to a section on the page, select Adjust next to the text input to give ChatGPT more granular style feedback. You can change values such as font, text, spacing, and color, preview the result on the page, and then send the annotation with a clearer target.

### Keep Browser Tasks Scoped

Keep each browser task small enough to review in one pass.

- Name the page, route, or URL.
- Name the state you care about, such as loading, empty, error, or success.
- Leave comments on the exact elements or areas that need changes.
- Review the page again after ChatGPT finishes.
- Ask ChatGPT to start or check the development server before it opens a local page.

For repository changes, use the review pane to inspect the changes and leave comments.

### Developer Mode

Developer mode works with Computer Use in Chrome and the built-in browser. It gives ChatGPT controlled access to the Chrome DevTools Protocol (CDP). Use it to profile JavaScript, inspect console output and network traffic, examine the DOM and applied styles, or diagnose an issue in the live browser.

To enable it, open Settings > Browser and, under Developer mode, turn on Enable full CDP access. If your organization has disabled this setting, you can't enable it locally. Admins can set `browser_use_full_cdp_access = false` under `[features]` in `requirements.toml` to disable full CDP access and prevent users from enabling the corresponding setting in the ChatGPT desktop app.

Important: Full CDP access can expose sensitive browser internals. ChatGPT asks for explicit approval before it uses full CDP to inspect a website. Review the site, task, and requested access before approving it.

Use `@Browser` for the built-in browser. To use Developer mode in Chrome, set up the Chrome extension and invoke `@Chrome`.

Example:

```
This app is slow. Use @Browser to capture a performance trace and inspect
network traffic, then identify the bottleneck.
```

### Cloud Browser in ChatGPT Work

In ChatGPT Work on the web, ChatGPT can use a cloud-operated browser to research and interact with public websites. It runs separately from the browser on your device, so you can delegate web tasks without giving ChatGPT access to your open tabs or personal browser history.

#### Start a Browser Task

1. Open ChatGPT Work and describe the result you want. Include relevant websites or constraints when they matter.
2. If ChatGPT needs a website, review the site-access request before allowing it.
3. Follow the browser's progress in Task details. Open Cloud browser to inspect the page screenshots and replay.
4. Review the result and any sources before using the information.

Example:

```
Compare the publicly listed prices and cancellation terms for these three
venues. Return a table with links to each source and flag anything that needs a
phone call to confirm.
```

Other useful browser tasks include checking public inventory or appointment times, gathering details from an interactive site, and comparing options whose information is spread across several pages.

#### Website Permissions and Confirmations

ChatGPT asks before accessing a new website by default. The permission applies to the site shown in the request, so check the hostname before allowing it.

In ChatGPT settings, open Cloud browser to manage website permissions. You can choose Always ask, Auto approve, or Always allow, and you can allow or block individual sites. Auto approve lets ChatGPT approve requests after its risk checks; Always allow removes that review step for website access. Use the least-permissive setting that works for your task.

A website permission doesn't approve every action. ChatGPT may ask separately for permission before performing consequential actions.

#### Browser Data

The cloud-operated browser keeps its cookies and browser data separate from the browser on your device. Clearing cloud browser data doesn't clear cookies from your device. To remove its cookies, open Cloud browser in ChatGPT settings, select Browser data, and choose Clear all.

Don't rely on open pages or browser history being available in a later task. Include the important sites and context when you start new work.

### Limitations

- The browser supports public, signed-out websites. It can't sign in to an account, ask for credentials, or use the signed-in session from your browser.
- Some sites block automated browsers or require a CAPTCHA. ChatGPT may not be able to complete a task on those sites.
- The browser is separate from the browser on your device. It can't use your open tabs, extensions, saved passwords, or local browser history.
- Availability can depend on your plan, workspace settings, and rollout. It is available in all regions on paid plans other than Free and Go. Enterprise admins must enable it for their workspace.

During rollout, the browser might not appear immediately even when your plan supports it.

---

## Computer Use

Let ChatGPT use desktop apps while it works

In supported regions, Computer Use in the ChatGPT desktop app is available on macOS and Windows in ChatGPT Work and Codex. Install the Computer Use plugin. On macOS, grant Screen Recording and Accessibility permissions when prompted.

With Computer Use, ChatGPT can see and operate graphical user interfaces on macOS or Windows. Use it for tasks where command-line tools or structured integrations aren't enough, such as checking a desktop app, using a browser, changing app settings, working with a data source that isn't available as a plugin, or reproducing a bug that only happens in a graphical user interface.

Because Computer Use can affect app and system state outside your project workspace, use it for scoped tasks and review permission prompts before continuing.

### Set up Computer Use

In the ChatGPT desktop app, select Work or Codex. Open Plugins > Computer Use and select Install plugin if prompted. If ChatGPT shows Enable, select it. Turn on the Computer Use server and skill toggles, then select Try now to start.

Then open Settings > Computer use to review app access. Connected browser controls show a Manage action. Apps you approve for future tasks appear in the Always-allowed apps section.

On Windows, keep the target app visible on the active desktop while the task runs. On macOS, grant Screen Recording and Accessibility permissions when prompted so ChatGPT can see and interact with the target app.

On macOS, grant:

- Screen Recording permission so ChatGPT can see the target app.
- Accessibility permission so ChatGPT can click, type, and navigate.

### When to use Computer Use

Choose Computer Use when the task depends on a graphical user interface that's hard to verify through files or command output alone.

Good fits include:

- Testing a macOS app, Windows app, iOS simulator flow, or another desktop app that ChatGPT is building.
- Performing a task that requires your web browser.
- Reproducing a bug that only appears in a graphical interface.
- Changing app settings that require clicking through a UI.
- Inspecting information in an app or data source that isn't available through a plugin.
- On macOS, running a scoped task in the background while you keep working elsewhere.
- Executing a workflow that spans more than one app.

For web apps you are building locally, use the built-in browser first.

#### Windows foreground use

On Windows, Computer Use runs on the active desktop. It can't operate in the background while you keep using the same Windows session, so expect ChatGPT to move the pointer, type, and take over the foreground while the task runs.

For Windows tasks that should continue while you step away, keep the Windows device unlocked and connected to the internet. Use remote control from your phone to check progress or send follow-up instructions, or run the ChatGPT desktop app inside a Windows virtual machine so Computer Use takes over the VM instead of your main desktop.

### Start a Computer Use task

Mention `@Computer` or `@AppName` in your prompt, or ask ChatGPT to use Computer Use. Describe the exact app, window, or flow ChatGPT should operate.

If the target app exposes a dedicated plugin or MCP server, prefer that structured integration for data access and repeatable operations. Choose Computer Use when ChatGPT needs to inspect or operate the app visually.

### Permissions and approvals

System permissions for Computer Use are separate from app approvals in ChatGPT. On macOS, Screen Recording and Accessibility permissions let ChatGPT see and operate apps. App approvals determine which apps you allow ChatGPT to use. File reads, file edits, and shell commands still follow the sandbox and approval settings for the task.

With Computer Use, ChatGPT can see and take action only in the apps you allow. During a task, ChatGPT asks for your permission before it can use an app on your computer. You can choose Always allow so ChatGPT can use that app in the future without asking again. You can remove apps from the Always allow list in the Computer Use section of the ChatGPT desktop app settings.

ChatGPT may also ask for permission before taking sensitive or disruptive actions.

If ChatGPT can't see or control an app, open System Settings > Privacy & Security and check Screen Recording and Accessibility for Codex Computer Use on macOS. On Windows, make sure the target app is visible in the active desktop session.

#### Configure Windows app policy

On Windows, Computer Use stores persistent app decisions in `$CODEX_HOME/config.toml`. List the apps that Computer Use can open without prompting:

```
[computer_use.windows]
always_allowed_app_ids = ["mspaint.exe"]
```

Use the app identifier that Windows Computer Use reports, such as an executable name for a desktop app or an app user model ID for a packaged app. ChatGPT prompts for apps that aren't in the list. To revoke a saved decision, remove the app from Settings > Computer Use > Always allow.

This table stores local Computer Use decisions. It's separate from the admin-enforced `requirements.toml`, where administrators can disable Computer Use with `[features].computer_use = false`. Older `$CODEX_HOME/computer-use/config.toml` allow-list entries are migrated into the current setting; its `denied` list isn't part of the current policy schema.

### Locked use

Locked use is for macOS. On Windows, Computer Use works in the foreground.

Locked use lets ChatGPT use Computer Use after your Mac locks, but only after you enable it. Use it when a ChatGPT task needs to use desktop apps from a connected device after the Mac locks.

When you enable locked use, ChatGPT installs an Apple authorization plug-in that participates in the macOS unlock flow.

Locked use is intentionally narrow. It's not a general-purpose remote-unlock path for your Mac, and it doesn't let other apps or local processes unlock the computer.

To use locked use:

1. Open Settings > Computer Use in the app.
2. Enable locked use.
3. Start a task that uses Computer Use from a connected device after your Mac's screen has locked.

When a ChatGPT task accesses an app via Computer Use after your Mac locks, ChatGPT temporarily unlocks the Mac while blocking local use and preserving the locked screen protections. Before unlocking, ChatGPT checks whether the unlock attempt is for an active, trusted Computer Use turn. Outside that short-lived window, ChatGPT denies the unlock and asks you to unlock manually if needed.

Locked use includes safeguards:

- The authorization window is short-lived and scoped to the current unlock attempt.
- Automatic unlock is available only to ChatGPT during active Computer Use turns.
- ChatGPT covers every display while the desktop is temporarily unlocked.
- If ChatGPT detects local keyboard or pointer input, it relocks the Mac and pauses automatic unlock until you unlock it manually.

### Safety guidance

With Computer Use, ChatGPT can view screen content, take screenshots, and interact with windows, menus, keyboard input, and clipboard state in the target app. Treat visible app content, browser pages, screenshots, and files opened in the target app as context ChatGPT may process while the task runs.

Keep tasks narrow and stay present for sensitive flows:

- Give ChatGPT one clear target app or flow at a time.
- You can stop the task or take over your computer at any time.
- Keep sensitive apps closed unless they're required for the task.
- On Windows, expect ChatGPT to take over foreground input while it works; use a secondary device, a VM, or stop the task before using that desktop yourself.
- Avoid tasks that require secrets unless you're present and can approve each step.
- Review app permission prompts before allowing ChatGPT to use an app.
- Use Always allow only for apps you trust ChatGPT to use automatically in future tasks.
- Stay present for account, security, privacy, network, payment, or credential-related settings.
- Cancel the task if ChatGPT starts interacting with the wrong window.

If ChatGPT uses your browser, it can interact with pages where you're already signed in. Review website actions as if you were taking them yourself: web pages can contain malicious or misleading content, and sites may treat approved clicks, form submissions, and signed-in actions as coming from your account. To keep using your browser while ChatGPT works, ask ChatGPT to use a different browser.

The feature can't automate terminal apps or ChatGPT itself, since automating them could bypass ChatGPT security policies. It also can't authenticate as an administrator or approve security and privacy permission prompts on your computer.

File edits and shell commands still follow ChatGPT approval and sandbox settings where applicable. Changes made through desktop apps may not appear in the review pane until they're saved to disk and tracked by the project. Your ChatGPT data controls apply to content processed through ChatGPT, including screenshots taken by Computer Use.

---

## Plugins

Browse, install, and use plugins in ChatGPT and Codex clients

### Overview

Plugins bundle capabilities into reusable workflows in ChatGPT. They can include skills, an MCP-backed app, or both. Plugins are available in ChatGPT Work on the web and in Work or Codex in the ChatGPT desktop app. Codex CLI and the IDE extension can also browse and install plugins for a Codex environment.

In the ChatGPT desktop app, open Plugins from Work or Codex to browse, install, and use plugins. Installed plugins can add skills, connectors, and MCP tools to new chats.

In ChatGPT web, select ChatGPT Work and open Plugins to browse, install, and use plugins. A plugin can prompt you to connect an external service before its tools become available.

In Codex CLI, enter `/plugins` to open the plugin browser. Install a plugin from a configured marketplace, then start a new session before using its bundled skills or tools.

In the IDE extension, open Settings > Plugins to browse and install plugins for the connected Codex host. Start a new chat after installation before using the plugin's bundled skills or tools.

Extend what ChatGPT can do, for example:

- Install the Codex Security plugin to scan authorized code and confirm plausible vulnerability findings.
- Install the Gmail plugin to let ChatGPT read and manage Gmail.
- Install the Google Drive plugin to work across Drive, Docs, Sheets, and Slides.
- Install the Slack plugin to summarize channels or draft replies.

A plugin can contain one or more of these parts:

- Skills: reusable instructions for specific kinds of work. ChatGPT can load them when needed so it follows the right steps and uses the right references or helper scripts for a task.
- Apps: connections to tools like GitHub, Slack, or Google Drive, so ChatGPT can read information from those tools and take actions in them. Apps expose tools and can optionally include custom ChatGPT UI.
- MCP servers: services that give ChatGPT access to more tools or shared information, often from systems outside your local project. They're also the services behind apps. They define tools, enforce auth, return structured data, and perform actions against external systems.
- Browser extensions: browser capabilities that a plugin needs for its workflow.
- Hooks: commands that run at configured lifecycle points. Review and trust plugin hooks before you enable them.
- Scheduled task templates: reusable starting points for recurring tasks where scheduled tasks are available.

You can share plugins by publishing them through a marketplace source, such as a repo marketplace for a project or team. See Build plugins (/codex/build-plugins) for marketplace setup, packaging, and distribution guidance.

If you are building an integration, start with Build an app (/codex/build-app). If the app needs custom ChatGPT UI, use the Apps SDK (https://developers.openai.com/apps-sdk) for that optional UI layer.

### Use and install plugins

#### Plugins in ChatGPT

To browse and install curated plugins:

- On the web, select Work and open Plugins.
- In the ChatGPT desktop app, select Work or Codex and open Plugins.

The plugin directory organizes plugins into tabs:

- OpenAI: plugins built by OpenAI.
- Your workspace name: plugins provided by your workspace.
- Personal: personal marketplace plugins, including Created by me and Shared with me sections when those plugins are available.

Use the separate Installed row to review plugins you already installed.

#### Install and use a plugin in ChatGPT

Once you open the plugin directory:

1. Search or browse for a plugin, then open its details.
2. Select the plus button to install the plugin.
3. If the plugin needs a connector, connect it when prompted. Some plugins ask you to authenticate during install. Others wait until the first time you use them.
4. After installation, start a new task and ask ChatGPT to use the plugin.

After you install a plugin, you can use it directly in the prompt window:

Describe the task directly

Ask for the outcome you want, such as "Summarize unread Gmail threads from today" or "Pull the latest launch notes from Google Drive."

Use this when you want ChatGPT to choose the right installed tools for the task.

Choose a specific plugin

Type `@` to invoke the plugin or one of its bundled skills explicitly.

Use this when you want to be specific about which plugin or skill ChatGPT should use. See Skills & Plugins (/codex/skills-and-plugins).

#### Plugin directory in Codex CLI

In Codex CLI, run the following command to open the plugin browser:

```
codex
/plugins
```

The CLI plugin browser groups plugins by marketplace. Use the marketplace tabs to switch sources, open a plugin to inspect details, install or uninstall marketplace entries, and press Space on an installed plugin to turn it on or off.

#### Plugin directory in the IDE extension

Open the extension settings and select Plugins. Browse or search the directory, open a plugin to inspect its details, and install it for the selected Codex host. Start a new chat before using the plugin's bundled skills or tools.

#### How permissions and data sharing work

On ChatGPT web, hosted Work conversations use the workspace permissions and tools available to that conversation. Connectors still require their own sign-in and access.

When a plugin capability runs through a Codex host, the host's sandbox and approval policy applies. Connections to external services use that service's own authentication and access controls.

- Bundled skills become available when you start a new chat or CLI session after installation.
- If a plugin includes connectors, ChatGPT may prompt you to install or sign in to those connectors in ChatGPT during setup or the first time you use them.
- If a plugin includes MCP servers, they may require extra setup or authentication before you can use them.
- When ChatGPT sends data through a bundled connector, that service's terms and privacy policy apply.

#### Remove a plugin

To remove a plugin, reopen it from the plugin browser and select Uninstall plugin when that action is available. Workspace-installed or default plugins may not offer that action; your workspace administrator controls them instead.

Uninstalling a plugin removes the plugin bundle from that ChatGPT or Codex environment, but bundled connectors stay connected until you manage them in ChatGPT.

### Build your own plugin

If you want to create, test, or distribute your own plugin, see Build plugins (/codex/build-plugins). That page covers local scaffolding, manual marketplace setup, workspace sharing, plugin manifests, and packaging guidance.

If your plugin includes an app, see Build an app (/codex/build-app). Apps are MCP-backed integrations. They can work with tools alone, or add custom ChatGPT UI through the Apps SDK (https://developers.openai.com/apps-sdk) when a visual surface helps the workflow.

When your plugin is ready for review, see Submit plugins (/codex/submit-plugins) for the OpenAI Platform submission flow, required permissions, review materials, MCP checks, and test case requirements.

### Plugin guides

- Record & Replay (/codex/extend/record-and-replay): Show ChatGPT a workflow once and turn it into a reusable skill.
- Codex Security plugin (/codex/security/plugin): Scan authorized code, confirm findings, and prepare reviewed fixes.

---

## Web search

Search the web while you work in ChatGPT

### Overview

ChatGPT includes a first-party web search tool. All web results should be treated as untrusted input.

### Using web search across platforms

ChatGPT desktop app: Request current information within a task, and ChatGPT will record search activity alongside other tool calls in the transcript.

ChatGPT web: Ask for current information or sources. Search results and citations display in the conversation when ChatGPT performs web searches. Workspace settings may restrict search availability.

CLI: Use the `--search` flag to fetch live results for a single execution:

```
codex --search "Summarize the latest release notes for this dependency"
```

Searches appear as `web_search` items in the interactive transcript and `codex exec --json` output.

IDE extension: Request web searches while working in the editor. The extension uses the connected Codex host's search configuration, with search activity appearing in the conversation transcript.

### Configuring local web search

Local Codex tasks enable cached search by default, using an OpenAI-maintained index rather than fetching arbitrary pages live. This approach reduces, though doesn't eliminate, prompt injection risks.

For tasks requiring the most recent information, set `web_search = "live"` in `config.toml`. Disable the tool entirely with `web_search = "disabled"`. The `"indexed"` mode restricts external web access to search index-gated requests. When Codex runs with full access, web search defaults to live results.

For network boundaries in Codex cloud environments, see the Internet access documentation.

---

## Image generation

Generate and edit images in ChatGPT

Ask ChatGPT to generate or edit images. Use image generation for UI assets, banners, backgrounds, illustrations, sprite sheets, and placeholders you want to create alongside code or in a ChatGPT conversation.

### Generate or edit an image

Describe the image in natural language. Add a reference image when you want ChatGPT to transform or extend an existing asset.

Include `$imagegen` in your prompt to invoke the image generation skill explicitly.

Built-in image generation uses `gpt-image-2` and counts toward your general Codex usage limits. Image generations use included limits 3 to 5 times faster on average than similar turns without image generation, depending on image quality and size. For larger batches, set `OPENAI_API_KEY` in your environment and ask ChatGPT to generate images through the API so API pricing applies.

Image availability and usage limits in ChatGPT web depend on your plan and workspace settings. For programmatic image generation, use the Image generation API.

### Write effective image prompts

A useful image prompt is often only one to three clear sentences. Describe the details that determine whether the result succeeds:

- Explain the image's purpose or intended audience
- Name the main subject and what is happening
- Describe the setting, composition, and visual style
- Add framing, dimensions, lighting, colors, or materials when they matter
- State constraints, including anything the image must not contain

Prefer concrete visual language over broad reactions. For example, describe where light comes from instead of asking for "beautiful lighting." Repeat any requirement that must stay fixed.

### Refine the result

Start with the core idea, then make small, targeted revisions. Adjust one element at a time so the composition and other important details do not drift. You can also select a specific area of an image and describe the change for that area.

When editing an existing image, state exactly what should change and what must stay the same.

For broader revisions, keep feedback direct and actionable: make the image brighter, reduce color saturation, simplify the background, or keep the composition while changing the style.

### Use multiple reference images

Use a small set of reference images when one image defines the content and another defines the style, layout, or other visual direction. Identify each image by order and explain how the images relate. Use spatial terms such as foreground, background, left, and right when combining elements.

### Add text to an image

Keep in-image text short and specify it precisely. Put the exact text in quotation marks, preserve the capitalization you want, and describe its font style, size, color, and placement. For an uncommon name, spell out the letters when accuracy matters. State whether any other text is allowed.

### Create infographics and dense layouts

Image generation can help draft explainers, posters, labeled diagrams, timelines, and other information-rich visuals. Describe the information hierarchy and layout, keep labels concise, and request sharp text rendering. For dense copy or production-critical typography, review every word and finish the asset in a design tool when needed.

### Additional considerations

- Use likenesses with care. When depicting a real person, provide a reference photo when appropriate and confirm that you have permission to use their likeness.
- Ask for an original treatment. Request a generic or original design instead of imitating a specific brand, product, artist, or artwork.
- Credit is optional. You do not need to credit OpenAI for generated images, though you can explain how an asset was made when that context is useful.
- Follow applicable policies. Use images in accordance with your organization's guidelines and OpenAI's usage policies.

---

## Image inputs

Attach screenshots, diagrams, and visual references to ChatGPT prompts

### Overview

Add images to a prompt when the task depends on visual context, such as an error screenshot, interface design, architecture diagram, or existing asset. Explain what ChatGPT should inspect and what outcome you want; don't rely on the image alone to communicate the task.

### ChatGPT desktop app

Drag an image into the prompt composer while holding Shift to include it as context. You can also ask ChatGPT to inspect an image on your system or use a screenshot tool to verify work in another app.

### ChatGPT web

Attach, paste, or drag an image into the ChatGPT web composer. In the prompt, tell ChatGPT what to inspect and what result you want from the image.

### Codex CLI

Paste an image into the interactive composer, or pass one or more files on the command line:

```bash
codex -i screenshot.png "Explain this error and suggest the smallest fix"
codex --image before.png,after.png "Compare these states and list the regressions"
```

For multiple images, separate paths with commas or repeat `--image`. Codex accepts common image formats, including PNG and JPEG.

### Chrome extension

Drag an image into the prompt composer while holding Shift so the extension accepts the drop instead of passing it to the editor.

### Write the prompt around the image

Name what the image shows, point to the area that matters, and state the output and constraints. If you attach more than one image, identify each one and explain how ChatGPT should compare them.

For example:

```
Compare this checkout screen with the design. Fix spacing and typography only;
do not change behavior. Verify the result with a new screenshot.
```

### Use the right image feature

Use an image input when you want ChatGPT to inspect a visual reference. Use image generation when you want ChatGPT to create or edit an image.

---

## Appshots

### Give ChatGPT context from any Mac app

Appshots let you send the frontmost app window to a task in ChatGPT. Use them when you're actively working in another app on your computer and want to provide ChatGPT with your current context so it can help you with the task.

Appshots are available in the ChatGPT desktop app on macOS. Press both Command keys, or your custom Appshots hotkey, to take one.

### What appshots capture

An appshot captures the frontmost window only. It can include:

- An image of the visible window.
- Available text from that window, including visible text and text the app makes available outside the visible scroll area.

After you add an appshot to a task, it behaves like an attachment. ChatGPT stores appshots locally in the session file, like files or images you attach manually.

### When to use appshots

Use appshots when ChatGPT needs context from a Mac app before it can act.

Examples:

- Share an API reference page and ask ChatGPT to write a script that uses it.
- Share an email or calendar view and ask ChatGPT to draft the next step.
- Share an image editor, design, or preview window and ask ChatGPT to revise the related assets or code.
- Share an error, settings panel, or app state that's easier to show than describe.

### Take an appshot

1. Bring the app window you want to share to the front.
2. Press both Command keys, or the custom hotkey you configured in ChatGPT settings.
3. Allow macOS permissions if ChatGPT asks.
4. Ask ChatGPT to perform a task with the appshot.

By default, ChatGPT starts a new task for the appshot. If you interacted with a task in the last 60 seconds, ChatGPT adds the appshot to that recent task instead. Taking consecutive appshots adds them to the same task.

You can change the Appshots hotkey in the app settings.

### Permissions and safety

ChatGPT may ask for permissions before it can take appshots:

- Screen & System Audio Recording lets ChatGPT capture an image of the frontmost window.
- Accessibility lets ChatGPT read available text from the frontmost window.

Taking an appshot shares the captured image and available text with ChatGPT. Avoid taking appshots of sensitive content unless the task requires that content.

Review appshots the same way you would review sharing screenshots and documents with ChatGPT.

### Limits and troubleshooting

Appshots are available in the ChatGPT desktop app on macOS. If you resume a task in the CLI that already contains an appshot, the attachment is part of the task history, but the CLI can't create a new appshot.

For some apps and websites, including Google Docs, Gmail, Google Sheets, and Google Slides, ChatGPT may receive only the visible screenshot and may not receive the full document or off-screen text. In ChatGPT Work or Codex, ChatGPT can use a matching installed plugin to access the relevant app content and help with your request.

If appshots don't work:

1. Open System Settings > Privacy & Security.
2. Check Screen & System Audio Recording and Accessibility for Codex Computer Use.
3. Restart the app and try again.

---

## Chrome extension

Use Chrome with ChatGPT while managing browser permissions, website approvals, and browsing data

### Overview

ChatGPT integrates with Chrome to handle browser tasks requiring your signed-in browser state. This is particularly useful for accessing services like LinkedIn, Salesforce, Gmail, or internal tools that require authentication.

For development servers, file-backed previews, and public pages without sign-in requirements, the built-in browser is recommended. ChatGPT intelligently switches between tools, using plugins for dedicated integrations, Chrome for logged-in contexts, and the built-in browser for localhost.

Important: Review websites before permitting ChatGPT to continue, treating page content as untrusted context.

### Setup Process

#### Install Chrome Plugin

1. Open the ChatGPT desktop app and select ChatGPT Work or Codex
2. Navigate to Plugins and add the Chrome plugin
3. Follow the setup flow to install the Chrome extension and approve permission prompts
4. Verify the extension displays Connected status in Chrome

Once configured, ChatGPT suggests Chrome when tasks require signed-in websites. You can also invoke it directly:

```
@Chrome open Salesforce and update the account from these call notes.
```

Chrome tasks run in tab groups, keeping work organized by task.

### Website Access Control

By default, ChatGPT requests confirmation before interacting with new websites based on domain hosts (e.g., `example.com`).

#### Response Options

When prompted about website access:

- Allow for current task only
- Always allow the domain without future prompts
- Decline access

#### Allowlist and Blocklist Management

In Computer Use settings, manage domain access:

- Allowlist: Domains ChatGPT uses without asking
- Blocklist: Domains ChatGPT cannot access

Removing entries restores the prompt or blocks behavior.

##### Always Allow Browser Content

This elevated-risk option eliminates confirmation prompts for all websites.

##### Browser History Access

Browser history may contain sensitive telemetry, internal URLs, and search terms. Enabling this elevated-risk feature allows ChatGPT to use relevant history entries as context, though malicious content increases data exposure risks.

### Data and Security

#### Chrome Extension Permissions

Installation requires accepting permissions including:

- Page debugger access
- Read/modify website data
- Browse history access on signed-in devices
- Notifications display
- Bookmark management
- Download management
- Native application communication
- Tab group management

These permissions enable browser workflows while ChatGPT applies its own confirmations and controls.

#### Memories Integration

Computer Use respects your Memories setting. With Memories enabled, ChatGPT can reference saved memories during Chrome tasks. Disabled Memories prevents their use.

#### Data Storage

OpenAI does not maintain separate records of Chrome actions. Activity is stored only when included in ChatGPT context (page text, screenshots, tool calls, summaries, messages). Your ChatGPT data controls apply to processed content. Avoid sending secrets or highly sensitive data unless necessary and you review each prompt.

### Troubleshooting

If ChatGPT cannot connect to Chrome:

1. Check blocklist: Verify the target website isn't blocked in Settings
2. Verify extension status: Open the extension and confirm Connected appears. If disconnected or missing native host, remove and re-add the Chrome plugin from Plugins, then follow setup again
3. Enable plugin: In Work or Codex Plugins, ensure Chrome plugin is active
4. Verify profile: Confirm you're using the Chrome profile where the extension is installed (multiple profiles require per-profile installation)
5. Restart task: Start a new task in Work or Codex to clear connection state
6. Restart browsers: Restart Chrome and the app, then retry. If issues persist, uninstall the extension, remove and re-add the plugin, and follow setup
7. Contact support: If the extension shows Connected but ChatGPT still cannot use Chrome, run `/feedback` and include the task ID

#### File Upload Configuration

For Chrome tasks involving file uploads:

1. In Chrome, open the extensions icon and select Manage Extensions
2. Click Details on the extension card
3. Enable Allow access to file URLs

Restart the Chrome task after changing this setting.

---

## Work with files

Create, preview, and refine documents, presentations, spreadsheets, and PDF files in ChatGPT

### Overview

When a task produces a file, provide ChatGPT with the source data, expected file type, structure, and review criteria relevant to your work. Preview and review capabilities vary by platform.

The ChatGPT desktop app displays generated documents, presentations, spreadsheets, and PDFs within the chat interface. You can use annotations to highlight specific sections and request targeted revisions.

In ChatGPT web, you can upload source files or request document creation. Review generated files in the conversation, download them as needed, and provide feedback for iterations.

Codex CLI creates and edits files in your working directory without visual preview capabilities. Request that Codex report output paths and validation steps performed.

The IDE extension manages files within your workspace. Text and code files display in the editor, while formatted documents open in compatible viewers.

### Create files for review

For spreadsheets and presentations, specify the sheets, columns, charts, slide sections, and validation checks you require. Ask ChatGPT to document where it saved outputs and what verification steps it completed.

### Refine files with annotations

Annotations enable you to indicate a specific area and request modifications. This workflow applies to code, Markdown, websites, documents, spreadsheets, and presentations.

Examples include:

- Selecting a navigation bar and requesting font changes
- Highlighting a statement to request source verification
- Marking a chart to request clearer labeling

ChatGPT uses your selection as context, allowing refinement without restarts or disrupting completed sections.

### Review and refine files on the web

Download or open generated files in appropriate viewers. When requesting revisions, specify the page, slide, sheet, or section needing changes while identifying what should remain unchanged. Request that ChatGPT report new filenames and verification steps before download.

### Review and refine files

Monitor the task sidebar during execution to access the agent's plan, sources, generated files, and summary. This allows you to guide progress and inspect outputs before requesting additional passes.

Request explanations of file locations and verification methods. Use previews to examine output, then provide focused feedback on structure, data, layout, or validation needs.

---

## Commands

Reference for commands and keyboard shortcuts in the ChatGPT desktop app

Use these commands and keyboard shortcuts to navigate the app.

### Keyboard shortcuts

| Action | Shortcut |
|--------|----------|
| General | |
| Command menu | Cmd/Ctrl + Shift + P or Cmd/Ctrl + K |
| Settings | Cmd/Ctrl + , |
| Keyboard shortcuts | Cmd/Ctrl + Shift + / |
| Open folder | Cmd/Ctrl + O |
| Navigate back | Cmd/Ctrl + [ |
| Navigate forward | Cmd/Ctrl + ] |
| Increase font size | Cmd/Ctrl + + |
| Decrease font size | Cmd/Ctrl + - |
| Toggle sidebar | Cmd/Ctrl + B |
| Open review tab | Ctrl + Shift + G |
| Toggle review panel | Cmd/Ctrl + Alt + B |
| Toggle bottom panel | Cmd/Ctrl + J |
| Toggle terminal | Ctrl + ` |
| Clear the terminal | Ctrl + L |
| Chat | |
| Quick Chat | Cmd/Ctrl + Alt + N |
| Task | |
| New task | Cmd/Ctrl + N or Cmd/Ctrl + Shift + O |
| Search tasks | Cmd/Ctrl + G |
| Find in task | Cmd/Ctrl + F |
| Previous task | Cmd/Ctrl + Shift + [ |
| Next task | Cmd/Ctrl + Shift + ] |
| Input | |
| Dictation | Ctrl + Shift + D |

To find, customize, or reset shortcuts, open Settings > Keyboard Shortcuts. You can search by command name or switch the search field into keystroke mode and press the shortcut you want to find.

### Search past tasks and find in a task

Use task search (Cmd/Ctrl + G) to reopen a past task. When expanded matching is available, it can also match task content and Git branch names, so you can search for a phrase from the task or a branch such as `fix/login-redirect`.

Use Find in task (Cmd/Ctrl + F) after opening a task to find text within it. It doesn't search across other tasks.

For actions that start with `/`, see Slash commands (/codex/reference/slash-commands).

### Deep links

The ChatGPT desktop app keeps the `codex://` URL scheme for compatibility, so links can open specific parts of the app directly. Encode query string values before adding them to a URL.

#### Supported links

Use these canonical forms when you create links. The sections below list the full reference by link type.

| Deep link | Opens |
|-----------|-------|
| `codex://threads/new` | A new local task. |
| `codex://new?<query>` | A new local task with at least one query parameter. |
| `codex://threads/<thread-id>` | A local task. `<thread-id>` is its technical thread ID. |
| `codex://settings` | Settings. |
| `codex://settings/connections/<connection-type>` | Computer, device, or SSH connection settings. |
| `codex://settings/connections/ssh/add?name=<ssh-config-host>` | Adds a host from your SSH config to Codex. |
| `codex://skills` | Skills. |
| `codex://automations` | Scheduled with the create flow open. |
| `codex://plugins/install/<plugin-name>?marketplace=<marketplace-name>` | The install flow for a plugin from a known marketplace. |
| `codex://plugins/<plugin-id>` | A plugin detail page. |
| `codex://plugins/<plugin-name>?marketplacePath=<absolute-marketplace-path>` | A local plugin detail page from a local marketplace. |
| `codex://pets/install?name=<pet-name>&imageUrl=<https-image-url>` | The pet install flow. |

#### Tasks

Use these links when you need to open an existing local task or start a new one.

| Deep link | Opens |
|-----------|-------|
| `codex://threads/<thread-id>` | A local task. `<thread-id>` is its technical thread ID. |
| `codex://threads/new` | A new local task. |
| `codex://threads/new?<query>` | A new local task with optional query parameters. |
| `codex://new?<query>` | A new local task. Include at least one of `prompt`, `path`, or `originUrl`; otherwise the link does nothing. |

For `codex://threads/new` or `codex://new`, add any of these query parameters as needed; you can combine them in the same URL.

| Query parameter | Required | What it does |
|-----------------|----------|--------------|
| `prompt=<text>` | No | Sets the initial composer text. |
| `path=<absolute-path>` | No | Opens the new task in a local workspace. `path` must be an absolute path to a local directory. When valid, Codex uses that directory as the active workspace. |
| `originUrl=<git-remote-url>` | No | Matches one of your current workspace roots by Git remote URL. If `path` is also present, Codex resolves `path` first. |

Example: Show me some fun stats about how I've been using Codex (codex://threads/new?prompt=Show%20me%20some%20fun%20stats%20about%20how%20I%27ve%20been%20using%20Codex)

##### Start a task with a plugin

To help users start a plugin-backed task, include a plugin mention in the prompt before you encode it:

```
[@Example](plugin://example@openai-curated) Summarize this document: https://example.com/document/123
```

Encode the complete prompt as a URI component, for example with `encodeURIComponent` in JavaScript, and pass it to the `prompt` parameter:

```
codex://new?prompt=%5B%40Example%5D(plugin%3A%2F%2Fexample%40openai-curated)%20Summarize%20this%20document%3A%20https%3A%2F%2Fexample.com%2Fdocument%2F123
```

The link opens a new task with the decoded prompt in the composer. It doesn't send the prompt automatically. After the user sends it, Codex can use an installed plugin in that task. If the plugin isn't installed but is available to the user, Codex asks the user to install it and connect any required apps. After setup, the user can select Continue to resume the same task. Workspace settings can limit which plugins a user can install. For plugin installation and permission details, see Plugins (/codex/plugins).

#### Settings

Use these links when you need to open Settings or a specific settings page.

| Deep link | Opens |
|-----------|-------|
| `codex://settings` | Settings. |
| `codex://settings/browser-use` | Browser settings. |
| `codex://settings/computer-use/google-chrome` | Google Chrome settings for computer use. |
| `codex://settings/connections` | Remote connections settings. |
| `codex://settings/connections/computer` | Settings for controlling this Mac or PC from another device. |
| `codex://settings/connections/devices` | Settings for controlling other devices. |
| `codex://settings/connections/ssh` | SSH connection settings. |
| `codex://settings/connections/ssh/add?name=<ssh-config-host>` | Adds the named host alias as a Codex-managed connection, then opens SSH connection settings. |

The `name` value must match a host alias in `~/.ssh/config`. The link disables automatic connection for the added host. If Codex can't find the named host, it opens SSH connection settings and shows an error.

Unsupported `codex://settings/...` paths open the main Settings page.

#### Skills

Use these links when you need to open Skills.

| Deep link | Opens |
|-----------|-------|
| `codex://skills` | Skills. |

#### Scheduled

Use these links when you need to open Scheduled.

| Deep link | Opens |
|-----------|-------|
| `codex://automations` | Scheduled with the create flow open. |

#### Plugins

Plugin links use different forms depending on whether you are installing from a marketplace, opening a plugin, or working from a local `marketplace.json`. For plugin basics, see Plugins (/codex/plugins). For local or repo marketplace setup, see Build plugins (/codex/build-plugins#build-your-own-curated-plugin-list).

##### Plugin install

Use this form to open the install flow for a plugin from a marketplace that Codex already knows about.

| Deep link | Opens |
|-----------|-------|
| `codex://plugins/install/<plugin-name>?marketplace=<marketplace-name>` | The plugin detail or install flow for a plugin. |

| Query parameter | Required | What it does |
|-----------------|----------|--------------|
| `marketplace=<marketplace-name>` | Yes | Identifies the marketplace. For an OpenAI-curated plugin, use `openai-curated`. |

The install link accepts only the `marketplace` query parameter. If Codex can't find the requested marketplace or plugin, it opens the Plugins page instead.

##### Plugin detail

| Deep link | Opens |
|-----------|-------|
| `codex://plugins/<plugin-id>` | A plugin detail page. |

`<plugin-id>` must identify the plugin. For an OpenAI-curated plugin, use the form `<plugin-name>@openai-curated`.

Codex-generated plugin links can also include these query parameters. Omit both when you write a link manually.

| Query parameter | Required | What it does |
|-----------------|----------|--------------|
| `hostId=<host-id>` | No | Identifies the Codex host that owns the plugin context, such as `local` or one of your configured remote connections. Codex provides these IDs. |
| `source=manage` | No | Preserves the app's plugin-management entry point. It's not admin-only. |

Example: Open the OpenAI Developers plugin (codex://plugins/openai-developers@openai-curated)

##### Local plugin

For local or repo marketplace setup, see Build plugins (/codex/build-plugins#build-your-own-curated-plugin-list).

| Deep link | Opens |
|-----------|-------|
| `codex://plugins/<plugin-name>?marketplacePath=<absolute-marketplace-path>` | A local plugin detail page from a local marketplace. |

| Query parameter | Required | What it does |
|-----------------|----------|--------------|
| `marketplacePath=<absolute-marketplace-path>` | Yes | Absolute path to the local `marketplace.json`, for example `/Users/alex/.agents/plugins/marketplace.json`. |
| `mode=share` | No | Opens the share flow for that local plugin. |

#### Pets

Use these links to open the pet install flow when that feature is enabled.

| Deep link | Opens |
|-----------|-------|
| `codex://pets/install?name=<pet-name>&imageUrl=<https-image-url>` | The pet install flow. |

| Query parameter | Required | What it does |
|-----------------|----------|--------------|
| `name=<pet-name>` | Yes | Sets the pet name. The value must contain at least one non-whitespace character. |
| `imageUrl=<https-image-url>` | Yes | Provides an absolute HTTPS URL for the pet image or sprite sheet. |
| `description=<text>` | No | Adds a description to the install flow. |
| `spriteVersionNumber=<1-or-2>` | No | Selects the sprite-sheet format. The default is `1`; the only other supported value is `2`. |

The install link accepts only these query parameters. Invalid names, non-HTTPS image URLs, unsupported sprite versions, or extra path segments cause the link to do nothing.

### See also

- Features (/codex/features)
- Settings (/codex/reference/settings)
- Slash commands (/codex/reference/slash-commands)

---

## Slash commands

Reference for slash commands in the ChatGPT desktop app

Slash commands enable you to perform actions directly within the task composer without switching contexts. The specific commands available depend on your setup and permissions.

### How to use a slash command

1. In the task composer, type `/`
2. Select from the displayed list or continue typing to narrow down options (e.g., `/status`)

You can also activate skills explicitly by typing `$` in the composer. Visit Skills & Plugins (/codex/skills-and-plugins) for more details.

Active skills and custom prompts (displayed as `/prompts:<name>`) appear within the slash command list.

### Complete slash command reference

| Command | Function |
|---------|----------|
| `/approve` | Authorize a single retry after an automatic-review block |
| `/cloud` | Execute the task in cloud when available |
| `/cloud-environment` | Select your cloud execution environment |
| `/compact` | Compress current task context |
| `/fast` | Toggle Fast service tier availability |
| `/feedback` | Launch feedback interface with optional log inclusion |
| `/fork` | Duplicate a task locally or across worktrees |
| `/goal` | Establish an enduring objective for ChatGPT to pursue |
| `/ide-context` | Toggle IDE context sharing |
| `/init` | Build an `AGENTS.md` template for your project |
| `/local` | Execute in your selected local project |
| `/mcp` | Display MCP server connections |
| `/memories` | Configure memory access and generation settings |
| `/model` | Select the task's operating model |
| `/pet` | Activate or deactivate the desktop companion |
| `/personality` | Select ChatGPT's communication style |
| `/plan` | Activate multi-step planning mode |
| `/project` | Select project for upcoming tasks |
| `/reasoning` | Adjust reasoning depth for current task |
| `/review` | Initiate code inspection for uncommitted work |
| `/side` | Begin isolated conversation without affecting main task |
| `/status` | Display task identifier, context consumption, and limits |
| `/task` | Initiate a standalone task |
| `/worktree` | Run in a fresh Git worktree |

### Using `/goal` for persistent objectives

Invoke `/goal` in the app composer to activate Goal mode, a framework for ChatGPT to maintain focus on a defined objective throughout execution. Consider using `/plan` beforehand to develop your goal statement collaboratively with ChatGPT.

When active, the progress displays above the composer with controls to pause/resume, modify, or abandon the goal without additional commands. You can provide ongoing guidance through messages while the objective progresses.

For best practices on crafting effective goals, consult Goal mode (/codex/prompting#goal-mode).

---

## Settings

Configure preferences in the ChatGPT desktop app

Access the settings panel via the app menu or by pressing Cmd+, (macOS) or Ctrl+, (Windows) to personalize your application experience.

### General

Customize input behavior by requiring Cmd+Enter for multiline prompts. Enable "Prevent sleep while running" to allow local tasks to continue executing when your device would normally sleep. The "Follow-up behavior" option lets you determine whether messages sent during task execution should influence the current run or queue for the next one.

### Profile

Review your activity analytics including lifetime tokens, peak token usage, streaks, longest task duration, and token consumption patterns. Update personal details such as profile picture, display name, and username. Eligible users can generate and share profile cards highlighting usage metrics. Depending on your plan status, you can send invitations to friends or coworkers through the profile menu.

### Keyboard Shortcuts

Access the keyboard shortcuts panel to view all available commands and customize key bindings. Use the search functionality to locate shortcuts by command name, or switch to keystroke search mode to identify which command uses a specific key combination. Reset any customized shortcuts to their default values as needed.

### Notifications

Configure when task completion notifications appear and manage whether the app requests notification permissions from your system.

### Appearance

Modify the visual presentation by selecting a base theme, adjusting accent colors, background tones, and foreground colors. Customize both UI and code display fonts. Share your custom theme configurations with other users.

### Pets

Optional animated companions available for the desktop app. Select from built-in or custom pets, then control the floating overlay using `/pet` command, the "Wake Pet," or "Tuck Away Pet" options.

### Browser

Install or enable the bundled Browser plugin and set up the Chrome extension. Manage website allowlists and blocklists—ChatGPT requests permission before accessing sites unless previously approved. Remove blocked sites to re-enable permission prompts.

### Computer Use

Review desktop access settings configured during initial setup. On macOS, revoke system-level access through Privacy & Security settings by adjusting Screen Recording or Accessibility permissions.

### Personalization

Select your default personality from "Friendly," "Pragmatic," or "None" to disable personality instructions. Add custom instructions that update your personal instructions within the configuration system.

### Suggested Prompts

Enable context-aware suggestions that surface relevant follow-ups and tasks you may want to resume when starting or returning to the application.

### Memories

Enable Memories functionality to allow the system to maintain useful context from previous conversations and tasks for use in future sessions.

### Archived Tasks

View a list of archived tasks with associated dates and project context. Use the "Unarchive" option to restore previously archived tasks.

### Keep a Task Near Your Work

Pop out active tasks into separate windows for side-by-side placement with browsers, editors, or design tools. Enable "Always on top" to maintain task visibility while working in other applications.

---

## Troubleshooting

### Frequently Asked Questions

#### Files appear in the side panel that Codex didn't edit

When your project exists within a Git repository, the review panel automatically displays changes based on Git state, including modifications Codex didn't make. You can toggle between staged and unstaged changes, and compare your branch against main. To view only your last Codex turn's changes, switch to the **Last turn** view.

#### Remove a project from the sidebar

Hover over a project name, click the three dots menu, and select "Remove." Use the **Add new project** button or press Cmd+O to restore it.

#### Find archived tasks

Access archived tasks through [Settings](codex://settings). Unarchiving restores tasks to their original sidebar locations.

#### Only some tasks appear in the sidebar

Select the filter icon next to **Tasks** and choose **Chronological**. If tasks remain missing, check [Settings](codex://settings) for archived tasks.

#### Code doesn't run on a worktree

Worktrees inherit Git-tracked files but may require setup scripts via [local environments](/codex/environments/local-environment) or copying ignored files using [`.worktreeinclude`](/codex/environments/git-worktrees). Alternatively, checkout changes in your regular project.

#### App doesn't pick up a teammate's shared local environment

The local environment configuration must be in the `.codex` folder at your project root. For monorepos, open the specific project directory containing the `.codex` folder.

#### Codex asks to access Apple Music

Codex may need file system navigation, and certain macOS directories (Music, Downloads, Desktop) require user approval. Approving home directory access resolves this.

#### Scheduled tasks create many worktrees

Archive unneeded scheduled runs and avoid pinning runs unless keeping their worktrees. Frequent tasks accumulate worktrees over time.

#### Recover a prompt after selecting the wrong target

If you accidentally chose the wrong target (**Local**, **Worktree**, or **Cloud**), cancel the current run and press the up arrow key in the composer to recover your prompt.

#### Feature is working in the Codex CLI but not in the ChatGPT desktop app

Different surfaces may bundle different Codex versions, with experimental features sometimes reaching CLI first.

Check versions with:

```bash
codex --version
```

For the desktop app's bundled version:

```bash
/Applications/Codex.app/Contents/Resources/codex --version
```

### Feedback and logs

Type `/` in the message composer to provide feedback. Sharing your session generates a session ID for team reference.

To report issues:

1. Search [existing GitHub issues](https://github.com/openai/codex/issues)
2. [Create a new bug report](https://github.com/openai/codex/issues/new?template=2-bug-report.yml)

Log locations:

- App logs (macOS): `~/Library/Logs/com.openai.codex/YYYY/MM/DD`
- Session transcripts: `$CODEX_HOME/sessions` (default: `~/.codex/sessions`)
- Archived sessions: `$CODEX_HOME/archived_sessions` (default: `~/.codex/archived_sessions`)

Review logs for sensitive information before sharing.

### Stuck states and recovery patterns

If a task appears stuck:

1. Check for pending approvals
2. Run basic terminal commands like `git status`
3. Start a focused new task

Press the up arrow key in the composer to recover lost prompts from cancelled worktree creation.

### Terminal issues

**Terminal appears stuck**

1. Close the terminal panel
2. Reopen with Ctrl+\`
3. Run basic commands like `pwd` or `git status`

Validate your current directory and branch first. If issues persist, wait for active tasks to complete and restart the app.

**Fonts aren't rendering correctly**

Configure the font in [Settings](codex://settings) under **Code font**. Codex applies this font consistently across the review pane, terminal, and code displays.
