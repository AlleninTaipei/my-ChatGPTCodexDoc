# FAQ

Common questions about Codex Security

## Getting started

### What is Codex Security?

"Software security remains one of the hardest and most important problems in engineering. Codex Security is an LLM-driven security analysis toolkit that inspects source code and returns structured, ranked vulnerability findings with proposed patches."

### Why does it matter?

The tool addresses systemic risk by supporting a defender-first approach that continuously identifies potential issues, validates them when feasible, and recommends fixes without hindering development velocity.

### What business problem does Codex Security solve?

"Codex Security shortens the path from a suspected issue to a confirmed, reproducible finding with evidence and a proposed patch."

### How does Codex Security work?

The system operates within an isolated, temporary container that clones the target repository. It performs code-level analysis and generates structured findings including descriptions, locations, criticality levels, root causes, and remediation suggestions. For findings with verification steps, it executes proposed commands in the sandbox and records results as evidence.

### Does it replace SAST?

No, "Codex Security complements SAST. It adds semantic, LLM-based reasoning and automated validation, while existing SAST tools still provide broad deterministic coverage."

## Features

### What is the analysis pipeline?

The process follows four stages: building a threat model, reviewing commits, validating suspected vulnerabilities in a sandbox to minimize false positives, and proposing patches for reviewer inspection.

### What languages are supported?

"Codex Security is language-agnostic. In practice, performance depends on the model's reasoning ability for the language and framework used."

### What outputs do I get after the scan completes?

You receive ranked findings with criticality, validation status, and proposed patches when available, plus supporting evidence like crash output and call-path context.

### How is customer code isolated?

"Each analysis and validation job runs in an ephemeral Codex container with session-scoped tools. Artifacts are extracted for review, and the container is torn down after the job completes."

### Does Codex Security auto-apply patches?

No. "The proposed patch is a recommended remediation. Users can review it and push it as a PR to GitHub from the findings UI, but Codex Security does not auto-apply changes to the repository."

### Does the project need to be built for scanning?

No, scanning works from repository context alone, though the system may attempt builds during validation to reproduce issues.

### How does Codex Security reduce false positives?

It employs two stages: the model ranks suspected issues, then auto-validation attempts reproduction in a clean container. Successfully reproduced findings receive a validated status before human review.

### How long do initial scans take?

"Initial scan time depends on repository size, build time, and how many findings proceed to validation. For some repositories, scans can take several hours. For larger repositories, they can take multiple days."

### What is a threat model?

"A threat model is the scan-time security context for a repository. It combines a concise project overview with attack-surface details such as entry points, trust boundaries, auth assumptions, and risky components."

### How is a threat model generated?

The system prompts the model to summarize repository architecture, identify security entry points, classify the repository type, run specialized extractors, and consolidate results into a threat model artifact.

### Does it replace manual security review?

No, "Codex Security accelerates review and helps rank findings, but it does not replace code-level validation, exploitability checks, or human threat assessment."

### Can I edit the threat model?

Yes. The initial threat model can be updated as architecture and business context evolve.

### Do I need to configure a scan before using threat modeling?

Yes, "Threat-model guidance is tied to how and what you scan, so you need to configure the repository first."

### What does the proposed patch contain?

"The proposed patch contains a minimal actionable diff with filename and line context when a remediation can be generated for the finding."

### Does the patch directly modify my PR branch?

No. The workflow generates a diff for inspection before application.

## Validation

### What is auto-validation?

"Auto-validation is the phase that tries to reproduce a suspected issue in an isolated container. It records whether reproduction succeeded or failed and captures logs, commands, and related artifacts as evidence."

### What happens if validation fails?

The finding remains unvalidated, but captured logs and reports document the attempted steps for further investigation or retry.
