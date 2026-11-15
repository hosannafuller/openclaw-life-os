---
name: social-commitment-scanner
description: Weekly scan of personal chats for social commitments the user has made or is hosting that are not on the calendar. Runs weekly from heartbeat.
---

# Social Commitment Scanner

## Setup

Accounts, chat IDs, and the delivery channel come from `USER.md` and the person's
`context-library/personal/<slug>/profile.md`. Skip this job if `USER.md` marks it off.

## Steps

1. Scan direct and group chats since the last run (WhatsApp, iMessage, or whichever
   tools are connected).
2. Look for commitments: invitations accepted, events the user is hosting, RSVPs
   requested, "let's do X on Saturday" that got a yes.
3. Ignore noise: classifieds, menus, newsletters, reactions, general group chatter.
4. Check each commitment against the calendar.
5. Flag what is missing, with the evidence (who, when, what was said). Hosting
   commitments come first.
6. Keep a watchlist of plans that are forming but not yet confirmed.

## State

`memory/social-commitment-state.json`

```json
{
  "last_run": "YYYY-MM-DD",
  "pending": [ { "event": "", "date": "", "evidence": "" } ],
  "resolved": [],
  "watching": []
}
```
