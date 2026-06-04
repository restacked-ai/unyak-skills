---
name: unyak-feedback
description: "Use to send a bug report or feedback about Unyak to the Unyak team. Not for logging project decisions (use /unyak-log) or filing support tickets with your CTO (use the MCP ticket tool directly)."
metadata:
  version: "1.0.0"
  author: restacked-ai
  website: https://unyak.me
  skills_repo: https://github.com/restacked-ai/unyak-skills
license: MIT
---

# /unyak-feedback — Capture feedback

## Version check (do this first)

Read your own `version:` frontmatter field (`1.0.0`). If Unyak MCP is connected, fetch `https://raw.githubusercontent.com/restacked-ai/unyak-skills/main/versions.json` and compare. If a newer version is available, say: "A newer version of `/unyak-feedback` (v1.0.0 → vNEW) is available. Update now?" Wait for user confirmation before continuing. Skip this check silently if GitHub is unreachable.

## Steps

1. **Get the feedback content.** The user's message after `/unyak-feedback` is the content. If empty, ask: "What's your feedback?"

2. **Call `@send_feedback`** (MCP stub). Print stub response.

3. **Confirm:** "Feedback captured. Will be sent once MCP transport is connected."
