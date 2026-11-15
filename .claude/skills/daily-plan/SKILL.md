---
name: daily-plan
description: Generate the morning plan for a household member. Pulls today's calendar, Fair Play duties, recurring chores, and a budget pulse, saves it to outputs/daily-plans/, and delivers a mobile-friendly summary. Use when asked for a daily plan, "what's on today", or from the morning cron job.
user-invocable: true
---

# Daily Plan

```
/daily-plan              compact plan for today (default)
/daily-plan full         adds budget pulse detail and the week ahead
/daily-plan tomorrow     evening preview of tomorrow
/daily-plan for <name>   plan for another household member
```

## 0. Who is this for?

Read `USER.md` to identify the person (chat ID or session). Load:

- `context-library/personal/<slug>/profile.md` — accounts, work pattern, preferences
- `context-library/shared/household.md` — rhythms, childcare, home help
- `context-library/shared/fairplay-assignments.md` — cards, weekly rhythm, chore table
- `context-library/shared/budget-targets.md` — if it exists
- `memory/night-rotation-state.json`, `memory/cleaning-state.json` — if they exist
- `memory/feedback*.md` — formatting and behaviour rules

Never read the other partner's private accounts or profile.

## 1. Same-day check

If `outputs/daily-plans/<date>-daily-plan.md` (or `<slug>-<date>-daily-plan.md`) exists,
ask whether to update, replace, or keep it. When run from cron, update silently.

## 2. Gather (in parallel)

**Calendar.** Today's events for the person's agent account and the shared account,
using the commands in their profile. Example with `gog`:

```bash
gog calendar events primary \
  --from "$(date +%Y-%m-%dT00:00:00)" --to "$(date +%Y-%m-%dT23:59:59)" \
  --account <account from profile>
```

No calendar access? Ask "What's on today?" and carry on.

**Carry-over.** Unchecked items in yesterday's plan.

**Fair Play.** Today's row from the weekly rhythm table, for this person and their partner.

**Recurring chores.** Compare today's date with every row of the chore table.
Due today goes in Home & Life. Due tomorrow goes in Heads Up. Appointments that need
booking get flagged 7 days ahead.

**Night duty.** If `tonightDuty` is known and fresh, state it. If stale, ask whose night it is.

**Home help.** If the cleaner is due today, say so with the payment reminder.
If due within two days, flag prep.

**Medical follow-up.** Scan the last 7 days of calendar for medical appointments.
If the household notes an insurance policy, ask once per appointment whether a claim
needs filing.

**Budget pulse** (only if a transaction sheet is configured). Pull the current month,
sum spending by category, compare to targets. Skip ignored categories. Collapse fixed
costs into one line. See the template in `budget-targets.md` for columns and date format.

## 3. Prioritise

Pick **Today's Three**: time-critical items first, then commitments to other people,
then whatever moves the week forward. Four or more hours of meetings means Today's Two.

Flag conflicts: a duty that collides with a meeting, a hard pick-up time with a late
call, a chore with no window.

## 4. Write the plan

Save to `outputs/daily-plans/YYYY-MM-DD-daily-plan.md` for the primary user and
`outputs/daily-plans/<slug>-YYYY-MM-DD-daily-plan.md` for anyone else.

```markdown
# Daily Plan — <Day>, <D Month YYYY>

## TL;DR
<one or two lines: shape of the day, the one thing not to drop>

## Today's Three
1. [ ] ...
2. [ ] ...
3. [ ] ...

## Schedule
- 09:00 <event> — <one line of context>

## Home & Life
### <Person> owns today
- 🚲 <timed task>
- 🍳 <task>
### <Partner> owns today (heads-up only)
- 🍲 <task>
### Shared
- 🌙 Night duty: <who>

## Budget check — <Month>, day X
Variable spend: <spent> vs <target> target — <under/over> by <amount> <✅ or 🔴>

🔴 Overspends
• <category>: <spent> vs <budget> (+<over>) — <what drove it, if one purchase stands out>

🚩 Flags
• <uncategorised total and count, or a category with no target that looks high>

🟢 Big wins
• <category>: <spent> vs <budget> — under by <amount>

Fixed costs: <amount>

## Heads Up
- <conflicts, tomorrow's chores, things to book, claims to file>
```

Formatting rules:

- Emoji bullets in Home & Life, one emoji per task type.
- Make the owner of every task obvious.
- Budget: lead with the overall under/over number. List overspends, then flags, then wins. Also warn (⚠️) on any category above 80% with under 10 days left. Name the purchase behind an overspend when one stands out, and say if it looks miscategorised.
- Omit any section with nothing in it.

## 5. Deliver

- Terminal session: show the plan.
- Cron or chat session: send TL;DR, Today's Three, and Home & Life to the person's
  chat ID from `USER.md`. No markdown tables (they do not render on mobile chat).
- Offer `/fairplay-reshuffle check` only if the week looks lopsided.

## Self-check

- [ ] Right person, right accounts
- [ ] Chore table checked against today's date
- [ ] Hard times (pick-ups, closing times) called out
- [ ] File saved
