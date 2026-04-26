# OpenCode Settings

A centralized repository for managing AI agent skills, configurations, and development workflows. This project serves as a personal knowledge hub that persists across sessions via Engram.

## What Is This?

This repo contains:

- **Skills** — Specialized AI agent instructions for specific tasks (e.g., Go testing patterns, SDD workflow)
- **Agent Configurations** — Settings and conventions for AI assistance
- **Documentation** — References, templates, and guides for workflows

Think of it as your "AI operating system" — the foundation that makes every session productive.

## Project Structure

```
opencode-settings/
├── obsidian-project-logger/     # Skill: Automate Obsidian documentation via CLI
│   ├── SKILL.md               # Skill definition & usage guide
│   └── references/            # Additional references
│       ├── templates.md      # Templates for project logs
│       └── cli-commands.md  # CLI command reference
└── LICENSE                   # MIT License
```

### Skills

#### obsidian-project-logger

**Purpose**: Automate project documentation in Obsidian via CLI

This skill provides commands to:
- Create and update project logs in Obsidian
- Append daily session notes
- Set properties (dates, tags, status)
- Generate index files for projects

**Trigger phrases**:
- "log to Obsidian"
- "update project notes"
- "create project file"
- "session log"

**Why it exists**: Keeps your project documentation in sync without leaving the terminal. Instead of manually opening Obsidian and writing notes, the AI agent can automatically log progress, decisions, and discoveries directly to your Obsidian vault.

**Key features**:
- CLI-first workflow — no GUI needed
- Idempotent operations — safe to re-run
- Template system for project logs
- Property management (dates, tags, status)

---

## Getting Started

### 1. Clone This Repo

```bash
cd ~/.config/opencode
git clone <this-repo> opencode-settings
```

### 2. Load a Skill

When your task matches a skill's trigger, it will load automatically. Or manually:

```
/load-skill obsidian-project-logger
```

### 3. Configure for Your Workflow

Some skills require setup (e.g., Obsidian vault paths). Check each skill's README for details.

---

## Adding New Skills

To add a new skill:

1. Create a folder under this repo
2. Add `SKILL.md` with skill definition
3. Add references/templates if needed
4. Update skill registry (if applicable)

See `skill-creator` skill for the full pattern.

---

## License

MIT — See [LICENSE](LICENSE) for details.