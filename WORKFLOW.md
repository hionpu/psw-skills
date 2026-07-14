# Owner-Driven Slice Workflow

This workflow is for solo development with AI pair programming.

It replaces the previous overengineered forced workflow. The old approach tried to make a deterministic harness own the process. This approach keeps the human owner in control and uses small documents and skills only where they help.

## What changed

### Old approach: forced workflow

The old `slicefsm` direction centered on a deterministic state machine:

- The harness owned phases.
- The harness proposed or enforced slices.
- Tool whitelist and hooks forced the agent through states.
- Context extraction decided what the agent should read.
- Approval, discovery, slicing, stuck states, scale checks, and review gates became part of the engine.

This can help when many people or mostly autonomous agents run the same workflow. But for solo development it can become a meta-project. The process starts to cost more attention than the game or app being built.

### New approach: owner-driven slices

The new workflow is lighter:

- The human owner defines the slice.
- The AI implements only that slice.
- `NEXT_SESSION.md` is the launch packet for one fresh session.
- `PROJECT_MAP.md` holds the durable project structure map.
- `WORKLOG.md` records completed slice receipts.
- `DECISIONS.md` records rare durable decisions.
- Skills provide discipline, not enforcement.

The harness is no longer the planner. The owner is the planner and reviewer. The AI is the drafter, implementer, and summarizer.

## Core principle

```text
Human plans.
AI implements.
Human verifies.
Docs keep only durable context.
```

Do not build a workflow engine unless the current pain demands one.

## Standard files

Put these in each project, usually under `docs/ai/`:

```text
docs/ai/PROJECT_MAP.md
docs/ai/NEXT_SESSION.md
docs/ai/WORKLOG.md
docs/ai/DECISIONS.md
```

### `PROJECT_MAP.md`

Stable structure map of the project.

Use it for:

- important directories
- module roles
- common entry points
- testing conventions
- terms that help the AI navigate

Do not put temporary task notes here.

### `NEXT_SESSION.md`

The current next-session launch packet.

It acts as the current slice contract. Keep only one. Overwrite it before the next fresh session.

Use it for:

- one slice goal
- non-goals
- allowed read files
- allowed edit files
- acceptance criteria
- verify commands
- stop conditions

Do not use it as history, backlog, or long-term memory.

### `WORKLOG.md`

Append-only receipts for completed slices.

Use it for:

- date
- slice id/title
- files changed
- behavior shipped
- verification result
- short follow-up notes

Keep entries short.

### `DECISIONS.md`

Rare durable decisions.

Use it only when a decision is:

- hard to reverse
- surprising without context
- based on a real trade-off

Most choices do not belong here.

## The cycle

### 1. Clarify the next slice

Start with a small goal. If it is fuzzy, use `grilling` or `grill-with-docs`.

Clarify:

- what user-visible behavior should change
- what is out of scope
- which files are likely relevant
- which files may be edited
- how to verify the result
- when the AI should stop and ask

The owner makes final scope decisions.

### 2. Draft `NEXT_SESSION.md`

Use the `next-session` skill or write it manually.

Default rule:

```text
AI drafts.
Human approves.
Only then save.
```

The packet should be short, concrete, and restrictive.

### 3. Start a fresh session

In the fresh session, use:

```text
/start-slice
```

The skill reads:

- `docs/ai/PROJECT_MAP.md`
- `docs/ai/NEXT_SESSION.md`

Then it summarizes the slice before editing.

### 4. Implement only the slice

The AI must stay inside `NEXT_SESSION.md`.

Rules:

- no scope broadening
- no unrelated refactor
- no edits outside `Allowed edit`
- ask before reading outside `Allowed read` when possible
- stop if the listed files are not enough
- stop if the test seam is unclear
- stop if verification fails twice for the same reason

Use `tdd` when the slice has a good test seam.

### 5. Verify

Run the listed verify command.

For Unity or game prototype work, manual Play Mode checks can be the main verification. Record them clearly.

The AI reports:

1. files changed
2. behavior implemented
3. verification run and result
4. durable structure learned, if any
5. suggested next slice, if any

### 6. Human review and manual check

The owner checks the result.

For code-heavy changes, use `code-review` against the slice contract:

- Standards axis: code quality and smells
- Spec axis: did it implement only this slice?

For bugs, use `diagnosing-bugs` instead of guessing.

### 7. Close the cycle

After the owner accepts the slice:

1. Append a short receipt to `WORKLOG.md`.
2. Update `PROJECT_MAP.md` only if durable structure was learned.
3. Update `DECISIONS.md` only if a durable decision was made.
4. Draft the next `NEXT_SESSION.md`.
5. Human approves the next packet.

Then start the next fresh session.

## Recommended skills

Core:

- `init-slice-workflow` — create `docs/ai/` scaffolding in a new project
- `start-slice` — start a fresh implementation session from `NEXT_SESSION.md`
- `next-session` — draft the next launch packet
- `grilling` — sharpen a fuzzy slice
- `tdd` — red-green-refactor discipline
- `diagnosing-bugs` — build a tight feedback loop before fixing bugs
- `code-review` — review diff against standards and spec

Useful when needed:

- `grill-with-docs` — clarification plus domain docs
- `domain-modeling` — sharpen terminology and rare ADRs
- `codebase-design` — seam, interface, depth, locality vocabulary
- `prototype` — answer a UI or state-model question with throwaway code
- `handoff` — preserve context when branching or crossing sessions

Optional training wheel:

- `slice-cycle` — reminds the full loop, but should fade once the habit is internalized

## Anti-goals

Do not rebuild the old forced workflow.

Avoid:

- automatic scale detection
- automatic slice approval
- complex FSM phases
- backlog machinery by default
- always-on huge rule files
- context extractor as the primary planner
- issue tracker flow unless the project truly needs it
- broad multi-slice plans inside `NEXT_SESSION.md`

## Good default prompt

For a new implementation session:

```text
/start-slice
```

If slash skills are unavailable:

```text
Read docs/ai/PROJECT_MAP.md and docs/ai/NEXT_SESSION.md.
Follow NEXT_SESSION.md strictly.
Implement only this slice.
Do not edit outside Allowed edit.
Run the listed verify command before saying done.
If you need more scope, stop and ask.
```

## Definition of healthy use

This workflow is healthy when:

- each session has one clear slice
- diffs stay small
- the owner understands the code that lands
- the project map gets clearer over time
- docs stay short
- no process document becomes the real project

If the workflow feels heavy, remove process before adding automation.
