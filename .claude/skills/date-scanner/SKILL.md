---
name: date-scanner
description: Daily scan of recent email and chats for dates, plans, and deadlines the user has agreed to that are not on the calendar. Runs daily from heartbeat.
---

# Date Scanner

## Setup

Accounts, chat IDs, and the delivery channel come from `USER.md` and the person's
`context-library/personal/<slug>/profile.md`. Skip this job if `USER.md` marks it off.

## Steps

1. Scan the last 2 days of mail for each account the agent may read, plus chat
   messages since the previous run (whichever messaging tools are connected). Note
   any source that was unavailable.
2. Extract anything with a date attached: plans the user said yes to, appointments,
   deliveries, deadlines, expiry dates, offers awaiting an answer.
3. Check each against the calendar. Keep only what is missing or clashes.
4. Compare with `pending` in the state file. Do not re-flag an item unless it changed.
5. If there is anything new, send:
   ```
   📆 Not on your calendar
   • <what> — <date> (<where it came from>)
   ⚠️ Clash: <event> overlaps <event>
   Add any of these?
   ```
6. Move items to `resolved` once they are on the calendar or declined.

## State

`memory/date-scanner-state.json`

```json
{
  "last_run": "YYYY-MM-DD",
  "last_result": "",
  "pending": [ { "event": "", "date": "", "flagged_at": "", "status": "" } ],
  "resolved": []
}
```
