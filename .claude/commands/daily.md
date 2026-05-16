---
description: Research today's news for the configured topic and write a daily update.
---

Run the `/daily` workflow defined in `AGENTS.md`. In short:

1. Read `config/topic.md`, `config/sources.md`, `config/format.md`, `config/outputs.md`.
2. Read the most recent file in `updates/daily/` (if any) so you don't repeat coverage.
3. Research per `config/sources.md`, respecting exclusions in `config/topic.md`.
4. Write `updates/daily/YYYY-MM-DD.md` (UTC date) using the daily template from `config/format.md`. Stay within the length budget.
5. If `config/outputs.md` declares extra outputs, update them in the same turn.
6. Stop. Do not touch `config/`. Do not summarize older dailies.

End-of-turn: one sentence on what was new, plus the path to the file you wrote.
