---
name: schedule
description: Create, list, and delete local reminders and recurring jobs that message a household member. Use for "remind me every X", one-off nudges, and recurring household tasks.
user-invocable: true
---

# Schedule

Creates local scheduled jobs that run on the machine hosting this workspace and
message the user on their delivery channel (from `USER.md`).

## 1. Understand the request

- **What** should the reminder say?
- **Who** is it for? (default: the person asking)
- **When / how often?**
- **What time?** Default to the person's morning start unless they say otherwise.

## 2. Pick the mechanism

| Frequency | Use |
|-----------|-----|
| One-off | One-shot job at an exact date and time |
| Daily or several times a week | Recurring cron job |
| Weekly | Recurring cron job, made permanent (see below) |
| Every two weeks, monthly, or rarer | A row in the "Recurring chore schedule" table in `context-library/shared/fairplay-assignments.md`. The daily plan surfaces it on the right day. |

The chore table is the preferred home for household recurring tasks. It survives
restarts and needs no scheduler.

**Permanence.** Session-scoped cron jobs can expire. For anything meant to last,
also register it with the host scheduler (`automations/cron-jobs.json` for OpenClaw,
or a launchd plist from `automations/launchd/`). Check `memory/feedback*.md` for
whether the user wants to be asked first.

## 3. Write the job prompt

The prompt runs in a fresh session with local tools. Keep it self-contained:

```
Send <name> a reminder on their delivery channel (look up the chat ID in USER.md).
Message: "<emoji> <reminder text>"
```

Do not paste chat IDs or email addresses into skills or tracked files. Rendered job
files in `automations/` are gitignored and may contain them.

## 4. Cron expression

Local timezone. Avoid `:00` and `:30`; use minutes like 3, 7, 11.

- 9am weekdays: `3 9 * * 1-5`
- 9am Mondays: `3 9 * * 1`
- 9am on the 8th and 22nd: `3 9 8,22 * *`

## 5. Confirm, then create

Show what it will do and when it fires in plain language, then create it.

## Listing and deleting

- Household recurring tasks: read the chore table.
- Standing reminders: the "Standing reminders" table in the same file. Keep it in sync
  whenever a job is created or removed.
- Scheduler jobs: list from the host scheduler, confirm before deleting.
