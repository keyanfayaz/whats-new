---
description: Roll the past month of weekly digests into a single monthly archive entry.
---

Run the `/monthly` workflow defined in `AGENTS.md`. In short:

1. Read `config/topic.md`, `config/sources.md`, `config/format.md`, `config/outputs.md`.
2. Collect every file in `updates/weekly/` whose ISO week falls in the most recently completed calendar month (UTC). If the user specified a different month in the prompt, use that.
3. If there are zero weekly files in the target month, stop and tell the user — don't fabricate an archive.
4. Write `updates/archive/YYYY-MM.md` using the monthly template from `config/format.md`.
5. Delete the weekly files you just consolidated. Do not delete weeklies from other months.
6. **Prune `updates/_ledger.md`:** remove every entry whose first-seen date is more than 14 days before today. Defense in depth — if `/weekly` was skipped near month-end, this still trims.
7. Stop. Do not touch `config/`.

End-of-turn: one sentence on the month's arc, the path to the archive file, the count of weeklies consolidated, and the count of ledger entries pruned.
