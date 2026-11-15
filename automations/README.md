# Automations

Templates for the scheduled side of Life OS. The onboarding skill renders them with
your answers into files that are gitignored:

| Template | Rendered to | What it is |
|----------|-------------|------------|
| `cron-jobs.example.json` | `cron-jobs.json` | OpenClaw cron jobs: the morning daily plan, budget tidy-up, one-off reminders |
| `launchd/travel-itinerary.plist.example` | `launchd/travel-itinerary.plist` | Weekly itinerary refresh (macOS launchd) |
| `launchd/wacli-sync.plist.example` | `launchd/wacli-sync.plist` | Daily WhatsApp sync so the scanners have fresh messages |

Everything else (date scanner, follow-up chaser, birthdays, travel prep, relationship
check, system health) runs from the heartbeat. See `HEARTBEAT.md`.

## Installing

Onboarding does not install anything without asking. To do it yourself:

**OpenClaw cron jobs.** Add the jobs from `cron-jobs.json` with your OpenClaw
version's cron command, or ask the agent to create them with `/schedule`.

**launchd (macOS).**

```bash
cp automations/launchd/*.plist ~/Library/LaunchAgents/
```

```bash
launchctl load ~/Library/LaunchAgents/<label>.plist
```

## Heartbeat settings worth copying

In your OpenClaw config, under `agents.defaults.heartbeat`:

```json
{ "isolatedSession": true, "lightContext": true }
```

Each heartbeat then starts in a fresh session and reads only `HEARTBEAT.md`. Without
this, a heartbeat can see its own earlier "already ran today" replies, trust them, and
quietly stop doing anything. State files in `memory/` are the only source of truth for
whether a job ran.
