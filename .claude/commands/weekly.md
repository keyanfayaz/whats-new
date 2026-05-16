---
description: Roll the past week of daily updates into a single weekly digest.
---

Run the `/weekly` workflow defined in `AGENTS.md`. In short:

1. Read `config/topic.md`, `config/sources.md`, `config/format.md`, `config/outputs.md`.
2. Collect every file in `updates/daily/` whose date falls in the target ISO week. Default to the ISO week containing yesterday (UTC). If the user specified a different week in the prompt, use that.
3. If there are zero daily files in the target week, stop and tell the user — don't fabricate a digest.
4. Write `updates/weekly/YYYY-Www.md` using the weekly template from `config/format.md`.
5. Delete the daily files you just consolidated. Do not delete dailies from other weeks.
6. Stop. Do not touch `config/`.

End-of-turn: one sentence on the week's themes, the path to the digest, and the count of dailies consolidated.
