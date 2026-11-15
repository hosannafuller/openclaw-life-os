---
name: travel-prep
description: Detect trips starting in the next 48 hours from calendar and email, then send a pre-departure brief with times, accommodation, and weather. Runs daily from heartbeat.
---

# Travel Prep

## Setup

Accounts, chat IDs, and the delivery channel come from `USER.md` and the person's
`context-library/personal/<slug>/profile.md`. Skip this job if `USER.md` marks it off.

## What counts as travel

- Calendar events mentioning flights, trains, hotels, trips, or a city other than home
- Booking emails from airlines, rail operators, and accommodation sites
- Events of 2+ days located away from the home area in `household.md`

## Steps

1. Scan the next 48 hours of calendar. If sparse, search recent booking emails.
2. Skip trips already in `memory/travel-prep-state.json`.
3. Gather destination, departure and return times, accommodation, booking references.
4. Look up the weather for the destination and dates.
5. Send, about 24 hours before departure:
   ```
   ✈️ Trip brief — <destination>, <dates>
   🗓️ Departing: <time>
   🏨 Staying: <place>
   🌤️ Weather: <forecast>
   Reminders:
   • <passport, online check-in, anything specific to this trip>
   ```
6. Mark the trip as prepped.

## Rules

- Never check in to a flight or change a booking without explicit confirmation.

## State

`memory/travel-prep-state.json`

```json
{ "last_run": "YYYY-MM-DD", "prepped": { "<trip key>": "<ISO timestamp>" } }
```
