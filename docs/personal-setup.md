# Personal Setup Notes (tuanta564/superpowers)

This is a personal fork of [obra/superpowers](https://github.com/obra/superpowers). These notes
are for re-installing my own Claude Code setup on a new machine — not upstream documentation.

## Branch layout

- `main` — kept in sync with upstream (`obra/superpowers`), no personal changes.
- `customize` — personal customizations on top of `main` (no `feature/` prefix,
  no slash in the name — `npx skills add .../tree/<branch>/...` can't parse a
  branch name containing `/`):
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

When upstream ships a new release, rebase or re-derive `customize` on
the new `main` — see the merge notes in git history (commits on
`customize` explain what was folded from upstream vs. kept custom).

## Installing skills on a new machine

Skills are managed with [`npx skills`](https://github.com/vercel-labs/skills)
(global scope, targeting Claude Code). It tracks skills by source + content
hash, not by version number — no manual version bump is needed for updates to
be detected.

`writing-skills` is skipped below — Claude's built-in default already covers it.
Note: `-s` must be repeated once per skill — a comma- or space-separated list
in a single `-s` is not accepted and silently matches nothing.

**Fresh machine (nothing installed yet):**

```bash
npx skills add https://github.com/tuanta564/superpowers/tree/customize/skills -g -a claude-code \
  -s brainstorming -s dispatching-parallel-agents -s executing-plans -s finishing-a-development-branch \
  -s receiving-code-review -s requesting-code-review -s subagent-driven-development -s systematic-debugging \
  -s test-driven-development -s using-git-worktrees -s using-superpowers -s verification-before-completion \
  -s writing-plans -y
```

**Machine that already has skills installed from upstream `obra/superpowers`**
(remove the ones this fork customizes first, so the add below doesn't collide):

```bash
npx skills remove writing-plans executing-plans brainstorming subagent-driven-development -g -y
npx skills add https://github.com/tuanta564/superpowers/tree/customize/skills -g -a claude-code \
  -s brainstorming -s dispatching-parallel-agents -s executing-plans -s finishing-a-development-branch \
  -s receiving-code-review -s requesting-code-review -s subagent-driven-development -s systematic-debugging \
  -s test-driven-development -s using-git-worktrees -s using-superpowers -s verification-before-completion \
  -s writing-plans -y
```

Ran successfully on this machine on the `tt14` CCS instance — the CLI
auto-detected the running agent's instance directory
(`~/.ccs/instances/tt14/skills/`) rather than plain `~/.claude/skills/`. On a
machine with only one Claude Code account, it installs straight to
`~/.claude/skills/`. On a machine with multiple CCS instances, re-run once per
instance (or check where it landed with `npx skills list -g -a claude-code`).

**Pulling in later changes:**

```bash
npx skills update -g
```

**Once `customize` is merged into this fork's `main`**, switch the
source in the commands above from `tree/customize/skills` to plain
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
