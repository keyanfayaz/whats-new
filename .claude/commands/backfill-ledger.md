---
description: One-time. Builds updates/_ledger.md from existing dailies and weeklies, then deletes itself.
---

This is a one-shot bootstrap for the coverage ledger. Follow these steps exactly.

1. **Refuse if ledger exists.** If `updates/_ledger.md` already exists, stop and tell the user it's already populated. Do not overwrite.
2. **Collect source files.** Read every file in `updates/daily/*.md` and `updates/weekly/*.md` (ignore `.gitkeep`).
3. **Extract entries.** For each bullet item in those files:
   - **Date** — for dailies, parse from the filename (`YYYY-MM-DD.md`). For weeklies, parse from the digest header line (`week of YYYY-MM-DD`); use that Monday as the date.
   - **Section** — from the nearest preceding `## <heading>` (lowercase: `ai`, `sports`, etc.).
   - **Slug** — generate an `entity-action` kebab-case identifier from the headline. Be consistent: if the same story appears across multiple files, all instances should produce the same slug. Examples: `anthropic-mythos-glasswing`, `openai-deployment-company`, `nba-wcf-set`, `meta-muse-spark`.
   - **Description** — an 8-15 word condensation of the bullet's main clause. Strip URLs and `[source]` markdown.
4. **Deduplicate.** If the same slug appears in multiple files, keep the **earliest** date as the entry's `first-seen` date and drop the rest.
5. **Sort chronologically** by date ascending, then by section (`ai` before `sports`).
6. **Trim to 14 days.** Drop any entry whose date is more than 14 days before today (UTC).
7. **Write `updates/_ledger.md`** using this format exactly:

   ```
   # Coverage ledger

   Trailing 14 days of headlines already reported. /daily reads this before
   researching to avoid re-printing old news. /weekly and /monthly prune entries
   older than 14 days. Append-only via /daily; prune-only via /weekly + /monthly.

   ## Active

   - YYYY-MM-DD · <section> · <slug> · <8–15 word description>
   - ...
   ```

8. **Delete this file** (`.claude/commands/backfill-ledger.md`).
9. **Commit.** Single commit with message:
   ```
   chore: backfill coverage ledger from existing updates
   ```
   Stage both `updates/_ledger.md` (created) and `.claude/commands/backfill-ledger.md` (deleted).
10. **Report.** In your final message: the count of ledger entries, the date range covered, and the count of files scanned. Then stop.
