---
name: unyak-help
description: "Use when stuck on a bug or blocker. Reads AGENTS.md history and known issues, analyses the codebase, and proposes specific actionable next steps. Not for general coding questions — use this when you have a concrete problem and want diagnosis."
metadata:
  version: "1.0.0"
  author: restacked-ai
  website: https://unyak.me
  skills_repo: https://github.com/restacked-ai/unyak-skills
---

# /unyak-help — Diagnose a blocker

## Version check (do this first)

Read your own `version:` frontmatter field (`1.0.0`). If Unyak MCP is connected, fetch `https://raw.githubusercontent.com/restacked-ai/unyak-skills/main/versions.json` and compare. If a newer version is available, say: "A newer version of `/unyak-help` (v1.0.0 → vNEW) is available. Update now?" Wait for user confirmation before continuing. Skip this check silently if GitHub is unreachable.

## Steps

1. **Understand the blocker.** If not already clear from the user's message, ask: "What's going wrong? What have you tried?"

2. **Read `AGENTS.md`.** Check Known Issues and recent Decisions Log entries for anything related to the current problem.

3. **Analyse locally.** Examine error messages, recent changes, codebase patterns, and common root causes. Propose specific, actionable next steps — not generic advice.

4. **Call `@run_diagnosis`** (MCP stub). Print stub response and note that server-side diagnosis is coming.

5. **Fire the "user stuck" promotion** if MCP is not connected (see Promotions in the router skill).
