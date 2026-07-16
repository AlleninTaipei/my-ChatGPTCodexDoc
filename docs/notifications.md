# Notifications

Choose how you are notified when work needs attention

ChatGPT desktop app

Notifications let you know when work needs attention. Their controls and delivery channels vary by surface.

## Configure desktop notifications

Open **Settings** to choose whether turn-completion alerts appear never, only while ChatGPT is in the background, or always. Separate controls let you turn permission and question notifications on or off. Your operating system may ask you to grant notification permission to the ChatGPT desktop app.

### Follow task activity with a pet

In the ChatGPT desktop app, a floating pet is another way to follow task activity while you work in other apps. It can show when a task is **Running**, **Needs input**, **Ready**, or **Blocked**.

See [Pets](/codex/pets?surface=app) to choose a pet, understand its status, or create your own.

## Configure web notifications

Open **Settings > Notifications** to manage the notification categories and channels available to your account. Depending on the category and account, channels can include push, email, or SMS. Use **Manage tasks** from the task notification settings to open **Scheduled**.

## Configure CLI notifications

For terminal and external notifications, see [Notifications](/codex/config-file/config-advanced#notifications) in the advanced configuration guide. You can choose when the TUI emits a notification and whether Codex runs an external program when a turn completes.

## Follow task activity in the IDE

The IDE extension doesn't provide separate notification controls. Keep the task open to follow its activity. To run an external program when a turn completes, configure `notify` on the connected Codex host. See [Notifications](/codex/config-file/config-advanced#notifications) in the advanced configuration guide.

## Related docs

*   [Long-running work](/codex/long-running-work)
*   [Scheduled tasks](/codex/automations)
*   [Pets](/codex/pets)
