---
name: whatsapp-calendar
description: Add an event to the calendar when the user replies to or reacts to a chat message with a calendar or thumbs-up emoji. Use when an incoming message is just 📅 (or the user's configured trigger) quoting another message.
---

# Chat Message → Calendar

## Setup

Accounts, chat IDs, and the delivery channel come from `USER.md` and the person's
`context-library/personal/<slug>/profile.md`. Skip this job if `USER.md` marks it off.

## Trigger

The user replies with **📅** to a message, or reacts with the trigger emoji set in
their profile. The quoted or reacted-to message is the event.

## Steps

1. Read the quoted message.
2. Extract title, date, time, location, and who is involved. Resolve relative dates
   ("Friday", "next week") against today.
3. Missing time or ambiguous date: ask one short question.
4. Confirm in one line ("Adding: Dinner, Fri 27 Jun, 7pm — ok?") unless a rule in
   `memory/feedback*.md` says to add without asking.
5. Create the event on the calendar named in the user's profile for shared plans.
   Default duration is one hour. Add the location and a map link.
6. Reply: "✅ Added: <event>, <date> <time>".

If the message is clearly not an event, say so and ask what to add.
