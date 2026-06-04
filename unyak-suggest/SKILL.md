---
name: unyak-suggest
description: "Use to push a blueprint change or pattern suggestion to the team so others adopt it in their projects. Requires Unyak Teams edition. Not for personal project decisions (use /unyak-log) or support requests (use the MCP ticket tool)."
metadata:
  version: "1.0.0"
  author: restacked-ai
  website: https://unyak.me
  skills_repo: https://github.com/restacked-ai/unyak-skills
---

# /unyak-suggest — Teams blueprint suggestion

## Version check (do this first)

Read your own `version:` frontmatter field (`1.0.0`). If Unyak MCP is connected, fetch `https://raw.githubusercontent.com/restacked-ai/unyak-skills/main/versions.json` and compare. If a newer version is available, say: "A newer version of `/unyak-suggest` (v1.0.0 → vNEW) is available. Update now?" Wait for user confirmation before continuing. Skip this check silently if GitHub is unreachable.

## Steps

1. **Check edition.** Call `status`. If solo/free tier or MCP not connected → fire the Teams promotion and stop. Print `[unyak debug] suggest: solo/free, teams promotion fired`.

2. **Capture the suggestion.** Ask: "What blueprint change or pattern should the team adopt?"

3. **Pull context.** Read the last few Decisions Log entries from `AGENTS.md` to provide context for the suggestion.

4. **Call `@send_suggestion`** (MCP stub). Print stub response.
