---
name: onboarding
description: First-run setup for a freshly cloned Life OS. Interviews the user, then generates their identity, household, Fair Play, budget, memory, and automation files from the templates in setup/templates/. Run this before anything else when USER.md is missing, or re-run it to change answers.
user-invocable: true
---

# Onboarding

Turn a blank clone into this person's Life OS. You interview, they answer, you write
the files. Nothing personal is ever written to a tracked file.

## Ground rules

- **Interview, do not hand over a form.** Ask a few questions at a time. Offer
  sensible defaults. Let people skip anything; skipped sections stay off.
- **Never ask for secrets.** No passwords, API keys, tokens, bot tokens, card or bank
  numbers. When a tool needs credentials, tell the user which command to run
  themselves (see `setup/integrations.md`).
- **Ask for the minimum.** A neighbourhood, not a street address. First names for
  children. No dates of birth, no health details unless the user volunteers them
  for a reminder.
- **Write only to gitignored paths:** `USER.md`, `IDENTITY.md`, `MEMORY.md`,
  `context-library/personal/`, `context-library/shared/`, `memory/`,
  `automations/cron-jobs.json`, `automations/launchd/*.plist`.
  Never edit `setup/templates/`, skills, or `.gitignore`.
- **Re-runs.** If `USER.md` already exists, show what is configured and ask what to
  change. Do not overwrite a file without showing the difference first.

## Step 0 — Preflight

1. Confirm you are in the repo root (`setup/templates/` exists).
2. Run `git check-ignore -q USER.md memory/x context-library/personal/x` and stop if
   any of them is not ignored. Personal files must not be committable.
3. Note which tools are installed (`which gog wacli imsg openclaw`). This decides
   which automations you can offer. Missing tools are fine.
4. Tell the user what is about to happen: about 10 minutes, five short rounds,
   everything stays on this machine.

## Step 1 — You

- What should I call you?
- Timezone and currency (guess from the system, confirm).
- Solo, or setting this up with a partner? Any children? (first names, rough age,
  nursery or school)
- Roughly where is home? (city or neighbourhood)
- Work pattern: which days at home, which away, usual commute.
- How do you like to be talked to? Tone, length, pet hates.

## Step 2 — Your agent

- Name, emoji, and a one-line vibe for the assistant. Offer three suggestions if they
  have no idea.
- If a partner will use it too: do they want their own agent name?

## Step 3 — Channels and accounts

- Where should plans and alerts arrive? (Telegram, WhatsApp, terminal only.) For
  Telegram, ask for the numeric chat ID and explain how to find it. Do not ask for
  the bot token.
- Which Google account should the agent operate? Recommend a dedicated account
  with calendars shared to it, rather than a personal inbox.
- Is there a shared household account or calendar? Read-only or full access?
- Any account the agent must never touch? Record it as off-limits.
- Morning start time and quiet hours.

## Step 4 — Household (skip for solo setups with no shared home)

- Morning duty and dinner by day of week.
- Childcare: provider, drop-off and pick-up times, closing time, evening routine,
  how night duty is split.
- Home help: cleaner frequency and day, how they are coordinated and paid
  (the method only, never account details).
- Recurring chores with their dates (bedsheets, appliance cleans, bills).
- Fair Play: do they use it? If yes, walk the four categories and record who holds
  which cards, using their own deck for the card names. If no, record a plain list
  of who owns what.
- Budget: is there a transaction sheet? Sheet ID, range, columns, categories, and
  monthly targets. Skip entirely if not.

## Step 5 — Automations

List each automation with one line on what it does and what it needs. Mark each
on or off. Only offer what the installed tools support.

| Automation | Needs |
|------------|-------|
| daily-plan | calendar |
| date-scanner | mail, optional chat tools |
| follow-up-chaser | mail |
| birthday-text-drafter | calendar, optional chat tools |
| relationship-maintenance | chat tools |
| social-commitment-scanner | chat tools |
| travel-prep | calendar or mail |
| travel-itinerary-builder | mail and docs |
| whatsapp-calendar | chat tools, calendar |
| budget-autocategorize | budget sheet |
| system-health | nothing |

For `relationship-maintenance`, ask for 5 to 10 close contacts and write them to
`context-library/personal/<slug>/close-contacts.md`.

## Step 6 — Generate

Slugs are lower-case first names. For every template, replace each `{{PLACEHOLDER}}`
with the answer, and delete rows or sections the user skipped. No `{{` may remain in
a generated file.

| Template | Writes |
|----------|--------|
| `setup/templates/USER.md.tmpl` | `USER.md` |
| `setup/templates/IDENTITY.md.tmpl` | `IDENTITY.md` |
| `setup/templates/MEMORY.md.tmpl` | `MEMORY.md` |
| `setup/templates/context-library/personal/_person/profile.md.tmpl` | `context-library/personal/<slug>/profile.md`, once per person |
| `setup/templates/context-library/shared/household.md.tmpl` | `context-library/shared/household.md` |
| `setup/templates/context-library/shared/fairplay-assignments.md.tmpl` | `context-library/shared/fairplay-assignments.md` |
| `setup/templates/context-library/shared/budget-targets.md.tmpl` | `context-library/shared/budget-targets.md` (only if a sheet was given) |
| `setup/templates/memory/*` | `memory/` (skip `README.md`) |
| `automations/cron-jobs.example.json` | `automations/cron-jobs.json`, enabled jobs only |
| `automations/launchd/*.plist.example` | `automations/launchd/*.plist`, enabled jobs only |

Write `USER.md` last. Its existence is what marks onboarding as complete.

## Step 7 — Verify

1. `grep -rn '{{' USER.md IDENTITY.md MEMORY.md context-library memory automations/cron-jobs.json`
   returns nothing.
2. `git status --porcelain` shows no new or modified files. If it shows any, something
   personal landed in a tracked path. Fix it before continuing.
3. If calendar access exists, fetch today's events as a smoke test.

## Step 8 — Hand over

- Summarise what was set up and what was skipped, in under ten lines.
- List the commands the user still has to run themselves (auth, installing jobs),
  taken from `setup/integrations.md` and `automations/README.md`. Do not install
  scheduled jobs without an explicit yes.
- Offer to run `/daily-plan` now.
