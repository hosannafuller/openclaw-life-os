# Integrations

Life OS skills describe *what* to do. These tools give the agent the access to do it.
Install only what you want. Every skill degrades to asking you when a tool is missing.

**You run every auth step yourself.** The agent should never see a password, API key,
OAuth client secret, or bot token, and none of them belong in this folder.

| Tool | Used for | Skills that use it |
|------|----------|--------------------|
| OpenClaw | Gateway, heartbeat, cron, chat delivery | all background jobs |
| Claude Code | Running skills from a terminal | all |
| `gog` | Google Calendar, Gmail, Sheets, Docs | daily-plan, scanners, travel, budget |
| `wacli` | WhatsApp history and sending | scanners, birthdays, relationships |
| `imsg` | iMessage history and sending (macOS) | scanners, birthdays, relationships |
| Telegram bot | Where plans and alerts arrive | delivery |

## Google (`gog`)

1. In Google Cloud Console, create **your own** project and an OAuth client of type
   "Desktop app". Enable the Calendar, Gmail, Sheets, Docs, and Drive APIs you need.
2. Download the client JSON to a location **outside this repo**.
3. Run `gog auth` and sign in with the account the agent should operate.
4. Repeat for a shared household account if you have one.

Notes:

- `redirect_uri_mismatch` means the OAuth client is not a Desktop app, or the redirect
  URI registered in the console differs from the one the tool uses.
- Refresh tokens for apps in "testing" mode expire. Publish the consent screen to
  yourself, or expect to re-run `gog auth` now and then. `system-health` will tell you.
- Recommended: a dedicated Google account for the agent, with your calendars shared
  to it. Keep your private inbox out of reach.

## Telegram

1. Create a bot with BotFather and give the token to OpenClaw's channel setup
   directly. Do not paste it into chat or any file here.
2. Message your bot, then find your numeric chat ID (any "what's my ID" bot will
   tell you). Onboarding asks for the chat ID only.

## WhatsApp (`wacli`) and iMessage (`imsg`)

- `wacli`: link the session by scanning the QR code with your phone, then schedule
  the daily sync (`automations/launchd/wacli-sync.plist.example`).
- `imsg`: needs Full Disk Access for the process that runs the agent, so it can read
  the Messages database.

## Checklist

- [ ] Agent can list today's calendar events
- [ ] Agent can send you a test message on your delivery channel
- [ ] `git status` is clean after onboarding
