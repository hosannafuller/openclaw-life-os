---
name: follow-up-chaser
description: Scan sent email for threads where the user sent the last message and has had no reply for 3+ days, then send a chase list. Runs daily from heartbeat.
---

# Follow-up Chaser

## Setup

Accounts, chat IDs, and the delivery channel come from `USER.md` and the person's
`context-library/personal/<slug>/profile.md`. Skip this job if `USER.md` marks it off.

## Steps

1. Pull sent mail from the last 14 days for the agent-operated account.
   Example: `gog gmail search -a <account> "in:sent newer_than:14d" --limit 50`
2. Keep threads where the user's message is the latest and it is 3+ days old.
3. Drop noise: newsletters, no-reply addresses, receipts, and subjects that expect no
   reply ("FYI", "Thanks").
4. Group: 🔴 7+ days, 🟡 3 to 7 days.
5. If there is anything to chase, send:
   ```
   📬 Follow-up radar
   🔴 Overdue (7+ days)
   • <who>: "<subject>" — sent <date>
   🟡 Due soon (3–7 days)
   • <who>: "<subject>" — sent <date>
   Want me to draft a nudge for any of these?
   ```
6. Nothing to chase: stay silent.

Also list separately any thread where the *user* owes the reply. Those are not chases.

## Rules

- Never send a chase email without per-thread approval.
- Do not re-alert a thread already flagged unless its bucket changed.

## State

`memory/follow-up-chaser-state.json` — last run, flagged thread IDs with bucket.
