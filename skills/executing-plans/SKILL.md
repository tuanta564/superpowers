---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan, review critically, execute all tasks, report when complete.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**Note:** Tell your human partner that Superpowers works much better with access to subagents. The quality of its work will be significantly higher if run on a platform with subagent support (such as Claude Code or Codex). If subagents are available, use superpowers:subagent-driven-development instead of this skill.

## The Process

### Step 1: Load and Review Plan
1. Read `overview.md` (e.g., `docs/superpowers/plans/<feature-name>/overview.md`) to get the task index — do NOT read individual `task-NN.md` files yet
2. Review the goal, architecture, and task list critically — identify any questions or concerns
3. If concerns: Raise them with your human partner before starting
4. If no concerns: Create TodoWrite from the task index and proceed

### Step 2: Execute Tasks

For each task:
1. Mark as in_progress in TodoWrite
2. Read the task's `task-NN.md` file now to get its full contract and acceptance criteria
3. Follow each step exactly (plan has bite-sized steps)
4. Run verifications as specified
5. Mark as completed in TodoWrite and update `overview.md`: change `- [ ]` to `- [x]` for that task

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

## Integration

**Required workflow skills:**
- **superpowers:using-git-worktrees** - REQUIRED: Set up isolated workspace before starting
- **superpowers:writing-plans** - Creates the plan this skill executes
- **superpowers:finishing-a-development-branch** - Complete development after all tasks
