# memory/ seed files

Onboarding copies these into `memory/` (which is gitignored).

- `feedback_*.md` — default operating rules. Edit or delete to taste.
- `*-state.json` — starting state for jobs that need one. Every other job creates
  its own state file on first run.

State file convention, used by every background skill:

```json
{
  "last_run": "YYYY-MM-DD",
  "last_run_at": "ISO-8601 timestamp",
  "last_result": "one line"
}
```
