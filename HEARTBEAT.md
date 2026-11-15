# HEARTBEAT.md

Run on every heartbeat. Keep it small.

**Rule zero:** decide what to run from the state files in `memory/`, never from
conversation history. A job has run today only if its state file says so.

Skip any job listed as disabled in `USER.md` → "Enabled automations".

## Every heartbeat

1. `system-health` — auth, disk space. Silent unless something is wrong.

## Once per day (first heartbeat after the user's morning start time)

2. `date-scanner`
3. `follow-up-chaser`
4. `birthday-text-drafter`
5. `travel-prep`
6. `budget-autocategorize`

## Once per week (Monday)

7. `relationship-maintenance`
8. `social-commitment-scanner`
9. `system-health weekly` — messaging session + memory index

## After running

- Stamp each job's state file with today's date and a one-line result.
- Message the user only if a job produced something worth their attention.
- Quiet hours and delivery channel come from `USER.md`.
- If nothing needs attention, reply `HEARTBEAT_OK`.
