---
name: init-slice-workflow
description: Initialize the owner-driven slice workflow in a new or undocumented project by creating docs/ai scaffolding and drafting the first NEXT_SESSION.md.
disable-model-invocation: true
---

# Init Slice Workflow

Initialize the lightweight owner-driven slice workflow for this repo.

Use this when `docs/ai/PROJECT_MAP.md` or `docs/ai/NEXT_SESSION.md` does not exist yet, or when the user wants to adopt this workflow in an existing project.

This skill creates scaffolding. It does not start implementation.

## Principle

The user owns scope. The AI may draft project docs and the first slice packet, but must ask before implementing.

## Process

### 1. Check current state

Look for:

- `docs/ai/PROJECT_MAP.md`
- `docs/ai/NEXT_SESSION.md`
- `docs/ai/WORKLOG.md`
- `docs/ai/DECISIONS.md`
- repo-level guidance files such as `AGENTS.md`, `CLAUDE.md`, or similar

If files exist, do not overwrite them without asking.

### 2. Ask for the first slice

If the user did not already provide it, ask one question:

```text
What is the first small slice you want the next fresh AI session to implement?
```

If the project itself is unclear, ask for one short project description first.

Keep questions one at a time.

### 3. Create docs scaffolding

Create `docs/ai/` if missing.

Create missing files with the templates below.

#### `docs/ai/PROJECT_MAP.md`

```md
# Project Map

## Purpose

<one or two sentences about what this project is>

## Stack

- Engine/framework:
- Language:
- Main runtime:
- Main verify command:

## Structure

- `<path>` — `<role>`

## Entry points

- `<path>` — `<when to read/edit>`

## Conventions

- `<convention>`

## Unknowns

- `<unknown>`
```

#### `docs/ai/WORKLOG.md`

```md
# Worklog

Append one short receipt per completed slice.
```

#### `docs/ai/DECISIONS.md`

```md
# Decisions

Record only rare durable decisions.

Add an entry only when a decision is:

- hard to reverse
- surprising without context
- based on a real trade-off
```

### 4. Draft first `docs/ai/NEXT_SESSION.md`

Use the `next-session` packet shape.

The first packet must be small. Prefer a bootstrap slice that creates or verifies the thinnest runnable path.

For example:

- Unity: a minimal playable scene with one visible object
- Web app: one route rendering one simple state
- CLI: one command with one fixture
- Library: one public function with one test

Do not put a backlog in `NEXT_SESSION.md`.

### 5. Ask before saving broad scope

If the user gave clear approval to create the docs, save them.

If scope is fuzzy, show the drafts first and ask for approval.

Never proceed to implementation in the same step unless the user explicitly asks. Prefer telling the user to start a fresh session with:

```text
/start-slice
```

## Optional `AGENTS.md` pointer

If the repo has no guidance file and the user wants one, offer to create `AGENTS.md` with this minimal pointer:

```md
# AI Workflow

This repo uses the owner-driven slice workflow.

If `docs/ai/PROJECT_MAP.md` or `docs/ai/NEXT_SESSION.md` is missing, do not start implementation. Help the user run `/init-slice-workflow` first.

For implementation sessions:

- Use `/start-slice`
- Read `docs/ai/PROJECT_MAP.md`
- Read `docs/ai/NEXT_SESSION.md`
- Follow `NEXT_SESSION.md` strictly
- Implement only the current slice
- Do not edit outside `Allowed edit`
- Run listed verify before done
- Stop and ask if more scope is needed

Long-term docs:

- `docs/ai/PROJECT_MAP.md` = stable project map
- `docs/ai/NEXT_SESSION.md` = current slice packet
- `docs/ai/WORKLOG.md` = completed slice receipts
- `docs/ai/DECISIONS.md` = durable decisions
```

## Done response

When initialization is done, report:

1. Files created or updated
2. First slice id and goal
3. Any placeholders the user should fill
4. Next command to run, usually `/start-slice`
