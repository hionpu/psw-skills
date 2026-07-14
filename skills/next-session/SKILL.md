---
name: next-session
description: Draft or update the next AI session launch packet for solo owner-driven slice work. Use when the user asks for a NEXT_SESSION.md draft, next slice packet, or end-of-slice handoff.
disable-model-invocation: true
---

# Next Session Packet

Draft the next session's `NEXT_SESSION.md` for a solo developer.

This skill does **not** choose scope on its own. The user owns scope. You may propose a draft, but the user must approve the final content before you overwrite `docs/ai/NEXT_SESSION.md`.

## Purpose

`NEXT_SESSION.md` is the launch packet for one fresh AI session. It also acts as the current slice contract.

It is not a diary, backlog, full plan, or conversation summary.

Use long-lived docs for memory:

- `PROJECT_MAP.md` — stable project map
- `DECISIONS.md` — rare durable decisions
- `WORKLOG.md` — append-only receipts for completed slices
- `NEXT_SESSION.md` — the next slice contract only

## Process

### 1. Read only relevant context

Prefer these files if they exist:

- `docs/ai/PROJECT_MAP.md`
- `docs/ai/DECISIONS.md`
- `docs/ai/WORKLOG.md`
- existing `docs/ai/NEXT_SESSION.md`

Do not rediscover the whole repo. If the next slice needs more facts, ask or inspect only the likely files.

### 2. Separate receipt from next packet

If the user asks to close a cycle, prepare two outputs:

1. A short `WORKLOG.md` receipt for the completed slice.
2. A new `NEXT_SESSION.md` draft for the next slice.

Keep them separate. Do not put historical receipt text into `NEXT_SESSION.md` except for one short "Last completed slice" line.

### 3. Draft first, do not overwrite first

Default behavior:

- Show the draft in chat.
- Ask the user to confirm or edit scope.
- Only write `docs/ai/NEXT_SESSION.md` after explicit approval.

If the user explicitly says to save it, you may overwrite `docs/ai/NEXT_SESSION.md`.

### 4. Protect owner control

Never silently broaden:

- `Allowed read`
- `Allowed edit`
- `Goal`
- `Non-goals`
- `Verify`

If unsure, leave a placeholder or ask. The user is the planner and reviewer. The AI is the drafter and implementer.

## Template

```md
# Next Session Packet

## 0. Mode

You are implementing one small slice.
Do not broaden scope.
Do not refactor unless listed.
Ask before reading or editing outside the listed files.

## 1. Project pointers

Read first:

- docs/ai/PROJECT_MAP.md

Read only if relevant:

- docs/ai/DECISIONS.md
- docs/ai/WORKLOG.md

Do not rediscover the whole repo.

## 2. Current state

Branch:

- `<branch-name>`

Last completed slice:

- `<slice id/title>`

Current known status:

- Tests passing: yes/no/unknown
- Typecheck passing: yes/no/unknown
- Working tree expected clean: yes/no

Important recent changes:

- `<short bullet>`

## 3. This slice

ID:

- `<slice-id>`

Goal:

- `<one sentence user-visible behavior>`

Non-goals:

- `<explicitly out of scope>`

Allowed read:

- `path/a`
- `path/b`

Allowed edit:

- `path/a`
- `path/c`

Likely entry point:

- `path/a`, symbol/function/class `<name>`

## 4. Acceptance criteria

- [ ] `<observable behavior>`
- [ ] `<edge case>`

## 5. Test / verify

Use this feedback loop first if possible:

```bash
<single narrow test command>
```

Before done, run:

```bash
<typecheck/build/test command>
```

## 6. Design constraints

- `<constraint>`
- `<naming convention>`
- `Do not introduce a new abstraction unless this slice needs it.`

## 7. Map updates expected

This slice may clarify:

- `<area of project structure>`

If you learn durable structure, mention it in the final response. The human will decide whether to update `PROJECT_MAP.md`.

## 8. Known unknowns

- `<unknown>`

## 9. Stop conditions

Stop and ask if:

- You need to edit outside `Allowed edit`
- The listed files are not enough
- The test seam is unclear
- The fix wants a larger refactor
- Verification fails twice for the same reason

## 10. Done response format

When finished, report:

1. Files changed
2. Behavior implemented
3. Verification run and result
4. Durable structure learned, if any
5. Suggested next slice, if any
```

## Start prompt for the next session

```text
Read docs/ai/PROJECT_MAP.md and docs/ai/NEXT_SESSION.md.
Follow NEXT_SESSION.md strictly.
Implement only this slice.
Do not edit outside Allowed edit.
Run the listed verify command before saying done.
If you need more scope, stop and ask.
```

If the `start-slice` skill is available, the user can use this shorter prompt in a fresh session:

```text
/start-slice
```

## Final response after saving

When you save `WORKLOG.md` and `NEXT_SESSION.md`, make the session boundary explicit.

State that the current cycle is closed, and that the next slice should start in a fresh AI session. If the `start-slice` skill is available, mention `/start-slice` as the command to run in that fresh session.

## Quality bar

A good packet is short, concrete, and restrictive.

Prefer:

- exact file paths
- one user-visible goal
- explicit non-goals
- one narrow feedback loop
- stop conditions

Avoid:

- long history
- full design essays
- broad repo scans
- speculative future slices
- vague goals like "clean up code"
