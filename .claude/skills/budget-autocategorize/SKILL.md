---
name: budget-autocategorize
description: Daily job that assigns categories to transactions left in the catch-all category of the household budget sheet. Runs daily from heartbeat or cron.
---

# Budget Auto-categorise

## Setup

Accounts, chat IDs, and the delivery channel come from `USER.md` and the person's
`context-library/personal/<slug>/profile.md`. Skip this job if `USER.md` marks it off.

## Steps

1. Read the sheet ID, range, and column mapping from
   `context-library/shared/budget-targets.md`. No sheet configured: stop.
2. Find rows whose category is the catch-all (for example "General").
3. Apply the merchant rules table from the same file. For merchants with no rule,
   infer a category only when it is obvious; otherwise leave the row alone.
4. Write the category back to the category column only. Never touch amounts or dates.
5. Stay silent unless more than 20 rows changed or new merchants need a rule. Then
   send a short summary and offer to add rules.

## State

`memory/budget-autocategorize-state.json` — last run, rows changed, merchants with no rule.
