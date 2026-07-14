# psw_skills

A curated skill set for AI coding agents that centers on the **owner-driven slice workflow**: the human owns scope and review, the AI implements one small slice at a time, and short durable docs hold the memory.

This repo is meant to be shared. Multiple AI agents can read it, use the skills, and keep improving them over time.

## Who this is for

- **Solo developers** who pair with an AI agent and want small, reviewable diffs.
- **AI agents** that need ready-made skills (start a slice, draft the next packet, review a diff, debug, do TDD).
- **Other AI agents that will improve this repo.** See [How agents should improve this repo](#how-agents-should-improve-this-repo).

## What is inside

```text
psw_skills/
├── README.md               # this file
├── OVERVIEW.md             # deeper tour of every skill and how they connect
├── WORKFLOW.md             # full owner-driven slice workflow spec
├── NEXT_SESSION_GUIDE.md   # how to write NEXT_SESSION.md (the slice packet)
└── skills/                 # one folder per skill, each with a SKILL.md
```

Each folder in `skills/` is a self-contained skill. A skill is a Markdown file (`SKILL.md`) with YAML front matter (`name`, `description`, optional flags) plus any helper docs the skill references.

## Core idea

```text
Human plans.
AI implements.
Human verifies.
Docs keep only durable context.
```

The AI does not own the process. The owner defines each slice, the AI implements just that slice, and the owner reviews. See `WORKFLOW.md` for the full spec.

## The four project docs

Projects that use this workflow keep four short docs, usually under `docs/ai/`:

| File | Role |
|------|------|
| `PROJECT_MAP.md` | Stable project structure map |
| `NEXT_SESSION.md` | The current slice contract (one at a time) |
| `WORKLOG.md` | Append-only receipts for completed slices |
| `DECISIONS.md` | Rare durable decisions |

## The skills

| Skill | Use it to |
|-------|-----------|
| `init-slice-workflow` | Create `docs/ai/` scaffolding in a new project |
| `start-slice` | Start a fresh session from `PROJECT_MAP.md` + `NEXT_SESSION.md` |
| `next-session` | Draft the next slice packet |
| `slice-cycle` | Follow the full loop (training wheel) |
| `grilling` | Sharpen a fuzzy plan or slice |
| `grill-with-docs` | Sharpen a plan and produce ADR / glossary docs |
| `tdd` | Red-green-refactor discipline |
| `diagnosing-bugs` | Build a tight feedback loop before fixing a bug |
| `code-review` | Review a diff on Standards and Spec axes |
| `domain-modeling` | Pin down terminology and rare ADRs |
| `codebase-design` | Vocabulary for deep modules, seams, locality |
| `prototype` | Answer a UI or state-model question with throwaway code |
| `handoff` | Preserve context across sessions or branches |

See `OVERVIEW.md` for a fuller description of each.

## Quick start

### For a human owner

1. In your project, run the `init-slice-workflow` skill to create `docs/ai/` scaffolding.
2. Draft the first slice with the `next-session` skill.
3. Start a fresh AI session and run `start-slice`.
4. Review the result, then repeat.

### For an AI agent starting an implementation session

```text
Read docs/ai/PROJECT_MAP.md and docs/ai/NEXT_SESSION.md.
Follow NEXT_SESSION.md strictly.
Implement only this slice.
Do not edit outside Allowed edit.
Run the listed verify command before saying done.
If you need more scope, stop and ask.
```

## Installing the skills

Point your agent's skill directory at `skills/`, or copy the folders you want into your agent's skill path. Each `skills/<name>/SKILL.md` is loadable on its own. Helper files referenced by a skill resolve relative to that skill's folder.

## How agents should improve this repo

This repo is intended to evolve. If you are an AI agent with write access:

1. **Keep skills small and single-purpose.** One skill = one clear job.
2. **Preserve the front matter contract.** Every `SKILL.md` needs `name` and `description`. Keep the `description` written so an agent can decide when to trigger it.
3. **Do not rebuild the old forced workflow.** See the Anti-goals section in `WORKFLOW.md`. Favor removing process over adding automation.
4. **Update docs when you change behavior.** If a skill's steps change, update its `SKILL.md` and, if the change is cross-cutting, `OVERVIEW.md` and `WORKFLOW.md`.
5. **Make small, reviewable commits.** One skill change or one doc change per commit where possible. Write a clear commit message.
6. **Test the trigger wording.** After editing a `description`, sanity-check that it would fire for the intended request and not for unrelated ones.
7. **Leave a receipt.** When you complete a meaningful change, note what and why in the commit message.

## License

Add a license file before publishing if you want to set reuse terms.
