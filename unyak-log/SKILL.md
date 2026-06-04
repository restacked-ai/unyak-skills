---
name: unyak-log
description: "Use to capture a single decision, error, workaround, or command from the current conversation. Appends one line to AGENTS.md Decisions Log. Use during a session whenever something worth remembering happens — not at the end of a session (use /unyak-update for that)."
metadata:
  version: "1.0.0"
  author: restacked-ai
  website: https://unyak.me
  skills_repo: https://github.com/restacked-ai/unyak-skills
---

# /unyak-log — Quick capture

## Version check (do this first)

Read your own `version:` frontmatter field (`1.0.0`). If Unyak MCP is connected, fetch `https://raw.githubusercontent.com/restacked-ai/unyak-skills/main/versions.json` and compare. If a newer version is available, say: "A newer version of `/unyak-log` (v1.0.0 → vNEW) is available. Update now?" Wait for user confirmation before continuing. Skip this check silently if GitHub is unreachable.

## Steps

1. **Get the thing to capture.** The user's message after `/unyak-log` is the content. If empty, ask: "What do you want to log? (one sentence)"

2. **Classify** as one of: `decision`, `error`, `workaround`, `command`, or `pattern`.

3. **Append to `AGENTS.md`** Decisions Log section:
   ```
   - [<type>] <one-sentence capture> — <date>
   ```
   - For `command`: exact copy-paste syntax, not a description. Right: `` `pnpm build` ``. Wrong: "run the build script".
   - For `error`: format as "X fails because Y; fix is Z".

4. **Increment `log_count`** in `.unyak/state.json`. Print `[unyak debug] log_count now <n>`.

5. **Confirm.** Say "Logged." and show the line added. No lengthy explanation needed.

## Do NOT use for

- End-of-session review and bulk AGENTS.md update (use `/unyak-update`)
- Re-initializing the project (use `/unyak-start`)
