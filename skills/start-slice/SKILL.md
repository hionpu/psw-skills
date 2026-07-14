---
name: start-slice
description: Start a fresh solo-dev slice session from docs/ai/PROJECT_MAP.md and docs/ai/NEXT_SESSION.md.
disable-model-invocation: true
---

# Start Slice

Start this session from the project's current slice packet.

## Required reads

Read, in order:

1. `docs/ai/PROJECT_MAP.md`
2. `docs/ai/NEXT_SESSION.md`

If either file is missing, stop and ask the user to create it. Do not infer the slice from chat history.

## Rules

Follow `NEXT_SESSION.md` strictly.

- Implement only the slice in `NEXT_SESSION.md`.
- Do not broaden scope.
- Do not edit outside `Allowed edit`.
- Do not read outside `Allowed read` unless needed; ask first unless the user has allowed more.
- Run the listed verify command before saying done.
- Do not hand back with a note like "run verify" if the verify command is available; run it yourself.
- If you need more scope, stop and ask.
- If verification fails twice for the same reason, stop and ask.

## Scope confirmation

After reading the files, confirm the scope for yourself before editing.

If your environment supports progress messages, you may show this short summary:

```text
Slice: <id>
Goal: <one sentence>
Allowed edit: <paths>
Verify: <commands>
Stop conditions understood: yes
```

Do **not** stop after this summary. Continue immediately to inspect the allowed files, implement the slice, and run verify in the same session. Only stop if a stop condition is hit.

## Done response

Only send the final done response after implementation and verify are complete.

Do not end with interim status such as:

- "Run verify."
- "Ready to test."
- "Next, run the build."

If the verify command is available, run it yourself with the shell tool. If it is unavailable or needs a human-only environment, say exactly why.

When finished, report:

1. Files changed
2. Behavior implemented
3. Verification run and result
4. Durable structure learned, if any
5. Suggested next slice, if any
