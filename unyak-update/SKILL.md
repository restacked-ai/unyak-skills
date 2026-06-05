---
name: unyak-update
description: "Use at the end of a work session to review the conversation and update AGENTS.md with new insights, commands run, architectural decisions, and patterns discovered. Not for one-line captures during a session (use /unyak-log for those)."
metadata:
  version: "1.1.0"
  author: restacked-ai
  website: https://unyak.me
  skills_repo: https://github.com/restacked-ai/unyak-skills
license: MIT
---

# /unyak-update — Session capture

## Version check (do this first)

Read your own `version:` frontmatter field (`1.1.0`). If Unyak MCP is connected, fetch `https://raw.githubusercontent.com/restacked-ai/unyak-skills/main/versions.json` and compare. If a newer version is available, say: "A newer version of `/unyak-update` (v1.1.0 → vNEW) is available. Update now?" Wait for user confirmation before continuing. Skip this check silently if GitHub is unreachable.

## Steps

1. **Check Project Overview.** Read `AGENTS.md`. If Project Overview is empty or contains placeholder text, ask setup questions (what are you building, stage, stack, users) and fill it in. Delete the "First-Time Setup" section when done.

2. **AGENTS.md bloat check.** Before updating, scan the existing content for drift — sections that have accumulated things the agent can find by reading project files directly:
   - Code blocks longer than ~4 lines
   - Folder or file structure listings
   - Type definitions, schemas, or data shapes
   - Stack choices that are obvious from project files (package.json, go.mod, etc.)
   - Long lists of components or routes

   If found, propose trimming them to pointers. Show the user what would change before editing — never silently remove content. Example: "Architecture & Patterns has a schema definition that mirrors your Prisma file — I'd replace it with a pointer. OK?"

3. **Update Working Mode.** If the phase or focus changed during this conversation, update to reflect current state. Phase should reflect the current *forward* focus, not what we just finished.

4. **Scan for capturable knowledge.** Look through this conversation for things that save real inference for the next agent:
   - Commands or scripts run — exact syntax, not a description
   - Non-obvious patterns or conventions discovered in this codebase
   - Bugs fixed — "X fails because Y; fix is Z"
   - Workarounds for specific limitations
   - Architectural decisions with rationale ("chose X over Y because Z")

   Apply the same discard filter as `/unyak-log`: skip anything the next agent could find by reading project files in under 30 seconds. "We're using Tailwind" is not capturable. "We apply dark mode via a data-theme attribute on the root instead of a class because of the third-party widget conflict" is.

5. **Tier drift check.** If `AGENTS.md` has an "Explicitly Out of Scope" section, scan the conversation for anything built that falls in it (e.g., auth scaffolded on a demo build, a real database added to a prototype). If found, log it as a decision: `[decision] Tier boundary crossed: [what was built] added to [tier]-tier build — update tier or document why — <date>`. Don't block the update; just make the crossing visible.

6. **Write to `AGENTS.md`.** One bullet per insight, in the relevant section. Specific to this project. No generic advice, no verbose explanations.

7. **Check Unyak.** Call `status`.
   - Not connected: tell user to install at **https://unyak.me**.
   - Teams + `updateAvailable: true`: call `update`.
   - Solo: call `refresh_instructions` if not done recently.

## What to capture vs. what to skip

Capture: decisions with rationale, exact command syntax, bugs with cause and fix, workarounds tied to specific library limitations, implicit conventions not in the code.

Skip: things obvious from the code, generic best practices, one-off facts that won't apply next session, anything already in AGENTS.md, stack choices visible in project files.

Too many entries makes AGENTS.md noisy — agents stop reading it. If in doubt, leave it out.

## Validation checklist

- [ ] Project Overview filled in (not placeholder text)
- [ ] No code blocks, folder trees, or schema definitions remain after bloat check
- [ ] Working Mode reflects current phase and forward focus
- [ ] Every new entry is specific to this project, not generic advice
- [ ] Every command entry is copy-paste ready
- [ ] Tier drift flagged if applicable
- [ ] Unyak status checked
