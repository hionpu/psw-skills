# Overview

This document is a deeper tour of `psw_skills`: what each skill does, how the skills connect, and how the whole set supports the owner-driven slice workflow.

Read `README.md` first for the short version. Read `WORKFLOW.md` for the full workflow spec.

## Mental model

The skill set has three layers.

```text
Layer 1: Workflow spine
  init-slice-workflow → next-session → start-slice → slice-cycle

Layer 2: Thinking and quality
  grilling, grill-with-docs, domain-modeling, codebase-design, code-review

Layer 3: Build and fix
  tdd, diagnosing-bugs, prototype, handoff
```

Layer 1 moves a project through slices. Layer 2 sharpens what to build and checks what was built. Layer 3 does the actual building, fixing, and context carrying.

## The workflow spine

These skills implement the core loop. The human owns scope; the AI drafts and implements.

### `init-slice-workflow`
Bootstraps a project. Creates `docs/ai/PROJECT_MAP.md`, `WORKLOG.md`, `DECISIONS.md`, and a first small `NEXT_SESSION.md`. Does not start implementation. Run once per project.

### `next-session`
Drafts or updates `NEXT_SESSION.md`, the single launch packet for the next fresh session. The packet lists one slice goal, non-goals, allowed read/edit files, acceptance criteria, verify commands, and stop conditions. Rule: AI drafts, human approves, then save. See `NEXT_SESSION_GUIDE.md` for the template.

### `start-slice`
Starts a fresh implementation session. Reads `PROJECT_MAP.md` and `NEXT_SESSION.md`, summarizes the slice, then implements only that slice. Stays inside `Allowed edit`. Stops and asks when scope is not enough.

### `slice-cycle`
A training-wheel skill that walks the full loop: clarify → draft packet → fresh session → implement → verify → record `WORKLOG` → prepare next slice. Useful while the habit is new; can fade once internalized.

## Thinking and quality skills

### `grilling`
A relentless interview that stress-tests a plan before building. Walks each branch of the design tree, resolves dependencies one at a time, and offers a recommended answer per question. Use when a slice is fuzzy.

### `grill-with-docs`
Like `grilling`, but also produces durable docs (ADRs and a glossary) as the interview goes. Use when the clarification should leave a paper trail.

### `domain-modeling`
Builds and sharpens a project's domain model and ubiquitous language. Records rare architectural decisions. Ships `ADR-FORMAT.md` and `CONTEXT-FORMAT.md` as reference formats.

### `codebase-design`
Shared vocabulary for designing deep modules: seams, interfaces, module depth, information hiding, locality, testability, AI-navigability. Ships `DEEPENING.md` and `DESIGN-IT-TWICE.md`. Use when improving a module's interface or deciding where a seam goes.

### `code-review`
Two-axis review of a diff against a fixed point (commit, branch, tag, merge-base):
- **Standards axis** — does the code follow the repo's coding standards?
- **Spec axis** — does the code match what the slice/issue asked for?

Runs both reviews in parallel and reports side by side. Use it to check that a slice implemented only its slice.

## Build and fix skills

### `tdd`
Red-green-refactor discipline. Ships `tests.md` and `mocking.md`. Use when a slice has a clean test seam.

### `diagnosing-bugs`
A diagnosis loop for hard bugs and performance regressions. Builds a tight reproduction and feedback loop before changing code. Ships helper scripts. Use instead of guessing at a fix.

### `prototype`
Builds throwaway code to answer a design question — does this state model or logic feel right, what should this UI look like. Ships `LOGIC.md` and `UI.md`. The output is meant to be discarded.

### `handoff`
Compacts the current conversation into a handoff document so another agent or a later session can pick up cleanly. Use when branching work or crossing a context boundary.

## How a full slice flows

A typical slice touches several skills:

```text
1. Fuzzy idea            → grilling / grill-with-docs
2. Write the contract    → next-session  (edits NEXT_SESSION.md)
3. Fresh session begins  → start-slice
4. Implement             → tdd  (or plain edits)
5. Bug appears           → diagnosing-bugs
6. Design question       → prototype  /  codebase-design
7. Review the diff       → code-review
8. Close the loop        → append WORKLOG.md, update PROJECT_MAP.md,
                           draft next NEXT_SESSION.md via next-session
```

Not every slice needs every skill. Most slices use only `start-slice`, an edit or `tdd`, and a `WORKLOG` receipt.

## Skill file conventions

Every skill lives in `skills/<name>/SKILL.md` with YAML front matter:

- `name` — the skill id, matches the folder name.
- `description` — written so an agent can decide when to trigger the skill. This is the most important field to get right.
- `disable-model-invocation: true` — set on skills that should only run when explicitly invoked (for example `start-slice`, `next-session`, `handoff`), not auto-triggered by the model.
- `argument-hint` — optional, hints at an expected argument.

Helper docs (like `tests.md` or `DEEPENING.md`) sit next to the `SKILL.md` that references them. A skill resolves relative paths against its own folder.

## Design principles behind the set

- **Owner owns scope.** Skills provide discipline, not enforcement.
- **Small durable docs beat a workflow engine.** Four short files hold memory; no FSM owns the process.
- **One skill, one job.** Keep skills small and composable.
- **Trigger wording matters.** A skill only helps if its `description` fires at the right time.
- **Remove process before adding automation.** If the workflow feels heavy, cut it. See Anti-goals in `WORKFLOW.md`.

## Where to go next

- `README.md` — short intro and install notes
- `WORKFLOW.md` — the full owner-driven slice workflow
- `NEXT_SESSION_GUIDE.md` — how to write the slice packet
- `skills/<name>/SKILL.md` — the source of truth for each skill
