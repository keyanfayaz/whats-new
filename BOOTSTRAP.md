# BOOTSTRAP

You are running the one-time setup for this repo. Follow these steps in order. Do not skip steps. Do not improvise structure — the `config/` shapes below are what every future run will rely on.

## Step 1 — Interview the user

Ask the questions below in a single `AskUserQuestion` round trip where it's natural to do so. Use multiple-choice options where you can; fall back to follow-up free-text questions only when the user picks something that requires more detail.

1. **Topic.** What subject do you want recurring "What's new" updates on? (One sentence is fine — e.g. "Anthropic product launches and Claude model releases.")
2. **Audience & goal.** Who reads these updates and what do they get out of them? (Shapes tone and depth — e.g. "Myself, to stay current for client work" vs. "My exec team, for weekly Monday briefings.")
3. **Cadence.** Daily, weekly, or both? (Determines which command files survive — see Step 4.)
4. **Sources.** Where should you look? Collect any combination of:
   - Specific URLs or RSS feeds (e.g. company blog, changelog page).
   - Search queries (e.g. `"Anthropic" site:news.ycombinator.com`).
   - MCP tools available in this environment (e.g. a news search MCP, a web fetch tool).
   - "Discover for me" — if the user is happy to let you pick sources during each run.
5. **Format preferences.** Bullet list or prose? Target length per update? Required sections (e.g. "Releases", "Rumors", "Worth reading")?
6. **Extra outputs.** Any non-markdown file you should also maintain alongside the updates? (e.g. `data/tracker.csv` with columns `date,headline,url,category`.) Most users will say "none."
7. **Exclusions.** Anything to specifically not cover? (e.g. "ignore stock-price chatter", "no rumors without two sources".)

## Step 2 — Materialize `config/`

Fill in each of the four files using exactly the shape below. The headings are load-bearing — future workflow runs grep for them. Leave the `## Suggested updates (pending review)` section empty (just the heading and nothing under it).

### `config/topic.md`

```
# Topic

## Subject

<one to three sentences describing what we track>

## Audience & goal

<who this is for and what they should get from each update>

## Exclusions

- <bullet per exclusion, or "None.">

## Suggested updates (pending review)
```

### `config/sources.md`

```
# Sources

## Primary sources

- <Name> — <URL or MCP tool name> — <one-line note on what it's good for>

## Search queries

- `<query>` — <when to run it, e.g. "every daily run", "weekly only">

## Refresh notes

- <Source name>: <how often it actually changes / whether to skip if recently checked>

## Suggested updates (pending review)
```

### `config/format.md`

```
# Format

## Length budget

- Daily: <e.g. 150–250 words, max 6 bullets>
- Weekly: <e.g. 400–600 words, max 10 bullets across all sections>
- Monthly: <e.g. 600–900 words, narrative paragraphs grouped by theme>

## Tone

<one to three sentences — e.g. "Plainspoken, no hype, link sources inline.">

## Daily template

<the exact markdown skeleton you will fill in for each daily file, including required headings>

## Weekly template

<the exact markdown skeleton for each weekly digest>

## Monthly template

<the exact markdown skeleton for each monthly archive>
```

### `config/outputs.md`

```
# Outputs

## Markdown outputs

- `updates/daily/YYYY-MM-DD.md` — written by `/daily`.
- `updates/weekly/YYYY-Www.md` — written by `/weekly`, consumes dailies.
- `updates/archive/YYYY-MM.md` — written by `/monthly`, consumes weeklies.

## Extra outputs

<Either "None." or, per extra output: path, schema, and update rule. Example:
- `data/tracker.csv` — columns `date,headline,url,category`. `/daily` appends one row per item covered in that day's update.>
```

## Step 3 — Trim `AGENTS.md`

Open `AGENTS.md`. Delete everything from the line containing `<!-- PRE-BOOTSTRAP:START -->` through the line containing `<!-- PRE-BOOTSTRAP:END -->`, inclusive. Verify the file now begins with the `# AGENTS.md` heading. This is what every future run will load.

## Step 4 — Drop unused command files

Based on the cadence the user picked in Step 1:

- **Daily only:** delete `.claude/commands/weekly.md`. Keep `.claude/commands/monthly.md` — archival is universal.
- **Weekly only:** delete `.claude/commands/daily.md`. Keep `.claude/commands/monthly.md`.
- **Both:** delete nothing.

## Step 5 — Run a first sample update

Execute the `/daily` workflow (or `/weekly` if the user picked weekly-only) once now, using the config you just wrote. This produces a concrete first output the user can react to immediately — and surfaces any gaps in the config before they bite.

Follow the workflow from `AGENTS.md` exactly. The first daily file should appear under `updates/daily/`.

## Step 6 — Self-destruct

Delete the following files:

- `BOOTSTRAP.md` (this file).
- `.claude/commands/start.md`.

The repo is now in steady state. Future runs of `/daily`, `/weekly`, or `/monthly` should never need to read anything you wrote here.

## Step 7 — Commit

Stage every change you've made in this turn and create one commit with the message:

```
chore: bootstrap whats-new for <short topic phrase>
```

Do not push. Do not open a PR. Leave that to the user.

## Step 8 — Hand off

In your final message to the user, in three or four short bullets:

- Confirm the topic, cadence, and number of sources configured.
- Point to the first sample update file.
- Remind them they can edit `config/*.md` any time, and that the agent will append suggestions there rather than overwriting.
- Tell them how to trigger the next update (`/daily` or `/weekly`).

Then stop.
