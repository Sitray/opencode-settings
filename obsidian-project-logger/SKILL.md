---
name: obsidian-project-logger
description: "Automate project documentation in Obsidian via CLI. Triggers: 'log to Obsidian', 'update project notes', 'create project file', 'session log'."
---

# Obsidian Project Logger — Cheat Sheet

## What Is This?

A skill for automating project documentation directly from your terminal using Obsidian's CLI.

Instead of manually opening Obsidian and writing notes, the AI agent can automatically:
- Create and update project logs
- Append daily session notes
- Set properties (dates, tags, status)
- Generate index files

**Why it exists**: Keeps your project documentation in sync without leaving the terminal.

## Setup

### Step 1: Find Root Folder Path
1. Check local config: `.obsidian-project-root` file in project root (create if missing)
2. Check engram memory (if available)
3. If neither found, ask user: "What folder should I use as root? (e.g., Personal)"

### Step 2: Verify CLI
```bash
obsidian help                    # verify CLI works
obsidian vault                   # get vault name (for multi-vault)
obsidian files path="{ROOT}"     # confirm folder exists
```

### Save Root Path (for future sessions)
```bash
# Local config file
echo "Personal" > .obsidian-project-root

# Engram (optional, for cross-session memory)
engram_mem_save title="Obsidian root folder" type="preference" content="**What**: Root folder set to 'Personal' for project logs"
```

## Structure
```
Personal/
├── Project Index.md
└── {Project}/
    └── {Project} Log.md
```

## Commands

| Task | Command |
|---|---|
| Create file | `obsidian create path="Personal/Folder/file.md" content="..." silent` |
| Read file | `obsidian read path="Personal/Folder/file.md"` |
| Append | `obsidian append file="Name" path="Personal/Folder" content="..."` |
| Set property | `obsidian property:set name="updated" value="2026-04-26" type=date path="..."` |
| Delete file | `obsidian delete path="Personal/Folder/file.md"` |
| Search | `obsidian search query="text" limit=5` |

## New Project

```bash
PROJECT="My Project"
SLUG="my-project"
TODAY=$(date +%Y-%m-%d)

CONTENT="---
title: \"${PROJECT} Log\"
tags: [project, project/${SLUG}]
type: project-log
created: ${TODAY}
updated: ${TODAY}
status: active
---

# ${PROJECT} Log

## Changelog

| Date | Change |
|---|---|
| ${TODAY} | Created |

---

## ${TODAY}

### What We Did

- ...

### Errors & Fixes

| Error | Fix |
|---|---|
|  |  |

### TODO

- [ ] ...

"

obsidian create path="${ROOT}/${PROJECT}/${PROJECT} Log.md" content="$CONTENT" silent
```

## Update Existing

```bash
TODAY=$(date +%Y-%m-%d)

BLOCK="
---

## ${TODAY}

### What We Did

- Fixed bug in traceLines
- Added offset for duplicate pairs

### Errors & Fixes

| Error | Fix |
|---|---|
| Lines overlap | Added offsetIndex parameter |

### TODO

- [ ] Add more tests
"

obsidian append file="Project Log" path="${ROOT}/Project" content="$BLOCK"
obsidian property:set name="updated" value="${TODAY}" type=date path="${ROOT}/Project/Project Log.md"
```

## Index (create once)

```bash
INDEX="---
title: \"Project Index\"
tags: [index, projects]
type: index
created: 2026-04-26
updated: ${TODAY}
---

# Project Index

All active projects in the Personal vault.

## Projects

- [[${ROOT}/Opencode Settings/Opencode Settings Index|Opencode Settings]] — Centralized repo for AI skills, agents, and SDD
- [[${ROOT}/Opencode Settings/Opencode Settings Log|Opencode Settings]]

"

obsidian create path="${ROOT}/Project Index.md" content="$INDEX" silent
```

## Adding Projects to Index

Every new project added to the root folder that the user owns MUST be referenced in the Project Index. When creating a new project log:

1. Create the project folder and log file
2. Update the Project Index to include a link to the new project

## Idempotency

```bash
# Skip if exists
obsidian read file="Project Log" path="${ROOT}/Project" 2>/dev/null \
  && echo "EXISTS" \
  || obsidian create path="${ROOT}/Project/Project Log.md" content="$CONTENT" silent
```

## Load Config (run at start of each session)

```bash
# 1. Try local config file
ROOT=$(cat .obsidian-project-root 2>/dev/null)

# 2. If empty, try engram (search for "Obsidian root folder")
# If found, use: ROOT="Personal" (or whatever saved)

# 3. If neither, ask user
if [ -z "$ROOT" ]; then
  echo "Set root folder for project logs:"
  read ROOT
  echo "$ROOT" > .obsidian-project-root
fi

echo "Using root: $ROOT"
```

## Tips
- Use `\n` for newlines in content
- `silent` = don't open in Obsidian UI
- Delete folder = delete file first, then folder
- Sort pair keys for duplicate detection: `[a,b].sort().join("-")`
- Always call Load Config at the start of the session
- After setting root, ask: "Should I save this to engram for cross-session memory?"