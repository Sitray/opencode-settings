# Obsidian Project Logger

A skill for automating project documentation directly from your terminal using Obsidian's CLI.

## What Is This?

This skill lets AI agents automatically create, update, and manage project logs in Obsidian — without needing to open the Obsidian GUI.

Instead of manually:
1. Opening Obsidian
2. Navigating to your vault
3. Finding the right file
4. Writing session notes

You just say:
- "log this to Obsidian"
- "update project notes"
- "create a project file"

And the agent does it for you.

## Why It Exists

Keeping documentation in sync is hard when you're in the flow. This skill solves that by:

- **CLI-first** — The agent runs commands, you stay in context
- **Templates** — Consistent project log structure
- **Idempotent** — Safe to re-run (won't duplicate)
- **Automatic properties** — Dates, tags, status updates

Your project decisions, bug fixes, and discoveries are automatically documented.

## Quick Start

### 1. Set Your Root Folder

```bash
echo "Personal" > .obsidian-project-root
```

### 2. Verify CLI

```bash
obsidian help
obsidian vault  # get vault name
```

### 3. Create a Project Log

```bash
obsidian create path="Personal/MyProject/MyProject Log.md" content="$(cat <<'EOF'
---
title: "MyProject Log"
tags: [project, myproject]
type: project-log
created: 2026-04-26
updated: 2026-04-26
status: active
---

# MyProject Log

## Changelog

| Date | Change |
|---|---|
| 2026-04-26 | Created |

---
EOF
)" silent
```

## What It Can Do

| Task | Example |
|---|---|
| Create file | `obsidian create path="..." content="..." silent` |
| Read file | `obsidian read path="..."` |
| Append to file | `obsidian append file="..." path="..." content="..."` |
| Set property | `obsidian property:set name="updated" value="2026-04-26" type=date path="..."` |
| Delete file | `obsidian delete path="..."` |
| Search vault | `obsidian search query="text" limit=5` |

## What Goes Where

```
Personal/
├── Project Index.md         # Master list of all projects
└── {Project}/
    └── {Project} Log.md    # Per-project changelog
```

Each project log contains:
- Changelog table
- Daily entries (what did, errors & fixes, TODO)
- Tags for organization
- Status tracking

## Key Patterns

### Daily Session Update

```bash
TODAY=$(date +%Y-%m-%d)

BLOCK="
---

## ${TODAY}

### What We Did

- Fixed N+1 query in UserList
- Added tests for auth middleware

### Errors & Fixes

| Error | Fix |
|---|---|
| Test timeout | Increased timeout to 5s |

### TODO

- [ ] Add integration tests
"

obsidian append file="Project Log" path="Personal/MyProject" content="$BLOCK"
obsidian property:set name="updated" value="${TODAY}" type=date path="Personal/MyProject/MyProject Log.md"
```

### Check Before Creating (Idempotency)

```bash
obsidian read path="Personal/MyProject/MyProject Log.md" 2>/dev/null \
  && echo "EXISTS" \
  || obsidian create path="Personal/MyProject/MyProject Log.md" content="$CONTENT" silent
```

## Tips

- Use `\n` for newlines in content
- `silent` flag = don't open in Obsidian UI
- Sort pair keys for duplicate detection: `[a,b].sort().join("-")`
- After setting root, ask: "Should I save this to engram for cross-session memory?"

## Files in This Skill

- `SKILL.md` — Full technical reference
- `references/templates.md` — Copy-paste templates
- `references/cli-commands.md` — CLI command reference

---

For the full technical guide, see [SKILL.md](SKILL.md).