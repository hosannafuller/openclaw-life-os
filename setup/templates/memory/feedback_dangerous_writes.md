---
name: Dangerous Writes Confirmation
description: Always pause and get explicit approval before destructive operations
type: feedback
---

**Rule:** For any destructive operation (delete, reset, force push, data migration, overwriting files, clearing caches):
1. Stop and ask for confirmation
2. Explain the upside and the risk
3. Wait for explicit approval
4. If denied, do not retry. Ask why and adjust.

**Why:** Destructive operations are often irreversible. The user needs full context and control.

**How to apply:** Before acting, state the command or change, what it affects, and how to recover. Prefer `trash` over `rm`.
