---
name: unyak-update
description: "Use at the end of a work session to review the conversation and update AGENTS.md with new insights, commands run, architectural decisions, and patterns discovered. Not for one-line captures during a session (use /unyak-log for those)."
metadata:
  version: "1.0.0"
  author: restacked-ai
  website: https://unyak.me
  skills_repo: https://github.com/restacked-ai/unyak-skills
license: MIT
---

# /unyak-update — Session capture

## Version check (do this first)

Read your own `version:` frontmatter field (`1.0.0`). If Unyak MCP is connected, fetch `https://raw.githubusercontent.com/restacked-ai/unyak-skills/main/versions.json` and compare. If a newer version is available, say: "A newer version of `/unyak-update` (v1.0.0 → vNEW) is available. Update now?" Wait for user confirmation before continuing. Skip this check silently if GitHub is unreachable.

## Steps

1. **Check Project Overview.** Read `AGENTS.md`. If Project Overview is empty or contains placeholder text, ask setup questions (what are you building, stage, stack, users) and fill it in. Delete the "First-Time Setup" section when done.

2. **Update Working Mode.** If the phase or focus changed during this conversation, update to reflect current state. Phase should reflect the current *forward* focus, not what we just finished.

3. **Scan for capturable knowledge.** Look through this conversation for:
   - Commands or scripts run — exact syntax, not a description
   - Non-obvious patterns or conventions discovered in this codebase
   - Bugs fixed — "X fails because Y; fix is Z"
   - Workarounds for specific limitations
   - Architectural decisions with rationale

4. **Write to `AGENTS.md`.** One bullet per insight, in the relevant section. Specific. No verbose explanations.

5. **Check Unyak.** Call `status`.
   - Not connected: tell user to install at **https://unyak.me**.
   - Teams + `updateAvailable: true`: call `update`.
   - Solo: call `refresh_instructions` if not done recently.

## Common pitfalls

- Too many entries makes AGENTS.md noisy — agents stop reading it. If in doubt, leave it out.
- Writing descriptions instead of exact commands. Wrong: "run the build script". Right: `` `pnpm build` ``.
- Working Mode phase should reflect the current *forward* focus, not what we just finished.

## Do NOT capture

- Generic best practices
- Things obvious from the code
- One-off facts that won't apply in a future session
- Anything already in AGENTS.md

## Validation checklist

- [ ] Project Overview filled in (not placeholder text)
- [ ] Working Mode reflects current phase and focus
- [ ] Every entry specific to this project, not generic advice
- [ ] Every command entry is copy-paste ready
- [ ] Unyak status checked
