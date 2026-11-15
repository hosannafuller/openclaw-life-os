---
name: system-health
description: Check that the agent's own plumbing still works (model auth, disk space, Google auth, messaging session, memory index) and alert only when something is wrong. Runs on every heartbeat, with extra weekly checks.
---

# System Health

## Setup

Accounts, chat IDs, and the delivery channel come from `USER.md` and the person's
`context-library/personal/<slug>/profile.md`. Skip this job if `USER.md` marks it off.

## Every heartbeat

| Check | Alert when |
|-------|-----------|
| Model auth | The agent's model login has expired |
| Disk space | Free space on `/` drops below 10 GB |
| Google auth | Any `gog` call returns an auth error. Tell the user to re-run `gog auth`. |

## Weekly (`system-health weekly`)

| Check | Alert when |
|-------|-----------|
| Messaging session | The WhatsApp CLI session is logged out or has not synced in 3+ days |
| Memory index | `MEMORY.md` points at files that no longer exist, or files in `memory/` are not indexed |
| Job freshness | Any state file's `last_run` is more than 2 days behind for a daily job |

Job freshness matters most. A heartbeat that replies OK without touching state files
looks alive while doing nothing.

## Behaviour

Silent when healthy. One short message per problem, with the exact command that fixes it.
Do not repeat an alert until the state changes.

## State

`memory/system-health-state.json` — last run, result per check, whether an alert is outstanding.
