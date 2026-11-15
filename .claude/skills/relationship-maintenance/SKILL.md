---
name: relationship-maintenance
description: Weekly check that flags close contacts the user has not exchanged direct messages with in 3+ weeks. Runs Mondays from heartbeat.
---

# Relationship Maintenance

## Setup

Accounts, chat IDs, and the delivery channel come from `USER.md` and the person's
`context-library/personal/<slug>/profile.md`. Skip this job if `USER.md` marks it off.

## Priority contacts

Read the list from `context-library/personal/<slug>/close-contacts.md`. If the file
does not exist, ask the user for 5 to 10 names and create it. That file is gitignored.

Also consider any contact with heavy message volume in the last six months who has
gone quiet.

## Steps

1. For each contact, find the date of the last direct message in either direction
   (WhatsApp, iMessage, or whichever tools are connected).
2. Flag anyone at 21+ days.
3. If anyone is flagged, send:
   ```
   💛 Relationship check
   Haven't spoken to:
   • <name> — <N> days
   Worth a quick message this week?
   ```
4. Otherwise stay silent.

## Rules

- Group chats do not count as contact.
- Do not flag the same person two weeks running.

## State

`memory/relationship-maintenance-state.json` — last run, last contact date per person, who was flagged.
