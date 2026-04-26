# Obsidian Project Logger — Note Templates

Copy-paste-ready content for every file this skill creates. All `{{ }}` values are replaced at runtime.

---

## Project Log File

**Path:** `Personal/{{ Project Name }}/{{ Project Name }} Log.md`

```markdown
---
title: "{{ Project Name }} Log"
tags:
  - project
  - project/{{ project-slug }}
type: project-log
project: "{{ Project Name }}"
created: {{ YYYY-MM-DD }}
updated: {{ YYYY-MM-DD }}
status: active
---

# {{ Project Name }} Log

> [!info] About This Log
> Tracks every development session for **{{ Project Name }}**.
> One dated block per session. Append — never overwrite.

## Changelog

| Date | Summary |
|---|---|
| {{ YYYY-MM-DD }} | Log created |

---

## {{ YYYY-MM-DD }}

### ✅ What We Did

- 

### 🐛 Errors & Fixes

| Error | Fix |
|---|---|
|  |  |

### 📋 TODO

- [ ] 
```

---

## Session Block (appended on each update)

This block is appended to the bottom of the project log on every new session:

```markdown

---

## {{ YYYY-MM-DD }}

### ✅ What We Did

- {{ bullet 1 }}
- {{ bullet 2 }}

### 🐛 Errors & Fixes

| Error | Fix |
|---|---|
| {{ what broke }} | {{ how it was fixed }} |

### 📋 TODO

- [ ] {{ task 1 }}
- [ ] {{ task 2 }}
```

---

## Project Index File

**Path:** `Personal/Project Index.md`

```markdown
---
title: "Project Index"
tags:
  - index
  - projects
type: index
created: {{ YYYY-MM-DD }}
updated: {{ YYYY-MM-DD }}
---

# Project Index

Central index of all projects logged in this vault. Each entry links to the project's session log.

## Active Projects

- [[Personal/{{ Project Name }}/{{ Project Name }} Log|{{ Project Name }}]] — created {{ YYYY-MM-DD }}

## Paused

<!-- Projects with status: paused -->

## Complete

<!-- Projects with status: complete -->

---

## Dataview Query (requires Dataview plugin)

If you have the Dataview plugin installed, replace the manual list above with this query for an auto-updating index:

```dataview
TABLE project, created, updated, status
FROM "Personal"
WHERE type = "project-log"
SORT updated DESC
```

*If not using Dataview, maintain the manual list above.*
```

---

## Status Lifecycle

Each project log uses a `status` property in frontmatter. Update it with:

```bash
obsidian property:set name="status" value="complete" \
  path="Personal/{{ Project Name }}/{{ Project Name }} Log.md"
```

| Status | Meaning |
|---|---|
| `active` | Currently being worked on |
| `paused` | On hold, may resume |
| `complete` | Done, no more sessions expected |
| `archived` | Old, closed out |

---

## Project Slug Convention

The `project-slug` used in tags is the project name lowercased with spaces replaced by hyphens:

| Project Name | Slug | Tag |
|---|---|---|
| Space Portfolio | `space-portfolio` | `#project/space-portfolio` |
| My CLI Tool | `my-cli-tool` | `#project/my-cli-tool` |
| PDR v1 | `pdr-v1` | `#project/pdr-v1` |

Generate in shell:
```bash
PROJECT_SLUG=$(echo "$PROJECT" | tr '[:upper:]' '[:lower:]' | sed 's/ /-/g')
```
