# Sources

## Primary sources

- WebSearch — built-in web search tool — primary discovery mechanism each run for both AI and sports.
- WebFetch — built-in fetch tool — pull and read specific articles found via search.
- Discover-for-me — pick the strongest credible sources at run time; prefer primary sources (company blogs, official league/team announcements) and established outlets over aggregators.

## Search queries

All queries below must be scoped to the trailing 24-48 hours. Use the search tool's date-range filter where available; otherwise append today's date (`YYYY-MM-DD`) or `past 24 hours` / `past 48 hours` to the query string.

- `AI model release OR product launch past 24 hours` — every daily run, AI side. Append today's date.
- `Anthropic OR OpenAI OR Google DeepMind OR Meta AI news past 24 hours` — every daily run, AI industry side.
- `AI developer tools OR coding agents news past 24 hours` — every daily run, AI tools side.
- `sports news today major headlines YYYY-MM-DD` — every daily run, sports side; substitute today's date.
- `NFL OR NBA OR MLB OR Premier League OR Champions League results today YYYY-MM-DD` — every daily run; skip off-season leagues; substitute today's date.

## Refresh notes

- AI industry: moves daily; always do a fresh search.
- Sports: moves daily; weight whatever is in-season. Skip off-season leagues rather than padding.
- If a search surfaces nothing genuinely new for a sub-topic on a given day, say so briefly rather than inflating thin items.

## Recency

- **48-hour rule.** Reject any candidate item whose underlying event is more than 48 hours old as of today (UTC), unless there is a concrete *new* development today (e.g. preview → GA, lawsuit-filed → settlement, lead changes hands in a series, model goes from announced to available).
- **Don't mistake a fresh article for a fresh event.** A new roundup that re-summarizes last week's news is not new.
- **Cross-check the ledger.** Before including any item, confirm it (or a same-slug variant) is not already in `updates/_ledger.md`. The ledger is the source of truth for "what we've already shipped."

## Suggested updates (pending review)
