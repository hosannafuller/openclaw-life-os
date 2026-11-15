---
name: fairplay-reshuffle
description: Facilitate a Fair Play reshuffle between partners. Review current card assignments, surface imbalances, and negotiate a new deal. Use for a full reshuffle, a quick imbalance check, or moving a single card.
user-invocable: true
---

# Fair Play Reshuffle

A reshuffle is a scheduled check-in where both partners review who holds which
household responsibilities and renegotiate. Aim for one every 3 to 6 months, or when
life changes (new job, new child, travel season).

```
/fairplay-reshuffle                      full facilitation
/fairplay-reshuffle check                quick imbalance check
/fairplay-reshuffle status               card counts and split
/fairplay-reshuffle add <card> to <who>  move one card
```

## Context

Load `context-library/shared/fairplay-assignments.md` and
`context-library/shared/household.md`. Player names come from `USER.md`.

## Workflow

### 1. Current state

Summarise each player's card count by category (Home, Out, Caregiving, Magic).
Flag cards marked "not in play" that may now be relevant, and anything shared that
has no clear owner.

Ask:
> Is there anything that has felt off or unfair since the last reshuffle? Any cards that
> feel too heavy, or cards you are doing but do not officially hold?

### 2. Imbalances

- Which card has felt most invisible lately?
- Is there a card you hold where your partner does not see how much is involved?
- Is there a card your partner holds that you would like back?
- What is coming up that adds cards (travel, school year, health)?

### 3. Cards on the table

List cards either player wants to drop, cards that should come into play, and cards
that could be split. For each, spell out the full scope: conception, planning, execution.

### 4. New deal

Draft the new split. Show before and after counts. Say so if it looks lopsided.
Suggest a trial: "Try this for six weeks, then check in?"

### 5. Save

After both agree:

1. Update `context-library/shared/fairplay-assignments.md`, including the weekly
   rhythm table if duties moved days.
2. Add a row to the reshuffle log with today's date.
3. Offer to update reminders that reference a moved card (`/schedule`).

Confirm what changed in one or two lines.
