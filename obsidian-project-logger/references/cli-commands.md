# Obsidian CLI — Command Reference for Project Logger

All commands used by this skill. Requires Obsidian 1.12+ with CLI enabled in Settings → General.

> Run `obsidian help` at any time to see all available commands and current syntax.

---

## File Operations

```bash
# Create a new note (creates intermediate folders automatically)
obsidian create name="Note Name" content="# Title\n\nBody"
obsidian create path="Folder/Subfolder/Note Name" content="..." silent

# Read a note (outputs markdown content)
obsidian read file="Note Name"
obsidian read path="Personal/Project/Note Name.md"

# Append content to end of a note
obsidian append file="Note Name" content="New line here"
obsidian append path="Personal/Project/Note Name.md" content="- bullet"

# List files in a folder
obsidian files path="Personal"
obsidian files path="Personal" total     # count only

# Search vault
obsidian search query="search term" limit=10
obsidian search query="tag:#project/my-project" format=json
```

## Property (Frontmatter) Operations

```bash
# Read a property value
obsidian property:read name="status" file="Note Name"

# Set a property (creates it if missing)
obsidian property:set name="status" value="active" file="Note Name"
obsidian property:set name="updated" value="2026-04-25" type=date file="Note Name"
obsidian property:set name="tags" value='["project", "project/my-app"]' type=list file="Note Name"

# Remove a property
obsidian property:remove name="status" file="Note Name"

# List all properties on a file
obsidian properties file="Note Name"
```

## Tag Operations

```bash
# List all tags in vault
obsidian tags

# List tags with file counts
obsidian tags counts sort=count

# List all files with a specific tag
obsidian tag name="project/space-portfolio" verbose

# Count files with a tag
obsidian tag name="project/space-portfolio" total
```

## Link Operations

```bash
# List all backlinks to a file
obsidian backlinks file="Project Index"

# Find orphan notes (no links)
obsidian orphans

# Find unresolved links (broken wikilinks)
obsidian unresolved
```

## Vault Info

```bash
# Show vault name, path, file count
obsidian vault

# Show specific vault info
obsidian vault info=name
obsidian vault info=path
```

## Multi-Vault Targeting

```bash
# Target a specific vault by name (must be first parameter)
obsidian "My Vault" read file="Note Name"
obsidian "My Vault" create path="Personal/Project/Note.md" content="..."
obsidian "My Vault" append file="Note" content="new content"
```

---

## Key Flags & Modifiers

| Flag | Effect |
|---|---|
| `silent` | Create/open without switching Obsidian focus to the new file |
| `overwrite` | Replace existing file content entirely (use with caution) |
| `total` | Return count instead of list for list commands |
| `--copy` | Copy command output to clipboard |
| `format=json` | Output as JSON (useful for scripting) |
| `format=text` | Plain text output (default) |
| `verbose` | Extended output with more details |

---

## Content Encoding Rules

| Character | How to write it |
|---|---|
| Newline | `\n` |
| Tab | `\t` |
| Values with spaces | Wrap in `"double quotes"` |
| Pipe character in tables | Escape as needed in shell context |

**Example — multiline content:**
```bash
obsidian create path="Personal/Project/Note" content="---\ntitle: \"My Note\"\ntags: [project]\n---\n\n# My Note\n\n## What We Did\n\n- First thing\n"
```

---

## Error Handling Patterns

```bash
# Check if file exists before creating
obsidian read file="Note" path="Personal/Project" 2>/dev/null
if [ $? -eq 0 ]; then
  echo "File exists — will append"
else
  echo "File missing — will create"
fi

# Check if a folder has any files
obsidian files path="Personal/ProjectName" total 2>/dev/null
```

---

## Platform Notes

| OS | Binary | Notes |
|---|---|---|
| macOS | `obsidian` (in PATH after registration) | Register via Settings → General → CLI |
| Linux | `obsidian` symlink at `/usr/local/bin/` | AppImage/Snap/Flatpak each have different symlink paths |
| Windows | Requires `Obsidian.com` redirector file | Place alongside `Obsidian.exe`, add to PATH |

**Linux headless note:** If running in a context without a display (CI, server), the official CLI requires a running Obsidian GUI instance. For headless use, consider `notesmd-cli` (community alternative that operates directly on vault files without a running Obsidian app).
