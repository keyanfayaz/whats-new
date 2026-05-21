---
description: Research the trailing 24-48h for the configured topic and write a daily update.
---

Run the `/daily` workflow defined in `AGENTS.md`. In short:

1. Read `config/topic.md`, `config/sources.md`, `config/format.md`, `config/outputs.md`.
2. **Read `updates/_ledger.md`.** This is your full memory of trailing-14-day coverage; treat every entry as already shipped. If the file doesn't exist, stop and ask the user to run `/backfill-ledger`.
3. Read the single most recent file in `updates/daily/` for tone/format reference only — not for dedup.
4. **Pre-flight guard:** if `updates/daily/` has 4 or more files (excluding `.gitkeep`), stop and tell the user to run `/weekly` first.
5. Research per `config/sources.md`, scoped to the trailing 48 hours (use the search tool's date filter, or append today's `YYYY-MM-DD` / `past 24 hours` to each query). Reject any item whose underlying event is more than 48 hours old unless there's a concrete new development today.
6. For each candidate item: generate an `entity-action` slug, match it against the ledger, and:
   - drop if already covered with no new event today,
   - prefix with `Update —` and re-use the slug if there *is* a new event.
7. Write `updates/daily/YYYY-MM-DD.md` (UTC) using the daily template from `config/format.md`. Stay within the length budget.
8. Append each new item to `updates/_ledger.md` under `## Active`, format `YYYY-MM-DD · <section> · <slug> · <8–15 word description>`.
9. If `config/outputs.md` declares extra outputs, update them in the same turn.
10. Stop. Do not touch `config/`. Do not summarize older dailies.

End-of-turn: one sentence on what was new, the file path, and the count of new ledger entries.
