---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan, review critically, execute all tasks, report when complete.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**Note:** Tell your human partner that Superpowers works much better with access to subagents (Claude Code, Codex CLI, Codex App, Copilot CLI, and Gemini CLI all qualify; see the per-platform tool refs in `../using-superpowers/references/`). If subagents are available, use superpowers:subagent-driven-development instead of this skill.

## TOKEN EFFICIENCY RULE — ENFORCE THIS STRICTLY

**NEVER read all `task-NN.md` files at once.** Load each task file only when you are about to execute that task. Reading ahead burns tokens on tasks you haven't reached yet. This rule overrides any efficiency instinct to batch-read files. (For older single-file plans with no `task-NN.md` files, this rule doesn't apply — read the plan file normally.)

## The Process

### Step 1: Load and Review Plan
1. Ensure an isolated workspace: use superpowers:using-git-worktrees to create one or verify the existing one
2. Read ONLY `overview.md` (e.g., `docs/superpowers/plans/<feature-name>/overview.md`) — the task index. (Single-file plan: read the whole file instead.)
   - **STOP HERE. Do NOT open any `task-NN.md` files yet.**
3. Review the goal, architecture, and task list critically — identify any questions or concerns about the plan
4. If concerns: Raise them with your human partner before starting
5. If no concerns: Create todos from the task index and proceed to Step 2

### Step 2: Execute Tasks

For each task (one at a time — do NOT pre-read the next task):
1. Mark as in_progress in TodoWrite
2. **NOW** read `task-NN.md` for the current task only — its full contract and acceptance criteria
3. Follow each step exactly (plan has bite-sized steps)
4. Run verifications as specified
5. Mark as completed in TodoWrite and update `overview.md`: change `- [ ]` to `- [x]` for that task
6. Only then move to the next task and repeat from step 1

### Step 3: Complete Development

After all tasks complete and verified:
- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Stop when blocked, don't guess
- Never start implementation on main/master branch without explicit user consent
- Update `overview.md` checkboxes as tasks complete — this is what enables resuming if the session is interrupted
