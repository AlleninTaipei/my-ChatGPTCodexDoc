# Remote connections

Use Remote to access work on a connected computer, or connect the ChatGPT desktop app to projects over SSH

Remote access uses the connected host's projects, tasks, files, credentials, permissions, plugins, Computer Use, browser setup, and local tools.

## What you can do remotely

- Start new tasks in projects on the host, or continue existing ones.
- Send follow-up instructions, answer questions, and steer active work.
- Approve commands and other actions.
- Review outputs, diffs, test results, terminal output, and screenshots.
- Get notified when ChatGPT completes a task or needs your attention.
- Switch between connected hosts and tasks.

## Before you set up Remote

Remote supports hosts running the ChatGPT desktop app on macOS and Windows. You can control a host from ChatGPT on iOS or Android, or from another Mac or Windows device when **Control other devices** is available.

Make sure you have:

- Codex access in the ChatGPT account and workspace you want to use.
- The latest ChatGPT mobile app on an iOS or Android device.
- The latest ChatGPT desktop app for macOS or Windows running on a host that's awake, online, and signed in to the same account and workspace.
- Any required multi-factor authentication, SSO, or passkey configuration for that account or workspace.

If you use Codex through a ChatGPT workspace, your admin may need to enable Remote Control access before you can connect from your phone.

## Set up Remote

Start in the ChatGPT desktop app on the host you want to connect. The setup flow enables remote access for that host, then shows a QR code you can scan from your phone.

1. Start Remote setup.

   Open the app on the host and select **Set up Remote** in the sidebar.

2. Scan the QR code.

   Use your phone to scan the QR code shown by the app.

3. Finish setup in ChatGPT.

   ChatGPT opens the Remote setup flow. Confirm the same ChatGPT account and workspace, then complete any required multi-factor authentication, SSO, or passkey steps.

4. Review host settings.

   In the app on the host, use **Settings > Connections** to manage connected devices.

## Choose what to connect

### Your laptop or desktop

Connect the Mac or Windows PC where the desktop app is already installed. This gives remote access to the same projects, tasks, credentials, plugins, and local setup you already use.

On a Mac laptop, remote access can stay available with the lid open and power connected. With the lid closed, connect an external display as well.

### A dedicated always-on computer

Use a dedicated always-on Mac or Windows PC when you want ChatGPT to stay reachable for longer-running work.

### A remote development environment

Use an SSH host or managed remote development environment when the project already lives in a remote environment.

## What comes from the connected host

Your phone sends prompts, approvals, and follow-up messages to ChatGPT. The connected host provides the environment ChatGPT uses.

That means:

- Repository files and local documents come from the connected host.
- Shell commands run on that host or remote environment.
- MCP servers, skills, browser access, and Computer Use come from that host's configuration.
- Signed-in websites and desktop apps are available only when the host can access them.

## Pick up work from another device

You can continue work from another signed-in device running the ChatGPT desktop app and supporting remote control.

## Connect to an SSH host

In the ChatGPT desktop app, add remote projects from an SSH host and run tasks against the remote filesystem and shell.

1. Add the host to your SSH config so Codex can auto-discover it.

```bash
Host devbox
  HostName devbox.example.com
  User you
  IdentityFile ~/.ssh/id_ed25519
```

2. Confirm you can SSH to the host from the machine running the app.

```bash
ssh devbox
```

3. Install and authenticate Codex on the remote host.

   The app starts the remote Codex app server through SSH, using the remote user's login shell.

4. In the app, open **Settings > Connections**, add or enable the SSH host, then choose a remote project folder.

## Hand off a task between hosts

Handoff moves an existing task and its Git state between your local computer and a connected remote host.

Before you hand off a task, connect the destination host and save a project for the same Git repository on that host.

To hand off a task:

1. Open the task in the desktop app.
2. In the task footer, select the current run location, then select the destination host.
3. Review the destination and branch, then select **Hand off**.

## Authentication and network exposure

Remote connections use SSH to start and manage the remote Codex app server. Don't expose app-server transports directly on a shared or public network.

If you need to reach a remote machine outside your current network, use a VPN or mesh networking tool instead of exposing the app server directly to the internet.

## Troubleshooting

### You don't see the host on your phone

Confirm that the desktop app is running on the host, you've enabled **Allow other devices to connect**, and both devices use the same ChatGPT account and workspace.

### Remote Control is off after you sign back in

Signing out of ChatGPT turns off **Remote Control**, but it doesn't remove your existing device pairings.

### The approval request doesn't appear

In the ChatGPT mobile app, open **Remote**. If you use a ChatGPT workspace, ask your admin to confirm that they've enabled Remote Control access.

### The remote session disconnects

Check whether the host went to sleep, lost network access, or closed the app. Keep the host awake and connected while ChatGPT works.

### Authentication blocks setup

Complete the account or workspace authentication prompt shown during setup. If setup still fails, ask your workspace admin to confirm that they've enabled Remote Control access.
