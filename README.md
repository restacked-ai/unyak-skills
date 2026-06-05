# Unyak Skills

> **These skills are under active development.** Behavior may change between versions without notice. Use at your own risk.

Agent Skills for [Unyak](https://unyak.me) — a tool that assembles and maintains a live `AGENTS.md` context file so your coding agent always starts with the right project context.

These skills follow the open [Agent Skills](https://agentskills.io) standard and work with any compatible agent: Claude Code, GitHub Copilot, Cursor, OpenAI Codex, and [many others](https://agentskills.io/clients).

## What is Unyak?

Unyak keeps a project-scoped `AGENTS.md` file up to date with your architecture, decisions, known issues, and working conventions. Instead of re-explaining your project to a new agent session, you run one command and the agent picks up where you left off.

The skills in this repo teach your coding agent the Unyak workflow — how to initialize a project, log decisions mid-session, sync context at the end, diagnose blockers, and keep the skills themselves up to date.

## Skills

| Skill | Command | Purpose |
|---|---|---|
| [unyak](unyak/SKILL.md) | `/unyak` | Entry point and router. Verifies setup, checks versions, routes to the right command skill. |
| [unyak-start](unyak-start/SKILL.md) | `/unyak-start` | Initialize Unyak in a project. Creates `AGENTS.md`, `.unyak/` tracking dir, and runs an adaptive project intent interview. |
| [unyak-plan](unyak-plan/SKILL.md) | `/unyak-plan` | Deep project planning. Runs a structured interview (goals, users, pain, journey, features, scope) and writes a well-specified `AGENTS.md`. Use before building starts when the idea needs more than a quick interview. |
| [unyak-log](unyak-log/SKILL.md) | `/unyak-log` | Quick mid-session capture. Appends one decision, error, workaround, or command to `AGENTS.md`. Only logs what can't be discovered from the code. |
| [unyak-update](unyak-update/SKILL.md) | `/unyak-update` | End-of-session review. Scans the conversation, updates `AGENTS.md` with new insights, and flags stale or drifted content. |
| [unyak-help](unyak-help/SKILL.md) | `/unyak-help` | Diagnose a blocker. Reads `AGENTS.md` history and analyses the codebase to propose specific next steps. |
| [unyak-suggest](unyak-suggest/SKILL.md) | `/unyak-suggest` | Push a pattern or blueprint change to the team (requires Unyak Teams). |
| [unyak-feedback](unyak-feedback/SKILL.md) | `/unyak-feedback` | Send a bug report or feedback to the Unyak team. |
| [unyak-check-update](unyak-check-update/SKILL.md) | `/unyak-check-update` | Check whether newer versions of any installed Unyak skills are available and optionally apply them. |

## Installation

Skills live in `.agents/skills/` at your project root. Claude Code (and most other agents) also pick them up via `.claude/skills/`, which is a symlink that Unyak manages automatically.

### Option 1 — One command (recommended)

```bash
npx skills add restacked-ai/unyak-skills
```

Then open your project in your agent and type `/unyak-start`.

### Option 2 — Let the agent auto-install

Copy only the router skill into your project, then type `/unyak` — it will fetch and install the remaining skills from GitHub automatically.

```bash
mkdir -p .agents/skills/unyak
curl -fsSL https://raw.githubusercontent.com/restacked-ai/unyak-skills/main/unyak/SKILL.md \
  -o .agents/skills/unyak/SKILL.md
```

Open your agent (e.g. Claude Code) in the project directory, then type:

```
/unyak
```

The router will fetch all remaining skills, create the `.claude/skills` symlink, and walk you through first-time setup.

### Option 3 — Clone the full repo

```bash
# From your project root
git clone https://github.com/restacked-ai/unyak-skills .agents/skills

# Create the Claude Code symlink
ln -s ../.agents/skills .claude/skills
```

### Option 4 — Copy individual skill folders

Copy only the skills you want into `.agents/skills/` — each folder must contain a `SKILL.md` file.

```
your-project/
└── .agents/
    └── skills/
        ├── unyak/
        │   └── SKILL.md
        ├── unyak-start/
        │   └── SKILL.md
        └── unyak-log/
            └── SKILL.md
```

### Agent-specific setup

| Agent | Skills directory | Notes |
|---|---|---|
| Claude Code | `.claude/skills/` or `.agents/skills/` | Unyak creates `.claude/skills → .agents/skills` symlink automatically |
| GitHub Copilot / VS Code | `.agents/skills/` | Default discovery path |
| Cursor | `.agents/skills/` | See [Cursor docs](https://cursor.com/docs/context/skills) |
| OpenAI Codex | `.agents/skills/` | See [Codex docs](https://developers.openai.com/codex/skills/) |
| Other agents | varies | Check [agentskills.io/clients](https://agentskills.io/clients) |

## Getting started

Once installed, open the project in your agent and type:

```
/unyak-start
```

The agent reads the project first (package.json, README, git log), then asks only what it can't already see. For a more thorough setup — when you want to map user goals, pain points, and scope before building — run `/unyak-plan` instead. unyak-start will offer this automatically if your project doesn't have an AGENTS.md yet.

From there:

- Type `/unyak-log <something worth remembering>` anytime during a session to capture a decision or command. The skill checks whether it's actually worth logging before writing anything.
- Type `/unyak-update` at the end of a session to sync everything back to `AGENTS.md`. It also scans for stale content and flags scope drift.
- Type `/unyak-help` when you're stuck — it reads your history and proposes specific next steps.
- Type `/unyak-check-update` to pull the latest skill versions from GitHub.

## Updating skills

Run `/unyak-check-update` inside your agent to compare installed versions against the [versions manifest](versions.json) and apply any updates.

To update manually:

```bash
# From your project root, if you cloned into .agents/skills
cd .agents/skills && git pull
```

## Optional: Unyak MCP

The skills work standalone, but connecting [Unyak MCP](https://unyak.me) unlocks session sync across machines, smarter `/unyak-update` analysis, and team blueprints via `/unyak-suggest`. Install at **https://unyak.me**.

## Project structure

```
unyak-skills/
├── unyak/                  # Router and entry point
│   └── SKILL.md
├── unyak-start/            # Project initialization
│   └── SKILL.md
├── unyak-plan/             # Deep project planning
│   └── SKILL.md
├── unyak-log/              # Quick mid-session capture
│   └── SKILL.md
├── unyak-update/           # End-of-session context sync
│   └── SKILL.md
├── unyak-help/             # Blocker diagnosis
│   └── SKILL.md
├── unyak-suggest/          # Team blueprint suggestions
│   └── SKILL.md
├── unyak-feedback/         # Feedback submission
│   └── SKILL.md
├── unyak-check-update/     # Skill version management
│   └── SKILL.md
└── versions.json           # Version manifest (used by check-update)
```

## About Agent Skills

Agent Skills are an open, lightweight format for giving AI agents specialized knowledge and workflows. A skill is a folder with a `SKILL.md` file containing YAML frontmatter (`name`, `description`) and Markdown instructions. Agents discover skills at startup by reading only the name and description, then load full instructions only when a task matches — keeping context usage low even with many skills installed.

Learn more at [agentskills.io](https://agentskills.io) or browse the [specification](https://agentskills.io/specification).

## License

MIT — see [LICENSE](LICENSE)

## Links

- Website: [unyak.me](https://unyak.me)
- Skills repo: [github.com/restacked-ai/unyak-skills](https://github.com/restacked-ai/unyak-skills)
- Agent Skills standard: [agentskills.io](https://agentskills.io)
