# NEXT_SESSION.md Guide

`NEXT_SESSION.md` is the single launch packet for the next AI session.

It replaces a separate `slice.md` in the default solo-dev workflow. Keep one copy per project, usually at `docs/ai/NEXT_SESSION.md`. Overwrite it before each new session.

## Role

Use long-lived docs for memory:

- `PROJECT_MAP.md` — stable project map
- `DECISIONS.md` — rare durable decisions
- `WORKLOG.md` — append-only receipts for completed slices
- `NEXT_SESSION.md` — the next slice contract only

Do not use `NEXT_SESSION.md` as a diary, backlog, or full conversation summary.

## Cycle

1. Finish the current slice.
2. Append a short receipt to `WORKLOG.md`.
3. Update `PROJECT_MAP.md` if you learned durable structure.
4. Update `DECISIONS.md` only for hard-to-reverse decisions.
5. Overwrite `NEXT_SESSION.md` with the next slice.
6. Start a fresh AI session and tell it to read `PROJECT_MAP.md` and `NEXT_SESSION.md`.

## Start prompt

```text
Read docs/ai/PROJECT_MAP.md and docs/ai/NEXT_SESSION.md.
Follow NEXT_SESSION.md strictly.
Implement only this slice.
Do not edit outside Allowed edit.
Run the listed verify command before saying done.
If you need more scope, stop and ask.
```

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

## Rule of thumb

If information must survive many future sessions, put it in `PROJECT_MAP.md`, `DECISIONS.md`, or `WORKLOG.md`.

If information is only needed to launch the next session, put it in `NEXT_SESSION.md`.
