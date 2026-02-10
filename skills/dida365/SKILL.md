---
name: dida365-tasks
description: Create, update, complete, and list Dida365/TickTick tasks. Use when user asks to add a todo, reminder, or manage tasks.
---

# Dida365 Tasks

## Requirements / Installation

This skill assumes the **`dida365`** CLI is available on the machine.

1) Install the CLI globally:

```bash
npm i -g dida365-cli
```

2) Verify:

```bash
dida365 --help
```

> If `dida365` is not found, ensure your global npm bin directory is on `PATH`.

## Quick Reference

```bash
# View all tasks
dida365 tasks get-all

# Create task
dida365 tasks create --title "Title" --project-id <projectId>

# Complete task
dida365 tasks complete --task-id <taskId>
```

## Commands Overview

| Action | Command |
|--------|---------|
| View all tasks | `dida365 tasks get-all` |
| View project tasks | `dida365 tasks get-all --project-id <projectId>` |
| List projects | `dida365 projects list` |
| Create task | `dida365 tasks create --title <title> --project-id <projectId>` |
| Update task | `dida365 tasks update --task-id <taskId>` |
| Complete task | `dida365 tasks complete --task-id <taskId>` |
| Delete task | `dida365 tasks delete --task-id <taskId>` |

## Creating Tasks

**Required:**
- `--title` — Task title
- `--project-id` — Project ID (run `dida365 projects list` first)

**Common options:**
- `--due "YYYY-MM-DD HH:mm"` — Due date
- `--reminder` — Reminder (can repeat)
- `--repeat-flag` — Repeat rule
- `--tag` — Add tag (repeatable)
- `--disable-required-tags` — Disable auto "cli" tag

**Examples:**
```bash
# Tomorrow 3pm, with reminders
dida365 tasks create --title "Meeting" --project-id WORK_ID --due "2026-02-08 15:00" --reminder "-PT30M"

# Weekly on Mon/Wed/Fri
dida365 tasks create --title "Exercise" --project-id PERSONAL_ID --repeat-flag "RRULE:FREQ=WEEKLY;BYDAY=MO,WE,FR"

# Task with custom tags
dida365 tasks create --title "Buy milk" --project-id PERSONAL_ID --tag shopping --tag urgent
```

## Viewing Tasks

```bash
# All tasks (all projects)
dida365 tasks get-all

# Specific project
dida365 tasks get-all --project-id <projectId>

# Get project list first
dida365 projects list
```

## Authentication

| Task | Command |
|------|---------|
| Check status | `dida365 status` |
| Login | `dida365 auth login` |
| Logout | `dida365 auth logout --force` |

## Configuration

**Config file locations (in priority order):**
1. `--config <path>` — CLI flag
2. `./dida365.config.json` — Current directory
3. `~/.config/dida365.json` — User config

**Default values:**
- API: `https://api.dida365.com/open/v1`
- Timezone: `Asia/Shanghai`
- Tags: `["cli"]`
- Cache: 10 min (tasks), 7 days (projects)

**Customize tags:**
```json
{
  "requiredTags": ["work", "important"],
  "enableRequiredTags": true
}
```

## Detailed Reference

- **[Field formats](REFERENCE.md#field-formats)** — datetime, reminders, RRULE, priority
- **[Examples](examples.md)** — complete usage examples for all commands
- **[Tags guide](REFERENCE.md#tags-标签)** — language matching, normalization
- **[Project types](REFERENCE.md#项目类型-project-kind)** — TASK vs NOTE
- **[Subtasks](REFERENCE.md#子任务-items)** — checklist format
- **[All fields](REFERENCE.md)** — complete API reference

## Error Handling

| Error | Solution |
|-------|----------|
| "No valid access token" | Run `dida365 auth login` |
| "Token expired" | Run `dida365 auth login` |
| "Project not found" | Run `dida365 projects list` for IDs |
| "401 Unauthorized" | Re-authenticate: `dida365 auth login` |

## Tagging Rules

- **Default:** Auto-adds `"cli"` tag
- **Disable:** Set `enableRequiredTags: false` in config
- **Language match:** Chinese title → Chinese tags; English → English tags

See [REFERENCE.md](REFERENCE.md#tags-标签) for full tagging guide.
