# AGENTS.md

This repo is a single-topic "What's new" reporter. The user runs `/daily`, `/weekly`, or `/monthly`; you produce a fresh, concise update against the config in this repo.

## Where to look

These files are read on every run, in this order:

- `config/topic.md` — the subject, audience, goal, and exclusions.
- `config/sources.md` — URLs, search queries, RSS feeds, and MCP tools to consult.
- `config/format.md` — templates and length budget for each output type.
- `config/outputs.md` — any non-markdown outputs you must also update.

`updates/_ledger.md` is read on every `/daily` as the trailing-14-day memory of headlines you've already shipped. `/weekly` and `/monthly` read it only to prune it.

Only read past files inside `updates/daily/`, `updates/weekly/`, or `updates/archive/` when the specific workflow below tells you to.

## Workflows

### `/daily`

1. Read all four `config/*.md` files.
2. **Read `updates/_ledger.md`.** This is your full memory of recent coverage; treat every entry as already shipped. If the file does not exist, stop and tell the user to run `/backfill-ledger` first.
3. Read the single most recent file in `updates/daily/` for tone/format reference only — not for dedup. The ledger is the source of truth for "what we've already covered."
4. **Pre-flight guard:** if `updates/daily/` already contains 4 or more files (excluding `.gitkeep`), stop. Tell the user to run `/weekly` before producing a new daily — compaction has fallen behind.
5. Research per `config/sources.md`, with explicit recency: every search query must scope to the trailing 48 hours (use the search tool's date filter where supported; otherwise append today's `YYYY-MM-DD` or `past 24 hours` to the query). Reject any candidate whose underlying event is more than 48 hours old, unless there is a concrete new development today (e.g. preview → GA, lawsuit-filed → settlement).
6. For each candidate item, before including it:
   - Generate a slug in `entity-action` kebab-case (e.g. `anthropic-mythos-glasswing`, `nba-wcf-set`).
   - Match the slug AND the candidate's entity + action against ledger entries. Same entity + same action = same story.
   - If matched and there is no concrete new event today → **drop it**.
   - If matched and there *is* a concrete new event → include it, prefix the headline with `Update —`, and use the same slug when appending to the ledger (tag the new entry `(update of <slug>)`).
7. Write `updates/daily/YYYY-MM-DD.md` (UTC date) using the daily template from `config/format.md`. Stay within the length budget.
8. **Append new items to `updates/_ledger.md`** under `## Active`, one bullet per item, format `YYYY-MM-DD · <section> · <slug> · <8–15 word description>`, kept in chronological order.
9. If `config/outputs.md` declares extra outputs, update them in the same turn.
10. Stop. Do not touch `config/`. Do not summarize older dailies — that's `/weekly`'s job.
11. End-of-turn: one sentence on what was new, the file path, and the count of new ledger entries.

### `/weekly`

1. Read all four `config/*.md` files.
2. Collect every file in `updates/daily/` whose date falls in the target ISO week (default: the week containing yesterday).
3. Write `updates/weekly/YYYY-Www.md` using the weekly template.
4. Delete the daily files you just consolidated.
5. **Prune `updates/_ledger.md`:** remove every entry whose first-seen date is more than 14 days before today. Do not touch newer entries; do not re-order; do not rewrite descriptions.
6. Stop.

### `/monthly`

1. Read all four `config/*.md` files.
2. Collect every file in `updates/weekly/` whose week falls in the most recently completed calendar month.
3. Write `updates/archive/YYYY-MM.md` using the monthly template.
4. Delete the weekly files you just consolidated.
5. **Prune `updates/_ledger.md`:** remove every entry older than 14 days from today. Defense in depth — if `/weekly` was skipped near month-end, this still trims.
6. Stop.

## Anti-pollution rules

- **Never modify `config/*.md` silently.** If you notice a stale source, a missing exclusion, or a format that isn't working, append a bullet under the `## Suggested updates (pending review)` heading at the bottom of the relevant config file. The user promotes suggestions into the main sections themselves.
- **Ledger discipline.** `updates/_ledger.md` is append-only during `/daily` (add new entries to `## Active`), and prune-only during `/weekly` and `/monthly` (delete entries older than 14 days). Never rewrite or re-order existing entries, never edit descriptions, and never hand-curate the ledger to "look cleaner." If a slug looks wrong, surface it to the user.
- **Stay inside the lines.** A `/daily` run writes one new file in `updates/daily/` (plus any extra outputs declared in `config/outputs.md`) and nothing else. A `/weekly` run writes one weekly file and deletes the dailies it consumed. A `/monthly` run writes one archive file and deletes the weeklies it consumed.
- **Surface any boundary crossing.** If a run needs to touch anything outside the files listed above — including this `AGENTS.md` — stop and ask the user first.
- **Keep this file lean.** If you find yourself wanting to add a paragraph here, it probably belongs in a `config/` file instead.
