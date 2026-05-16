# whats-new

A small, markdown-driven framework that turns your coding agent into a recurring "What's new" reporter on a topic of your choice. You configure the topic, sources, and format once during bootstrap; after that you run `/daily` or `/weekly` whenever you want a fresh update, and the agent maintains a tidy log of past updates without bloating itself.

## Quickstart

1. Clone this repo.
2. Open it with [Claude Code](https://docs.claude.com/en/docs/claude-code).
3. Run `/start`.

The `/start` command walks you through a one-time interview (topic, audience, sources, format, cadence), fills in the config files under `config/`, produces a first sample update, then deletes its own setup files. You only do this once per clone.

> **Note:** This framework uses `/start`, not `/init`. Claude Code's built-in `/init` generates a `CLAUDE.md` and would short-circuit this setup.

## Steady-state commands

After bootstrap, these are the commands you'll use:

- `/daily` — research today's news for your topic and write `updates/daily/YYYY-MM-DD.md`.
- `/weekly` — roll the past week's daily updates into a single digest at `updates/weekly/YYYY-Www.md`, then delete the dailies it consumed.
- `/monthly` — roll the past month's weekly digests into `updates/archive/YYYY-MM.md`, then delete the weeklies it consumed.

This three-tier compaction (daily → weekly → monthly) keeps the working set small so your agent's context stays focused on what's recent.

## What you maintain

All behavior is driven by markdown files you can read and edit directly:

- `AGENTS.md` — the contract your agent reads on every run. Keep it lean.
- `config/topic.md` — what subject to track, audience, goals, and exclusions.
- `config/sources.md` — where to look: URLs, search queries, RSS feeds, MCP tools.
- `config/format.md` — templates and length budget for daily / weekly / monthly outputs.
- `config/outputs.md` — any extra non-markdown outputs (e.g. a CSV the agent should keep in sync).

The agent will never silently rewrite these files. If it notices a stale source or wants to suggest a new exclusion, it appends to a `## Suggested updates (pending review)` section at the bottom of the relevant file. You promote those suggestions into the main sections yourself.

## One topic per clone

Each clone of this repo is dedicated to a single topic. To track a second topic, clone the repo into a different directory and run `/start` again. This keeps each agent loop focused on one subject and one set of sources.
