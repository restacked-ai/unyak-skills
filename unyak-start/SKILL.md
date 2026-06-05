---
name: unyak-start
description: "Use to initialize Unyak in a project for the first time: creates AGENTS.md, .unyak/ tracking directory, and state files. Also re-runs the project intent interview to refresh the overview. Not for logging decisions mid-session (use /unyak-log) or end-of-session updates (use /unyak-update)."
metadata:
  version: "1.1.0"
  author: restacked-ai
  website: https://unyak.me
  skills_repo: https://github.com/restacked-ai/unyak-skills
license: MIT
---

# /unyak-start — Unyak project initialization

## Version check (do this first)

Read your own `version:` frontmatter field (`1.1.0`). If Unyak MCP is connected, fetch `https://raw.githubusercontent.com/restacked-ai/unyak-skills/main/versions.json` and compare. If a newer version is available, say: "A newer version of `/unyak-start` (v1.1.0 → vNEW) is available. Update now?" Wait for user confirmation before continuing. Skip this check silently if GitHub is unreachable.

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

5. **Read the project before asking anything.** Scan for context:
   - `package.json`, `go.mod`, `Pipfile`, `pyproject.toml`, `Cargo.toml` — detect stack and description
   - `README.md` — detect project purpose, target users, problem statement
   - Existing `AGENTS.md` — check if Project Overview is already filled in
   - `git log --oneline -5` — detect stage (recent commits suggest "building")

6. **Check for existing context.** Three cases:

   - **AGENTS.md has real content** (Project Overview is filled in with project-specific text, not placeholder text or template headers): this project is already unyak-managed. Skip the unyak-plan offer and skip the interview. Just verify all required sections exist, add any that are missing as empty headers, and proceed to step 9.

   - **AGENTS.md is missing or scaffolded** (file doesn't exist, or exists but contains only template headers and placeholder text): offer the planning skill before proceeding:
     > "Want a more thorough setup? `/unyak-plan` walks through goals, users, pain points, and scope — and produces a richer AGENTS.md. Or skip it and I'll ask a few quick questions now."

     If the user chooses unyak-plan: read `.agents/skills/unyak-plan/SKILL.md` and follow its instructions. When it completes, finish the remaining unyak-start steps (stray file check, increment `start_count`) and stop — do not run the interview below.

     If the user skips: continue to the adaptive interview.

7. **Adaptive intent interview.** Ask one question at a time. Use what you found in step 5 to skip or pre-fill obvious answers — never ask for something you can already see. Rephrase each question to fit the specific project; do not use these as literal scripts.

   **a. Tier** — Ask this first. Always. It gates everything that follows.
   "Is this a production product, a prototype, or a demo/mockup?"

   **b. What you're building** — If README or package.json has a clear description, quote it: "I see this is [X] — is that right, or do you want to describe it differently?" Only ask from scratch if nothing is detectable.

   **c. Pain and solution** — Skip if README already names the problem and solution clearly. Otherwise ask: "What does the user find broken or slow today?" Push for a specific process or gap, not a vague feeling ("it's annoying" is not pain). Then ask "How does this fix it in one sentence?" only if the solution isn't already obvious from the pain answer.

   **d. End users** — Skip if clearly named in the README, or if it's an internal tool / solo project (often obvious from context). Otherwise ask: "Who are the end users?"

   **e. Primary tech stack** — If detected from project files, confirm it: "I can see you're using [X] — is that the full stack?" Only ask from scratch if nothing is detectable.

   **f. Project stage** — If git log shows recent commits, lead with that: "Looks like active development — are you in the building phase, or something else?" Options: planning / building / debugging / testing / shipped.

   **g. Most important thing to know** — Ask this last. Never skip. Open-ended: "What's the one thing about this project that isn't obvious from reading the code?"

   **Additional questions based on tier and stack:**

   If tier is Demo or Prototype AND stack includes a frontend technology (React, Next.js, Vue, Svelte, SvelteKit, Remix, Astro, Lovable, etc.):

   - Ask: "Mobile-first, desktop-first, or responsive?"

   - Then offer the visual style menu. Explain once: "Picking a style now prevents the agent from defaulting to 'modern and clean' — which is what every AI-built app looks like. Pick one or describe your own with a reference site."

     Options (present as a list):
     - **Brutalist / raw** — Stark oversized text, harsh contrast, no decoration. Both platforms.
     - **Editorial / magazine** — Big typography, generous whitespace. Best on desktop.
     - **Glassmorphism / Apple-modern** — Frosted glass, soft shadows, rounded corners. Native to mobile/iOS.
     - **Soft pastel / playful** — Rounded shapes, soft colors, friendly type. Both platforms.
     - **Terminal / monochrome** — Monospace font, dark background, single accent color. Best on desktop.
     - **Luxury / refined** — Minimal, serif fonts, muted tones. Both platforms.
     - **Custom** — Ask for one reference site.

     If the user picked Mobile-first, note that Terminal, Editorial, and Brutalist styles translate poorly and suggest alternatives.

8. **Create or update `AGENTS.md`** at project root:
   - If MCP is connected: call `refresh_instructions` to get the latest template and use it.
   - If not connected: create with this section order:
     ```
     <!-- AGENTS.md version: 1.1 -->
     # Project Overview
     # Working Mode
     # Unyak MCP
     # Commands & Workflows
     # Architecture & Patterns
     # Explicitly Out of Scope
     # Known Issues & Workarounds
     # Skills
     # Decisions Log
     ```
   - If `AGENTS.md` already exists with filled content, preserve it. Only update empty or placeholder sections.

9. **Write interview answers into `AGENTS.md`:**

   **Project Overview** — Cover: what the project is, who it's for, what problem it solves, and the tier. Write for an agent reading this cold — give the intent and constraints, not a summary of the code. Do not list the tech stack unless there's a constraint the agent can't infer from the project files. Do not describe folder structure, schemas, or components.

   **Working Mode** — Phase from stage answer. Focus = what we're starting on today.

   If platform and visual style were answered, add under Project Overview:
   ```
   ### Technical Context
   - Tier: [demo / prototype / production]
   - Platform: [mobile-first / desktop-first / responsive]
   - Visual style: [chosen option — 1 sentence on feel, reference site if provided]
   ```
   For mobile-first, add platform rules: touch targets ≥44px, bottom navigation, one column, no hover-only interactions, viewport meta tag, test at 375px.
   For desktop-first, add: hover states allowed, sidebar navigation, test at 1440px.

   **Explicitly Out of Scope** (tier-gated):
   - Demo tier:
     ```
     - No auth — use a mock role picker or skip login entirely
     - No real database — use in-memory data or hardcoded JSON
     - No dynamic AI responses — all suggestions are hardcoded
     - No secrets management
     - No analytics
     ```
   - Prototype tier:
     ```
     - No SSO or enterprise auth
     - No compliance review
     - No infrastructure automation
     ```
   - Production tier: omit this section entirely.

10. **Content discipline check.** Before saving, review what you've written. Remove anything that:
    - Is a code block longer than ~4 lines
    - Lists files, directories, or folder structure
    - Repeats type definitions, schemas, or data shapes from project files
    - Would be stale as soon as the code changes

    Replace with a pointer if needed ("see `src/types/` for data shapes"). The principle: if the next agent could find it by reading a project file in under 30 seconds, don't put it in AGENTS.md. Drift is worse than incompleteness.

11. **Confirm with the user.** Show filled-in Project Overview and Working Mode, ask "Does this look right?" Apply any corrections before saving.

12. **Stray file check.** Look for old standalone skill files:
    - `.claude/commands/initialize-project.md`
    - `.claude/commands/update-agents.md`
    - `.claude/commands/review-context.md`

    If found, list them and ask for confirmation before removing — never delete silently.

13. **Increment `start_count`** in `.unyak/state.json`.

## Validation checklist

- [ ] `.unyak/` directory exists
- [ ] `.unyak/state.json` exists with correct fields
- [ ] `.unyak/debug.log.md` exists
- [ ] `.gitignore` has `.unyak/state.json` and `.unyak/debug.log.md` on separate lines
- [ ] `AGENTS.md` exists at project root with `<!-- AGENTS.md version: 1.1 -->` comment
- [ ] Project Overview covers intent, problem, and users — not a list of files or stack choices
- [ ] Working Mode shows correct phase and a meaningful focus sentence
- [ ] "Explicitly Out of Scope" section present and correct for Demo and Prototype tiers
- [ ] No code blocks, folder trees, or schema definitions in AGENTS.md
- [ ] User confirmed the overview looks right

## Do NOT use for

- Updating context after a session (use `/unyak-update`)
- Logging a single fact or decision (use `/unyak-log`)
- Full project planning before building starts (use `/unyak-plan`)
