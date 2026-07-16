# Pets

Choose an animated companion and follow task activity across supported interfaces

Pets are optional animated companions for following work. Where a pet appears and what it shows depend on the interface you use. Choosing a pet changes its appearance, not how ChatGPT completes tasks.

## Use a floating pet

In the ChatGPT desktop app, a pet can float above other app windows and help you follow activity across your tasks.

### Choose and wake a pet

1. Open the profile menu at the bottom of the app and select Pets. You can also open Settings and go to Pets.
2. Choose a built-in or custom pet.
3. Enter `/pet`, or open the command menu and select Wake Pet.

Select Tuck Away Pet in Settings > Pets or the command menu, or enter `/pet` again, to hide the pet. Your selection and the pet's position persist when you reopen the app.

When you select a custom pet, it also appears in your Profile view.

### Understand pet status

| Status | Meaning |
|--------|---------|
| Running | A task is actively working. |
| Needs input | A task needs your approval, answer, or another decision. |
| Ready | A task has completed and has unread activity. |
| Blocked | A task failed or encountered a system error. |

When more than one task has activity, the pet prioritizes tasks that need input, followed by blocked, ready, and running tasks. Open the activity tray to choose a task.

Select the pet to return to ChatGPT, or select an activity to open its task. The activity tray is separate from system notifications.

### Follow Computer Use

On macOS, the Computer Use picture-in-picture window can attach to an awake pet. Move the pet, and the window follows.

### Create a custom pet

1. Open Settings > Pets and select Create your own pet.
2. The app installs the bundled `hatch-pet` skill, reloads skills, and opens a new task.
3. Describe the pet you want and send the prompt.
4. When the task finishes, return to Settings > Pets, select Refresh, and choose your new pet.

Custom pets created in the desktop app are stored locally on your computer. They don't automatically sync to ChatGPT web.

### Reduce animation

Pets respect your operating system's reduced motion setting. When reduced motion is enabled, the pet uses a still frame instead of sprite animation.

## Choose a pet on the web

If Pets are available for your account and workspace, open Settings > Personalization > Pet > Select pet. Choose a built-in pet, or choose Default to use ChatGPT without a pet.

A web pet appears inside supported ChatGPT Work tasks. It doesn't provide the desktop app's floating overlay, activity tray, or `/pet` command.

### Upload a custom pet

Select Upload pet to add a custom sprite sheet. The file must be a transparent PNG or WebP, exactly 1536 x 1872 pixels, and no larger than 20 MiB. You can edit, download, refresh, or delete uploaded pets from the same setting.

## Choose a terminal pet

In an interactive Codex CLI session:

- Enter `/pets` or `/pet` to open the pet picker.
- Enter `/pets <name>` to choose a pet directly.
- Enter `/pets off` to disable terminal pets.

The picker includes built-in pets and compatible custom pets installed on your computer. A terminal pet reports activity for the current CLI session. It uses Running, Needs input, Ready, and Blocked states, but it doesn't provide the desktop app's multiple-task activity tray.

Terminal pets require iTerm2 3.6 or later, or a terminal with Kitty graphics or Sixel support. They are unavailable inside tmux and Zellij.

## Pets in the IDE extension

The Codex IDE extension doesn't provide a pet picker or floating pet overlay. Use the ChatGPT desktop app or Codex CLI when you want to use your own pet.

## Related docs

- Notifications
- Long-running work
- ChatGPT desktop app settings
