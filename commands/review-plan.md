# Review Implementation Plan

You are reviewing an implementation plan against its original spec.

> If the artifact is primarily a spec, design doc, migration proposal, or requirements document rather than a task-by-task implementation plan, use `/review-spec` instead.

## Review Mode

Default: **full review** using every dimension below.

Optional fast mode: if the arguments include `--fast`, `fast`, or clearly ask for a quick/light review, run a **fast review** instead.

Fast review rules:
- Focus on the highest-risk plan issues only.
- Prioritize: missing spec coverage, blocker/order mistakes, unclear tasks, rollout/cutover risk, and obvious over-planning.
- Keep the same overall verdict structure, but keep each section brief.
- Do not do a full exhaustive sweep unless the user asks for it.

## Instructions

1. First, locate and read:
   - The **spec file** (ask the user for the path if not provided)
   - The **implementation plan** (ask the user for the path if not provided)

2. Determine the output file path:
   - Use the **same directory as the plan file**
   - Name format: `<spec-name>-implementation-review-YYYYMMDD-HHmm.md`
   - Example: if spec is `docs/auth-spec.md` → save to `docs/auth-spec-implementation-review-20250113-1430.md`
   - Get the current timestamp at the time of running (use shell: `date +%Y%m%d-%H%M`)

3. Cross-reference them and evaluate across all four dimensions below.

---

## Review Dimensions

### 1. Completeness vs Spec
- Does every requirement in the spec have a corresponding task in the plan?
- Are edge cases, error states, and validation covered?
- Are any spec requirements missing or only partially addressed?

### 2. Task Sequencing & Order
- Are dependencies between tasks correctly ordered (blockers come first)?
- Are there tasks that could run in parallel but are serialized unnecessarily?
- Are there circular or ambiguous dependencies?
- If rollout depends on deploy order, migration state, backfills, or cutover timing, does the plan include a measurable go/no-go gate and a verification point before exposure?

### 3. Technical Soundness
- Does the approach fit the current codebase, architecture, and stack?
- Are new dependencies or patterns justified and low-risk?
- Are there technically vague steps that need more detail before implementation?
- Does each task define a clear outcome and enough detail that an implementer can execute it without guessing?

### 4. Gaps, Risks & Simplification
- What assumptions in the plan could turn out to be wrong?
- What external unknowns (APIs, third-party services, unclear requirements) could block progress?
- What is underspecified and would leave a developer guessing?
- Is the plan introducing work, phases, or task splits that the spec does not require?
- Is there obvious over-planning or unnecessary serialization that increases implementation cost without reducing risk?

---

## Output Format

Respond with the following structure:

### Summary
One short paragraph on the overall quality of the plan — is it ready to implement, needs minor fixes, or needs significant revision? If fast mode is active, keep this especially concise.

### Issues

List every issue found, grouped by dimension. In fast mode, list only the highest-signal issues. For each issue:

```
- [CRITICAL | MINOR] <Short title>
  Problem: <What is wrong or missing>
  Fix: <Concrete suggestion to resolve it>
```

### Verdict
One of:
- ✅ **Ready** — plan is solid, minor issues only
- ⚠️ **Needs revision** — fix critical issues before starting
- ❌ **Replan** — fundamental gaps, revisit the plan structure

---

## Save the Review

After completing the review, write the full output to the file path determined in step 2.

Confirm to the user:
> "Review saved to `<output-path>`"

---

## After the Review

Ask the user:
> "Would you like me to rewrite the plan incorporating all critical fixes?"
