---
name: plan-model-split
description: Assigns a model (Haiku, Sonnet, Opus) and an effort level (low, medium, high) to every step of an implementation plan, and then implements the plan by delegating each step to a subagent running on its assigned model and effort. Use this skill whenever you write or present an implementation plan — in plan mode, before calling ExitPlanMode, when the user asks for a plan, roadmap or step list for a code change — and whenever you start implementing a plan that has numbered steps, even if the user does not mention models, effort or delegation.
---

# Per-step model split

A plan is not only a list of steps: it is also a budget. Running every step on the
strongest model wastes time and money on mechanical edits; running everything on a
small model risks the delicate parts. So every step of a plan gets the cheapest model
and the lowest effort that will still do it well, chosen step by step, and the
implementation then actually honours that choice.

## 1. Writing the plan

End every numbered step with a model/effort tag:

`— Model: Sonnet · Effort: medium`

Write the tag in the same language as the rest of the plan (for a plan in Spanish:
`— Modelo: Sonnet · Esfuerzo: medium`). Add a short reason after the tag when the
choice isn't obvious, e.g. `— Model: Opus · Effort: high (touches the cache's
concurrency)`.

At the top of the plan, add a one-line summary of the split (`Reparto:` in Spanish):

`Split: 1 Opus·high, 2–3 Sonnet·medium, 4 Haiku·low`

### Choosing the model

Choose per step, not per plan. A typical plan mixes models: an Opus step for the
tricky core logic, a Sonnet step for the UI that uses it, a Haiku step for strings or
docs. If every step ends up on the same model, look again — it usually means the steps
are too coarse or the choice was made per plan.

- **Haiku** — mechanical, small, well-defined changes where there is one obvious way to
  do it: strings, renames, docs, a single obvious edit, updating a version number.
- **Sonnet** — typical features and fixes touching a few files, with clear patterns in
  the codebase to follow.
- **Opus** — design-heavy or risky work: architecture, concurrency, low-level internals,
  memory management, signal processing / numeric code, security-sensitive code, tricky
  bugs whose cause is not yet understood. Also anything the project's CLAUDE.md or docs
  flag as delicate.

When torn between two models, ask what a mistake in this step would cost. Cheap to
spot and fix → the smaller model. Silent, subtle or hard to undo → the bigger one.

### Choosing the effort

`low`, `medium` or `high` — never above `high`. Match it to how much reasoning and
verification the step needs, independently of the model: an Opus step can be
`medium` (well understood but delicate), and a Sonnet step can be `high` (ordinary code
but many edge cases to check).

### The verification step

The final verification step (running the project's build/test command, reviewing the
diffs) always belongs to the session model. Tag it as such, e.g. `— Model: session`
(`— Modelo: sesión` in Spanish), because the session is the one accountable for the
whole result.

## 2. Implementing the plan

The tags are a commitment, not decoration. Each step runs on the model and effort the
plan assigned to it:

- If the step's model **and** effort both match the current session, do it directly.
- Otherwise, delegate it to a subagent with the Agent tool, passing `model`
  (`"haiku"`, `"sonnet"` or `"opus"`) and `effort` (`"low"`, `"medium"` or `"high"`)
  exactly as the plan says. This includes Opus steps: when the session runs on
  Sonnet/Haiku, or on Opus with a different effort, send the Opus step to an Opus
  subagent instead of doing it on the wrong model or effort.
- One subagent per step, or per group of consecutive steps with the same model and
  effort. Steps that touch different files can run in parallel; steps that depend on
  each other's output run in order.

### The subagent brief

The subagent starts with no context, so the brief must stand on its own:

- the step's goal and what "done" looks like;
- the files involved (paths), and any interfaces or decisions from earlier steps it
  must build on;
- the project rules that apply, taken from CLAUDE.md and the project's docs: language
  of UI text/comments/commits, layout or style conventions, architecture and
  testability patterns, anything the project says to read before changing a feature;
- what not to do: don't commit, don't push, and don't run session-teardown or cleanup
  scripts (anything that stops daemons, disconnects devices, etc.). Those belong to the
  session.

### Afterwards

Review every subagent's diff yourself, then run the project's build/test command (as
given in CLAUDE.md) as the final verification step. A subagent's report is not proof —
the diff and the build are.

If delegating a step isn't practical (the Agent tool isn't available, the step is too
entangled with the current context to brief, etc.), say so before starting that step
instead of silently doing it on another model or effort.
