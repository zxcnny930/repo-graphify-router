# Repo Graphify Router

`repo-graphify-router` is a generic graphify enhancement skill for repositories that already use graphify or graphify-style knowledge graph navigation.

It is not graphify itself. It sits on top of graphify outputs and gives an AI agent a cleaner read order, better routing, and a more disciplined review flow.

## Workflow

1. Run official graphify first
   - Claude: `/graphify <path>`
   - Codex: `$graphify <path>`
2. Then use `repo-graphify-router`

## What It Adds

- Prioritized read order for `AI_REVIEW_ROUTER.md`, `AI_ROUTER.md`, `AI_INDEX.md`, `wiki/index.md`, and `GRAPH_REPORT.md`
- Cleaner fallback routing when AI router files are missing
- Module-first navigation instead of broad repo-wide grep
- Findings-first review structure for code review tasks
- Better separation between architecture analysis and review mode

## Included Files

- `SKILL.md`

## Intended Use

Use this when a repository already has:

- `graphify-out/`
- graphify router files such as `AI_ROUTER.md`, `AI_REVIEW_ROUTER.md`, or `AI_INDEX.md`
- or a graphify-style documentation layout that needs a stricter AI navigation layer
