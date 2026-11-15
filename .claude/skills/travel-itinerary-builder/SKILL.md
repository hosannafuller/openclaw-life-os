---
name: travel-itinerary-builder
description: Build or refresh one document per upcoming trip, pulling booking details from email and messages. Runs weekly from a scheduled job.
---

# Travel Itinerary Builder

## Setup

Accounts, chat IDs, and the delivery channel come from `USER.md` and the person's
`context-library/personal/<slug>/profile.md`. Skip this job if `USER.md` marks it off.

## Steps

### 1. Discover trips

Search the agent-operated mailbox for confirmations covering the next 6 months
(flights, rail, hotels, rentals). Check connected messaging tools for dates and
logistics shared in chat. Deduplicate into distinct trips.

### 2. One document per trip

Look up the trip in `memory/travel-itinerary-state.json`. Refresh the existing doc
or create "<Destination> — <Month Year>":

```
# <Destination> — <Dates>
## ✈️ Transport       departure, return, carrier, booking ref
## 🏨 Accommodation   name, area, check-in/out, booking ref
## 📋 Activities      bookings, events, reservations
## 📞 Key contacts    host, hotel
## 🗒️ Notes           packing, visas, anything else
```

Save the doc ID and URL back to the state file.

### 3. Report

Send the user the docs refreshed this run, with links, and call out anything missing
(for example a trip with no accommodation found).

## State

`memory/travel-itinerary-state.json`

```json
{ "trips": { "<destination>-<month>-<year>": { "docId": "", "docUrl": "", "lastUpdated": "" } } }
```

## Needs

Email search and document create/update (for example `gog` with Gmail and Docs scopes).
