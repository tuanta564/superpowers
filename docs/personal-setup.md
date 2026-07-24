# Personal Setup Notes (tuanta564/superpowers)

This is a personal fork of [obra/superpowers](https://github.com/obra/superpowers). These notes
are for re-installing my own Claude Code setup on a new machine — not upstream documentation.

## Branch layout

- `main` — kept in sync with upstream (`obra/superpowers`), no personal changes.
- `feature/customize` — personal customizations on top of `main`:
  - `writing-plans` / `executing-plans`: plans are split into `overview.md` + one
    `task-NN.md` per task (contract + acceptance criteria, no inline code — the
    executor writes code via `superpowers:test-driven-development`). Tasks load
    lazily, one at a time, during execution.
  - `subagent-driven-development`: kept as upstream, plus a small compatibility
    patch so `task-brief` / `sdd-workspace` / `review-package` also accept a
    folder-based plan (not just a single plan file).
  - `brainstorming`: specs are split the same way (`overview.md` + section files),
    with review escalation (self-review for simple specs, dispatched reviewer
    subagent for complex ones).
  - `commands/review-plan.md`, `commands/review-spec.md`: local review commands.
  - `.vscode/settings.json`: editor color settings.

When upstream ships a new release, rebase or re-derive `feature/customize` on
the new `main` — see the merge notes in git history (commits on
`feature/customize` explain what was folded from upstream vs. kept custom).

## Installing skills on a new machine

Skills are managed with [`npx skills`](https://github.com/vercel-labs/skills)
(global scope, targeting Claude Code). It tracks skills by source + content
hash, not by version number — no manual version bump is needed for updates to
be detected.

**Fresh machine (nothing installed yet):**

```bash
npx skills add https://github.com/tuanta564/superpowers/tree/feature/customize/skills -g -a claude-code --all -y
```

**Machine that already has skills installed from upstream `obra/superpowers`**
(remove the ones this fork customizes first, so the add below doesn't collide):

```bash
npx skills remove writing-plans executing-plans brainstorming subagent-driven-development -g -y
npx skills add https://github.com/tuanta564/superpowers/tree/feature/customize/skills -g -a claude-code --all -y
```

**Pulling in later changes:**

```bash
npx skills update -g
```

**Once `feature/customize` is merged into this fork's `main`**, switch the
source in the commands above from `tree/feature/customize/skills` to plain
`tuanta564/superpowers` (defaults to `main`).

## Installing commands on a new machine

`npx skills` only manages skills, not Claude Code slash commands. Copy these by hand:

```bash
cp commands/review-plan.md commands/review-spec.md ~/.claude/commands/
```

## Not covered here (yet)

Only skills and commands are tracked in this repo so far. Other personal
Claude Code config (hooks, statusline, settings.json, the multi-account CCS
setup under `~/.ccs/instances/`) still lives only on this machine.
