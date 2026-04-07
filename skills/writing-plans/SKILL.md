---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write implementation plans as bite-sized tasks with contracts and acceptance criteria. Document which files to touch, what interfaces to implement, and how to verify success — but leave the actual code to the executor and TDD skill. DRY. YAGNI. TDD. Frequent commits.

Assume the executor is a skilled developer who will use `superpowers:test-driven-development` for implementation. Give them clear contracts and acceptance criteria, not code to copy-paste.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** This should be run in a dedicated worktree (created by brainstorming skill).

**Save plans to:** `docs/superpowers/plans/YYYY-MM-DD-<feature-name>/` (directory)
- `overview.md` — header, goal, architecture, tech stack, task index
- `task-NN.md` — one file per task with contract and acceptance criteria

## Scope Check

If the spec covers multiple independent subsystems, it should have been broken into sub-project specs during brainstorming. If it wasn't, suggest breaking this into separate plans — one per subsystem. Each plan should produce working, testable software on its own.

## File Structure

Before defining tasks, map out which files will be created or modified and what each one is responsible for. This is where decomposition decisions get locked in.

- Design units with clear boundaries and well-defined interfaces. Each file should have one clear responsibility.
- You reason best about code you can hold in context at once, and your edits are more reliable when files are focused. Prefer smaller, focused files over large ones that do too much.
- Files that change together should live together. Split by responsibility, not by technical layer.
- In existing codebases, follow established patterns. If the codebase uses large files, don't unilaterally restructure - but if a file you're modifying has grown unwieldy, including a split in the plan is reasonable.

This structure informs the task decomposition. Each task should produce self-contained changes that make sense independently.

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

## Plan Document Header

**Every plan overview.md MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

## Tasks

- [ ] [Task 1: Component Name](task-01.md)
- [ ] [Task 2: Component Name](task-02.md)
- ...

---
```

## Task Structure

Each task is a separate file (`task-NN.md`) containing contracts and acceptance criteria — not implementation code. The executor uses `superpowers:test-driven-development` for the RED-GREEN-REFACTOR cycle.


````markdown
### Task N: [Component Name]

**Why:** [What this task accomplishes and how it fits the architecture]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Contract:**
- `function_name(param: Type) -> ReturnType` — [what it does]
- Interface/type shapes (signatures, not bodies)

**Acceptance Criteria:**
- [ ] [Observable behavior that proves it works]
- [ ] [Edge case or error condition handled]
- [ ] [Integration point verified]

**Constraints:**
- [Dependencies on other tasks or external systems]
- [Performance or compatibility requirements]

**Test guidance:** Use superpowers:test-driven-development. Feature/bugfix tasks MUST follow RED-GREEN-REFACTOR.
```

**What NOT to include in tasks:**
- Function bodies or implementation code (the executor writes this via TDD)
- Complete test files (the executor writes tests first per TDD skill)
- Shell commands for running tests (the executor knows their test runner)
- TDD step sequences (the TDD skill already enforces this)
- Commit messages (the executor crafts these from context)

## Remember
- Exact file paths always
- Contracts and acceptance criteria, not implementation code
- Reference relevant skills with @ syntax
- DRY, YAGNI, TDD, frequent commits

## Plan Review Loop

After writing the complete plan:

1. Dispatch a single plan-document-reviewer subagent (see plan-document-reviewer-prompt.md) with precisely crafted review context — never your session history. This keeps the reviewer focused on the plan, not your thought process.
   - Provide: path to the plan document, path to spec document
   - Skim each section/requirement in the spec. Can you point to a task that implements it? List any gaps.
2. If ❌ Issues Found: fix the issues, re-dispatch reviewer for the whole plan. If you find a spec requirement with no task, add the task.
3. If ✅ Approved: proceed to execution handoff

**Review loop guidance:**
- Same agent that wrote the plan fixes it (preserves context)
- If loop exceeds 2 iterations, surface to human for guidance — a third automated pass rarely resolves what two couldn't
- Reviewers are advisory — explain disagreements if you believe feedback is incorrect


## Execution Handoff

After saving the plan, offer execution choice:

**"Plan complete and saved to `docs/superpowers/plans/<feature-name>/`. Two execution options:**

**1. Subagent-Driven (recommended)** - I dispatch a fresh subagent per task, review between tasks, fast iteration

**2. Inline Execution** - Execute tasks in this session using executing-plans, batch execution with checkpoints

**Which approach?"**

**If Subagent-Driven chosen:**
- **REQUIRED SUB-SKILL:** Use superpowers:subagent-driven-development
- Fresh subagent per task + two-stage review

**If Inline Execution chosen:**
- **REQUIRED SUB-SKILL:** Use superpowers:executing-plans
- Batch execution with checkpoints for review
