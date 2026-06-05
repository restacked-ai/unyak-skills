---
name: unyak-log
description: "Use to capture a single decision, error, workaround, or command from the current conversation. Appends one line to AGENTS.md Decisions Log. Use during a session whenever something worth remembering happens — not at the end of a session (use /unyak-update for that)."
metadata:
  version: "1.1.0"
  author: restacked-ai
  website: https://unyak.me
  skills_repo: https://github.com/restacked-ai/unyak-skills
license: MIT
---

# /unyak-log — Quick capture

## Version check (do this first)

Read your own `version:` frontmatter field (`1.1.0`). If Unyak MCP is connected, fetch `https://raw.githubusercontent.com/restacked-ai/unyak-skills/main/versions.json` and compare. If a newer version is available, say: "A newer version of `/unyak-log` (v1.1.0 → vNEW) is available. Update now?" Wait for user confirmation before continuing. Skip this check silently if GitHub is unreachable.

## Steps

1. **Get the thing to capture.** The user's message after `/unyak-log` is the content. If empty, ask: "What do you want to log? (one sentence)"

2. **Discard check.** Before logging, ask: could the next agent figure this out by reading a project file in under 30 seconds? If yes, don't log it. Tell the user: "That's findable from the code — logging it would just drift. Not worth capturing."

   Examples to decline:
   - "we're using React" — visible in package.json
   - "the Button component lives in /components" — discoverable by file search
   - "we use TypeScript" — obvious from file extensions
   - "we have a /api folder" — visible from the directory tree

   Log only what saves real inference for the next agent:
   - **Decisions:** "chose X over Y because Z constraint" — not just "using X"
   - **Errors:** "X fails because Y; fix is Z" — not just "there's a bug in X"
   - **Workarounds:** "Y behavior works around a limitation in Z library" — not just "we do Y"
   - **Commands:** exact run syntax that's non-obvious or not documented in a Makefile/script
   - **Patterns:** implicit conventions not expressed anywhere in the code (naming rules, ordering exceptions, unwritten team norms)

3. **Classify** as one of: `decision`, `error`, `workaround`, `command`, or `pattern`.

4. **Append to `AGENTS.md`** Decisions Log section:
   ```
   - [<type>] <one-sentence capture> — <date>
   ```
   - For `command`: exact copy-paste syntax, not a description. Right: `` `pnpm build` ``. Wrong: "run the build script".
   - For `error`: format as "X fails because Y; fix is Z".
   - For `decision`: include the rationale, not just the choice. "Using X" is not a decision. "Using X instead of Y because Z" is.

5. **Increment `log_count`** in `.unyak/state.json`. Print `[unyak debug] log_count now <n>`.

6. **Confirm.** Say "Logged." and show the line added. No lengthy explanation needed.

## Do NOT use for

- End-of-session review and bulk AGENTS.md update (use `/unyak-update`)
- Re-initializing the project (use `/unyak-start`)
- Things the next agent can read directly from the code
