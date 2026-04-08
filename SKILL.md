---
name: repo-graphify-router
description: Use this skill when working in any repository that uses graphify or graphify-style knowledge graph navigation and you need an AI navigation enhancement layer. It is a graphify enhancement skill, not graphify itself. It tells the agent how to read graphify-out files in the right order, how to fall back when AI router files are missing, and how to route architecture analysis and code review to the right files with minimal broad search.
---

# Repo Graphify Router

## Overview

Use this skill when the task depends on navigating a repository through `graphify-out/` or a graphify-style knowledge graph workflow.

This skill is a graphify enhancement layer. It does not replace graphify or replace engineering judgment. The intended order is: first run the official graphify skill, then use this skill to navigate and review the resulting graph outputs. Official invocation depends on the assistant:
Codex: `$graphify <path>`
Claude: `/graphify <path>`

## When To Use

Use this skill when any of these are true:

- The repo has a `graphify-out/` directory.
- The user asks for architecture, codebase analysis, code review, regression analysis, or "AI-specific graphify" flow.
- The task mentions `AI_ROUTER.md`, `AI_REVIEW_ROUTER.md`, `AI_INDEX.md`, `GRAPH_REPORT.md`, or `wiki/index.md`.

Do not use this skill for tasks unrelated to repo navigation or review.

## Workflow

### 0. Run graphify first

If the user wants fresh graph output, the repo has no `graphify-out/`, or the existing graph looks stale, first invoke the official graphify skill:

```text
Codex: $graphify <path>
Claude: /graphify <path>
```

These are the official graphify entrypoints for Codex and Claude. Do not substitute them with a shell command such as `graphify <path>` unless the repo explicitly documents a separate terminal CLI workflow.

Only start this enhancement after graphify has already produced or refreshed `graphify-out/`, or when the user explicitly wants routing on top of existing graph outputs.

### 1. Pick the mode first

- For code review tasks, prefer `graphify-out/AI_REVIEW_ROUTER.md`.
- For architecture or codebase questions, prefer `graphify-out/AI_ROUTER.md`.

If both files exist, use the mode-specific file first.

### 2. Follow the graphify read order when available

For code review:

1. `graphify-out/AI_REVIEW_ROUTER.md` if present
2. `AGENTS.md` if present
3. `graphify-out/AI_ROUTER.md` if present
4. `graphify-out/AI_INDEX.md` if present
5. `CHANGELOG.md` if present
6. Target module files
7. Matching docs only if the change touches a documented external system or protocol

For architecture or codebase analysis:

1. `graphify-out/AI_ROUTER.md` if present
2. `graphify-out/AI_INDEX.md` if present
3. `graphify-out/wiki/index.md` if present
4. `graphify-out/GRAPH_REPORT.md` only for graph-structure context
5. Raw files only in the chosen module

### 3. Fall back cleanly when AI router files are missing

If the repo has `graphify-out/` but is missing some or all AI navigation files:

1. Check `README.md`, `CHANGELOG.md`, `ARCHITECTURE.md`, `CLAUDE.md`, and `AGENTS.md` if present.
2. Find the primary runtime entry points:
   Common examples: `main.*`, `index.*`, `cmd/main.go`, `src/index.ts`, `app.*`, `server.*`
3. Identify the major runtime modules:
   Common examples: `internal/`, `src/`, `pkg/`, `app/`, `server/`, `web/`, `docs/`
4. Route to one module first before any broad search.

Do not block on missing `AI_ROUTER.md` or `AI_INDEX.md`. Use the repo's existing docs and entry points as the fallback routing layer.

### 4. Route by intent instead of broad search

Choose the narrowest likely module first:

- Startup, wiring, dependency graph:
  entrypoint files, config modules, app wiring modules
- Execution or business logic:
  runtime modules under `internal/`, `src/`, `pkg/`, or equivalent
- API, dashboard, or UI:
  `api/`, `server/`, `web/`, `frontend/`, `ui/`
- Persistence or data flow:
  `database/`, `db/`, `store/`, `state/`
- External integration:
  protocol adapters, client modules, or matching docs
- Risk, validation, policy, or auth:
  `risk/`, `policy/`, `auth/`, validation modules, supporting docs

Do not begin with repo-wide grep unless the routed entry points are insufficient.

## Review Focus

When reviewing changes, optimize for:

- correctness bugs
- behavioral regressions
- missing wiring
- interface or contract mismatches
- missing tests

Prioritize these checks:

- data shape or schema mismatches
- inbound and outbound format conversion mistakes
- retry, idempotency, or duplicate-execution bugs
- reconnection or lifecycle leaks
- stale state or race-prone updates
- missing config, API, persistence, or UI wiring for new fields
- hidden behavior changes implied by changelog or recent refactors

## Search Rules

- Prefer graphify navigation files over blind search.
- Prefer one module plus one supporting doc over many files.
- Exclude dependency trees, generated outputs, caches, and vendored code unless directly relevant.
- If the repo is a multi-project workspace, choose one subproject first.

## Output Rules

For code review output:

- Findings first, ordered by severity
- Include file and line references
- Open questions or assumptions second
- Summary only last

For architecture or analysis output:

- State the route you followed
- Separate graphify-derived routing from your own fallback inference when applicable
- Use `graphify-out/GRAPH_REPORT.md` only when discussing graph structure, communities, god nodes, or surprising edges

## Handoff Rule

When handing work to another agent, include the read order you used and whether you followed a graphify router file or a fallback path.

## Maintenance Rule

After modifying code files in a repo that uses graphify, prefer rerunning the official graphify skill when you need fresh graph output:

```text
Codex: $graphify <path>
Claude: /graphify <path>
```

If the local repo explicitly documents a lighter-weight code-only rebuild command, you may use that local command instead. A common example is:

```powershell
python3 -c "from graphify.watch import _rebuild_code; from pathlib import Path; _rebuild_code(Path('.'))"
```
