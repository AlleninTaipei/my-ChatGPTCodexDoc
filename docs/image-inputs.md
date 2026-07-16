# Image inputs

Attach screenshots, diagrams, and visual references to ChatGPT prompts

## Overview

Add images to a prompt when the task depends on visual context, such as an error screenshot, interface design, architecture diagram, or existing asset. Explain what ChatGPT should inspect and what outcome you want; don't rely on the image alone to communicate the task.

## ChatGPT desktop app

Drag an image into the prompt composer while holding Shift to include it as context. You can also ask ChatGPT to inspect an image on your system or use a screenshot tool to verify work in another app.

## ChatGPT web

Attach, paste, or drag an image into the ChatGPT web composer. In the prompt, tell ChatGPT what to inspect and what result you want from the image.

## Codex CLI

Paste an image into the interactive composer, or pass one or more files on the command line:

```bash
codex -i screenshot.png "Explain this error and suggest the smallest fix"
codex --image before.png,after.png "Compare these states and list the regressions"
```

For multiple images, separate paths with commas or repeat `--image`. Codex accepts common image formats, including PNG and JPEG.

## Chrome extension

Drag an image into the prompt composer while holding Shift so the extension accepts the drop instead of passing it to the editor.

## Write the prompt around the image

Name what the image shows, point to the area that matters, and state the output and constraints. If you attach more than one image, identify each one and explain how ChatGPT should compare them.

For example:

```
Compare this checkout screen with the design. Fix spacing and typography only;
do not change behavior. Verify the result with a new screenshot.
```

## Use the right image feature

Use an image input when you want ChatGPT to inspect a visual reference. Use image generation when you want ChatGPT to create or edit an image.
