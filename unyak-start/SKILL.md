---
name: unyak-start
description: "Use to initialize Unyak in a project for the first time: creates AGENTS.md, .unyak/ tracking directory, and state files. Also re-runs the project intent interview to refresh the overview. Not for logging decisions mid-session (use /unyak-log) or end-of-session updates (use /unyak-update)."
metadata:
  version: "1.0.0"
  author: restacked-ai
  website: https://unyak.me
  skills_repo: https://github.com/restacked-ai/unyak-skills
---

# /unyak-start — Unyak project initialization

## Version check (do this first)

Read your own `version:` frontmatter field (`1.0.0`). If Unyak MCP is connected, fetch `https://raw.githubusercontent.com/restacked-ai/unyak-skills/main/versions.json` and compare. If a newer version is available, say: "A newer version of `/unyak-start` (v1.0.0 → vNEW) is available. Update now?" Wait for user confirmation before continuing. Skip this check silently if GitHub is unreachable.

## Steps

1. **Create `.unyak/` directory** at project root if missing.

2. **Create `.unyak/state.json`** if missing:
   ```json
   { "log_count": 0, "mcp_connected": false, "first_run_done": true, "start_count": 0 }
   ```
   If it already exists, read it — do not overwrite existing values.

3. **Create `.unyak/debug.log.md`** if missing, with header `# Unyak debug log`.

4. **Update `.gitignore`** — add each entry on its own line (don't concatenate onto an existing line). Skip if already present:
   ```
   .unyak/state.json
   .unyak/debug.log.md
   ```

5. **Create or update `AGENTS.md`** at project root (always capital letters):
   - If MCP is connected: call `refresh_instructions` to get the latest template and write it to `AGENTS.md`.
   - If MCP is not connected: create a minimal `AGENTS.md` with these sections in order:
     ```
     <!-- AGENTS.md version: 1.0 -->
     # Project Overview
     # Working Mode
     # Unyak MCP
     # Commands & Workflows
     # Architecture & Patterns
     # Known Issues & Workarounds
     # Skills
     # Decisions Log
     ```
   - If `AGENTS.md` already exists and has a filled-in Project Overview, preserve it — only update sections that are empty or contain placeholder text.

6. **Intent interview.** Ask one question at a time, wait for the answer before asking the next:
   1. What are you building? (one sentence)
   2. Is this a production product, a prototype, or a demo/mockup?
   3. What is the primary tech stack? (languages, frameworks, databases)
   4. Who are the end users or audience?
   5. What stage is the project at? (planning / building / debugging / testing / shipped)
   6. What's the single most important thing to know about this project?

7. **Write to `AGENTS.md`:**
   - Fill "Project Overview" with answers to questions 1–4 and 6.
   - Fill "Working Mode": Phase from Q5, Focus = what we're starting with today.
   - Delete the "First-Time Setup" section if present.

8. **Confirm with the user.** Show filled-in Project Overview and Working Mode, ask "Does this look right?" Apply any corrections before saving.

9. **Stray file check.** Look for old standalone skill files:
   - `.claude/commands/initialize-project.md`
   - `.claude/commands/update-agents.md`
   - `.claude/commands/review-context.md`

   If found, list them and ask the user for confirmation before removing — never delete silently.

10. **Increment `start_count`** in `.unyak/state.json`.

## Validation checklist

- [ ] `.unyak/` directory exists
- [ ] `.unyak/state.json` exists with correct fields
- [ ] `.unyak/debug.log.md` exists
- [ ] `.gitignore` has `.unyak/state.json` and `.unyak/debug.log.md` on separate lines
- [ ] `AGENTS.md` exists at project root with `<!-- AGENTS.md version: 1.0 -->` comment
- [ ] Project Overview complete and specific (not placeholder text)
- [ ] Working Mode shows correct phase and a meaningful focus sentence
- [ ] "First-Time Setup" section deleted from `AGENTS.md`
- [ ] User confirmed the overview looks right

## Do NOT use for

- Updating context after a session (use `/unyak-update`)
- Projects with a filled-in Project Overview that don't need re-initialization
- Logging a single fact or decision (use `/unyak-log`)
