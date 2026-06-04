---
name: unyak-check-update
description: "Use to check whether newer versions of any installed Unyak skills are available and optionally apply them. Not for updating AGENTS.md with session insights (use /unyak-update for that)."
metadata:
  version: "1.0.0"
  author: restacked-ai
  website: https://unyak.me
  skills_repo: https://github.com/restacked-ai/unyak-skills
---

# /unyak-check-update — Refresh skill versions

## Steps

1. **Fetch the version manifest.** Call `refresh_instructions` via Unyak MCP if connected. Also fetch `https://raw.githubusercontent.com/restacked-ai/unyak-skills/main/versions.json` directly via WebFetch.
   - If both fail: "MCP is not connected and GitHub is unreachable. Install MCP at **https://unyak.me** or check your connection."
   - If only MCP fails: proceed with GitHub manifest.

2. **Read installed skill versions.** For each skill in `.agents/skills/unyak*/SKILL.md`, read its `version:` frontmatter field.

3. **Compare.** List any skills where the installed version is behind the manifest.

4. **Report.**
   - All up to date: "You're on the latest version of all Unyak skills."
   - Outdated skills found: list each with current → latest version. Ask: "Apply updates now?"

5. **Apply updates (if confirmed).** For each outdated skill:
   - Fetch `https://raw.githubusercontent.com/restacked-ai/unyak-skills/main/skills/<cmd>/SKILL.md`.
   - Write to `.agents/skills/<cmd>/SKILL.md`.
   - Replace the skill file wholesale — no project-specific content lives in skill files.
   - Print `[unyak debug] updated: <cmd> v<old> → v<new>`.

6. **Merge `AGENTS.md`** (if `refresh_instructions` returned an updated template):
   - Preserve all project-specific sections: Project Overview, Working Mode, Commands & Workflows, Architecture & Patterns, Known Issues & Workarounds, Decisions Log.
   - Update only: the version comment at the top, the Unyak MCP section, and the Skills section.
   - **Never overwrite project-specific sections.**

## Do NOT use for

- Updating AGENTS.md with session insights (use `/unyak-update`)
- Initial project setup (use `/unyak-start`)
