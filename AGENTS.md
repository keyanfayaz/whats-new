<!-- PRE-BOOTSTRAP:START -->
## ⚠️ This project is not yet initialized

If you are reading this section, `BOOTSTRAP.md` still exists in the repo root, which means setup has not been run.

**Do not perform any other work on this repo until bootstrap is complete.** That includes: no research runs, no edits to `config/` (which is empty), no commits. Bootstrap configures the entire framework based on a short interview with the user.

Tell the user, in one short sentence, that they should run `/start` to initialize the project. Then stop.

(This block is removed automatically at the end of bootstrap. Once it's gone, the steady-state contract below is the only thing future runs will see.)

<!-- PRE-BOOTSTRAP:END -->

# AGENTS.md

This repo is a single-topic "What's new" reporter. The user runs `/daily`, `/weekly`, or `/monthly`; you produce a fresh, concise update against the config in this repo.

## Where to look

Read these on every run, in this order:

- `config/topic.md` — the subject, audience, goal, and exclusions.
- `config/sources.md` — URLs, search queries, RSS feeds, and MCP tools to consult.
- `config/format.md` — templates and length budget for each output type.
- `config/outputs.md` — any non-markdown outputs you must also update.

Only read past `updates/` files when the specific workflow below tells you to.

## Workflows

### `/daily`

1. Read all four `config/*.md` files.
2. Read the most recent file in `updates/daily/` (if any) to avoid repeating coverage.
3. Research per `config/sources.md`, respecting exclusions in `config/topic.md`.
4. Write `updates/daily/YYYY-MM-DD.md` (UTC date) using the daily template from `config/format.md`. Stay within the length budget.
5. If `config/outputs.md` declares extra outputs, update them in the same turn.
6. Stop. Do not touch `config/`. Do not summarize older dailies — that's `/weekly`'s job.
7. End-of-turn: one sentence on what was new, plus the file path.

### `/weekly`

1. Read all four `config/*.md` files.
2. Collect every file in `updates/daily/` whose date falls in the target ISO week (default: the week containing yesterday).
3. Write `updates/weekly/YYYY-Www.md` using the weekly template.
4. Delete the daily files you just consolidated.
5. Stop.

### `/monthly`

1. Read all four `config/*.md` files.
2. Collect every file in `updates/weekly/` whose week falls in the most recently completed calendar month.
3. Write `updates/archive/YYYY-MM.md` using the monthly template.
4. Delete the weekly files you just consolidated.
5. Stop.

## Anti-pollution rules

- **Never modify `config/*.md` silently.** If you notice a stale source, a missing exclusion, or a format that isn't working, append a bullet under the `## Suggested updates (pending review)` heading at the bottom of the relevant config file. The user promotes suggestions into the main sections themselves.
- **Stay inside the lines.** A `/daily` run writes one new file in `updates/daily/` (plus any extra outputs declared in `config/outputs.md`) and nothing else. A `/weekly` run writes one weekly file and deletes the dailies it consumed. A `/monthly` run writes one archive file and deletes the weeklies it consumed.
- **Surface any boundary crossing.** If a run needs to touch anything outside the files listed above — including this `AGENTS.md` — stop and ask the user first.
- **Keep this file lean.** If you find yourself wanting to add a paragraph here, it probably belongs in a `config/` file instead.
