---
name: slice-cycle
description: "Guide the solo owner-driven slice cycle: clarify, draft NEXT_SESSION.md, start a fresh session, implement, verify, record WORKLOG, and prepare the next slice."
disable-model-invocation: true
---

# Slice Cycle

Use this as the lightweight operating loop for solo AI pair programming.

This is a checklist, not a state machine. The user owns scope and approval. The agent may draft, implement, and summarize, but must not choose final scope silently.

## The cycle

1. Clarify the slice.
2. Draft `docs/ai/NEXT_SESSION.md`.
3. Wait for user approval of scope.
4. In a fresh session, implement from `NEXT_SESSION.md` only.
5. User verifies manually when needed.
6. Draft `WORKLOG.md` receipt and the next `NEXT_SESSION.md`.
7. Wait for user approval, then update docs.

## Mode A — Plan next slice

Use when the user has an idea but no precise slice yet.

Process:

1. Run the grilling discipline: ask one question at a time, with a recommended answer.
2. Clarify goal, non-goals, allowed read, allowed edit, acceptance criteria, verify command, and stop conditions.
3. Use the `next-session` skill to draft `docs/ai/NEXT_SESSION.md`.
4. Show the draft. Do not save until the user approves.

Good user prompt:

```text
/slice-cycle plan
I want the next slice to be <idea>.
```

## Mode B — Start implementation

Use at the start of a fresh session.

Process:

1. Run the `start-slice` skill.
2. Read `docs/ai/PROJECT_MAP.md` and `docs/ai/NEXT_SESSION.md`.
3. Summarize the slice before editing.
4. Implement only the listed slice.
5. Run the listed verify command before done.

Good user prompt:

```text
/start-slice
```

## Mode C — Close current slice

Use after implementation and user/manual checks.

Process:

1. Summarize changed files and behavior.
2. Draft a short `WORKLOG.md` receipt.
3. Ask whether durable structure should update `PROJECT_MAP.md`.
4. Ask whether any hard-to-reverse decision should update `DECISIONS.md`.
5. Draft the next `NEXT_SESSION.md` if the user gives the next slice.
6. Show all doc changes first. Save only after approval.

Good user prompt:

```text
/slice-cycle close
Manual check passed. Next slice is <next idea>.
```

## Authority rules

The user owns:

- final scope
- allowed read/edit
- non-goals
- manual verification
- whether docs are updated
- commit timing

The agent owns:

- asking clarifying questions
- drafting packets
- implementation inside scope
- running verify
- reporting facts

## Do not

- Turn this into a multi-phase FSM.
- Create backlog machinery unless the user asks.
- Auto-save `NEXT_SESSION.md` without approval.
- Add future slices to `NEXT_SESSION.md` beyond one short suggestion.
- Treat `NEXT_SESSION.md` as long-term memory.
