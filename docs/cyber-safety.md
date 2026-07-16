# Cyber Safety

Cybersecurity safeguards and trusted access for Codex users

## Overview

GPT-5.3-Codex represents OpenAI's first model classified as having high cybersecurity capability under their Preparedness Framework, which mandates enhanced protective measures. The model includes training to decline clearly harmful requests such as credential theft. Beyond safety training, automated classifier-based detection systems identify suspicious cyber activity and redirect high-risk traffic to the less cyber-capable GPT-5.2 model. The organization anticipates minimal traffic impact from these protections while continuing to refine associated policies and classifiers.

## Why we're doing this

Recent months have demonstrated substantial improvements in model performance on cybersecurity tasks, benefiting both developers and security professionals. As capabilities advance in cybersecurity-related functions like vulnerability discovery, the organization adopts a precautionary stance by strengthening protections to support legitimate research while constraining misuse.

Cybersecurity capabilities present inherent dual-use challenges. Techniques supporting defensive work, penetration testing, vulnerability research, malware analysis, threat intelligence, can equally enable harmful activities. These tools require controlled availability in legitimate security contexts. The organization offers "Trusted Access for Cyber," a pilot program enabling continued use of advanced models for potentially high-risk cybersecurity work without disruption.

## How it works

Developers and security professionals conducting cybersecurity work may experience request rerouting to GPT-5.2 as a fallback. The latest Codex CLI alpha version includes in-product messaging for rerouted requests, with broader client support forthcoming. Users affected by mitigations can regain GPT-5.3-Codex access through the Trusted Access program.

Request-level rather than account-level checks represent the organization's long-term direction as mitigations scale.

## Trusted Access for Cyber

The pilot program allows developers to maintain advanced capabilities during the calibration phase. Access paths include:

- Individual identity verification at chatgpt.com/cyber
- Enterprise team access through OpenAI representatives
- Invite-only program for security researchers requiring maximum cyber-capable models

All trusted access users must comply with Usage Policies and Terms of Use.

## False positives

Legitimate non-cybersecurity activity may occasionally trigger rerouting. Users can report incorrect routing via `/feedback` in the CLI, with visibility of the fallback model appearing in API logs and product notices.
