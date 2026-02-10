# Dida365 CLI Examples

## Creating Tasks

### Simple Task
```bash
dida365 tasks create --title "Buy milk" --project-id PROJECT_ID
```

### Task with Due Date
```bash
# Today at 6pm
dida365 tasks create --title "Submit report" --project-id WORK_ID --due "2026-02-07 18:00"

# Next Friday 3pm
dida365 tasks create --title "Team meeting" --project-id WORK_ID --due "2026-02-13 15:00"
```

### Task with Reminders
```bash
# 30 min before due
dida365 tasks create --title "Call mom" --project-id PERSONAL_ID --due "2026-02-08 20:00" --reminder "-PT30M"

# Two reminders: 1 hour and 10 minutes before
dida365 tasks create --title "Dentist" --project-id PERSONAL_ID --due "2026-02-10 09:00" --reminder "-PT1H" --reminder "-PT10M"
```

### Recurring Task
```bash
# Every day at 9am
dida365 tasks create --title "Morning jog" --project-id HEALTH_ID --due "2026-02-08 09:00" --repeat-flag "RRULE:FREQ=DAILY;BYHOUR=9"

# Every Monday 9am
dida365 tasks create --title "Weekly review" --project-id WORK_ID --repeat-flag "RRULE:FREQ=WEEKLY;BYDAY=MO;BYHOUR=9"

# Every 2 hours (for hydration reminders)
dida365 tasks create --title "Drink water" --project-id HEALTH_ID --repeat-flag "RRULE:FREQ=HOURLY;INTERVAL=2"

# Workdays at 9am and 6pm
dida365 tasks create --title "Stand up" --project-id HEALTH_ID --repeat-flag "RRULE:FREQ=WEEKLY;BYDAY=MO,TU,WE,TH,FR;BYHOUR=9,18"
```

### Task with Priority
```bash
# High priority (3)
dida365 tasks create --title "URGENT: Fix production bug" --project-id BUG_ID --priority 3

# Medium priority (2)
dida365 tasks create --title "Review PR" --project-id CODE_ID --priority 2
```

### Task with Tags
```bash
# Add custom tags (multiple)
dida365 tasks create --title "Buy milk" --project-id PERSONAL_ID --tag shopping --tag urgent

# Add tag and disable auto "cli" tag
dida365 tasks create --title "Work task" --project-id WORK_ID --tag important --disable-required-tags
```

### Task with Checklist (Subtasks)
```bash
# Create base task first
dida365 tasks create --title "Prepare quarterly report" --project-id WORK_ID

# Then update with subtasks (via API or config)
# Subtasks format in task:
{
  "items": [
    { "title": "Collect data" },
    { "title": "Make slides" },
    { "title": "Send email" }
  ]
}
```

## Viewing Tasks

### View All Tasks (All Projects)
```bash
dida365 tasks get-all
```

### View Specific Project
```bash
# First list projects
dida365 projects list

# Then get tasks for specific project
dida365 tasks get-all --project-id 669dbc91ec40910af1328eec
```

### Force Refresh Cache
```bash
# Bypass cache, fetch fresh from API
dida365 tasks get-all --force-refresh
```

## Updating Tasks

### Update Title
```bash
dida365 tasks update --task-id TASK_ID --title "New title"
```

### Update Due Date
```bash
dida365 tasks update --task-id TASK_ID --due "2026-02-15 18:00"
```

### Add Reminder
```bash
dida365 tasks update --task-id TASK_ID --reminder "-PT30M"
```

## Searching Tasks

### Search by Keyword
```bash
# Search for tasks containing "review"
dida365 tasks search review

# Search with force refresh (bypass cache)
dida365 tasks search test --force-refresh

# Search in specific project
dida365 tasks search test --project-ids 669dbc91ec40910af1328eec

# Search for completed tasks only
dida365 tasks search report --status 2
```

### Search Options
| Option | Description |
|--------|-------------|
| `[query]` | Search keyword (positional argument) |
| `--status 0` | Active tasks only |
| `--status 2` | Completed tasks only |
| `--force-refresh` | Bypass cache |

## Completing Tasks

### Complete One Task
```bash
dida365 tasks complete --task-id TASK_ID
```

## Managing Projects

### List All Projects
```bash
dida365 projects list
```

## Authentication

### Check Login Status
```bash
dida365 status
```

### Login (OAuth)
```bash
dida365 auth login
```

### Logout
```bash
dida365 auth logout --force
```

## Configuration

### View Current Config
```bash
# Edit ~/.config/dida365.json
cat ~/.config/dida365.json
```

### Set Custom Tags
```json
{
  "requiredTags": ["work", "important"],
  "enableRequiredTags": true
}
```

### Set Timezone
```json
{
  "timezone": "America/New_York"
}
```

### Disable Auto Tag
```json
{
  "enableRequiredTags": false
}
```

## Common Workflows

### Daily Standup Reminder
```bash
# Create a daily 9am reminder
dida365 tasks create --title "Daily standup" --project-id WORK_ID \
  --due "2026-02-08 09:00" \
  --repeat-flag "RRULE:FREQ=DAILY;BYHOUR=9" \
  --reminder "-PT10M"
```

### Weekly Report (Every Friday 5pm)
```bash
dida365 tasks create --title "Weekly report" --project-id WORK_ID \
  --due "2026-02-13 17:00" \
  --repeat-flag "RRULE:FREQ=WEEKLY;BYDAY=FR;BYHOUR=17"
```

### Bi-weekly Meeting
```bash
dida365 tasks create --title "Sync meeting" --project-id WORK_ID \
  --due "2026-02-08 14:00" \
  --repeat-flag "RRULE:FREQ=WEEKLY;INTERVAL=2;BYDAY=MO;BYHOUR=14"
```

### Every 2 Hours Reminder
```bash
dida365 tasks create --title "Drink water" --project-id HEALTH_ID \
  --repeat-flag "RRULE:FREQ=HOURLY;INTERVAL=2"
```

## Error Recovery

### Token Expired
```bash
# Re-authenticate
dida365 auth login

# Then retry your command
dida365 tasks get-all
```

### Force Refresh (Stale Cache)
```bash
dida365 tasks get-all --force-refresh
```

### Wrong Project ID
```bash
# List projects to get correct ID
dida365 projects list
```
