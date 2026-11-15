---
name: birthday-text-drafter
description: Scan the calendar for birthdays in the next 7 days and draft a personal message for each, for the user to approve. Runs daily from heartbeat.
---

# Birthday Text Drafter

## Setup

Accounts, chat IDs, and the delivery channel come from `USER.md` and the person's
`context-library/personal/<slug>/profile.md`. Skip this job if `USER.md` marks it off.

## Steps

1. Fetch birthdays in the next 7 days across all calendars (events with "birthday" in
   the title, or the contacts birthday calendar).
2. For each one not already listed in `memory/birthday-drafter-state.json`:
   - Work out the relationship from recent chat history, if messaging tools are connected.
   - Skip if the user has already messaged them today.
   - Draft a short message in the user's own voice. Reference something real from recent
     conversation where possible.
3. Send each draft to the user:
   > 🎂 **<Name>'s birthday <today / on Friday>**
   > Draft: "<message>"
   > Reply YES to send, or edit and send it yourself.
4. Send only after an explicit YES, on the platform the user normally uses with that person.

## Voice

Short, warm, specific. Match how the user writes to that person. Emojis only where the
relationship is clearly casual. Never greeting-card prose.

## State

`memory/birthday-drafter-state.json`

```json
{ "last_run": "YYYY-MM-DD", "drafted": { "YYYY-MM-DD": ["<name>"] } }
```

## Needs

Calendar access. Optional: WhatsApp / iMessage CLI for history and sending.
