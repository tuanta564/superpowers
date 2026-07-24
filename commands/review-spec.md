# Review Specification

You are reviewing a software spec for implementation readiness.

## Review Mode

Default: **full review** using every review dimension below.

Optional fast mode: if the arguments include `--fast`, `fast`, or clearly ask for a quick/light review, run a **fast review** instead.

Fast review rules:
- Focus on the highest-risk issues only.
- Prioritize: scope, ambiguity, contract/data gaps, rollout/cutover risk, technical feasibility, and overengineering.
- Keep the same overall verdict structure, but keep each section brief.
- List missing edge cases only when they are likely to cause wrong behavior, rollout problems, or major rework.
- Do not do a full exhaustive sweep unless the user asks for it.

## Instructions

1. First, locate and read:
   - The **spec file** (ask the user for the path if not provided)
   - Any **referenced docs/specs** that materially affect the behavior
   - If feasibility depends on existing code, inspect the relevant code paths and current patterns before judging the spec

2. Determine the output file path:
   - Use the **same directory as the spec file**
   - Name format: `<spec-name>-review-YYYYMMDD-HHmm.md`
   - Example: if spec is `docs/auth-spec.md` → save to `docs/auth-spec-review-20250113-1430.md`
   - Get the current timestamp at the time of running (use shell: `date +%Y%m%d-%H%M`)

3. Review the spec critically. Do **not** start with a summary. First identify gaps, ambiguities, and risks.

---

## Review Dimensions

### 1. Problem Framing
- Is the problem clearly defined?
- Is the target user / workflow clear?
- Is the business or product value explained?
- Are assumptions explicit?

### 2. Scope Definition
- What is clearly in scope?
- What is clearly out of scope?
- Is there any hidden scope creep?
- Are MVP and future ideas mixed together?

### 3. Requirement Quality
- Are requirements specific, testable, and unambiguous?
- Flag vague language like: "support", "handle", "improve", "optimize", "properly", "seamless", "flexible", "robust"
- Would two engineers implement the same behavior from this text?

### 4. Functional Completeness
- Are the main use cases covered?
- Are edge cases covered?
- Are failure paths covered?
- Are empty / null / partial-data cases defined?
- Are auth / permission / tenancy concerns addressed if relevant?

### 5. Data, Contracts, and Compatibility
- Are inputs, outputs, schemas, field names, enums, and nullability defined clearly?
- Are API / DB / event contract changes explicit?
- Are backward-compatibility expectations stated?
- Are naming inconsistencies or missing shape details present?

### 6. Logic and Internal Consistency
- Are there contradictions between sections?
- Are any steps missing in the flow from trigger → processing → output?
- Are there undefined terms, states, or dependencies?
- Are there places where implementation teams would make different interpretations?

### 7. Technical Feasibility and Architecture Fit
- Does this fit the current architecture and module boundaries?
- Does it rely on data, joins, services, or behaviors that may not exist?
- Are there likely performance, migration, concurrency, ordering, or caching risks?
- Does it introduce unnecessary new abstractions or duplication?

### 8. Simplification / YAGNI
- Is the design more complex than the problem requires?
- Are there abstractions, phases, flags, registries, or extension points that are not yet justified?
- Can the same outcome be achieved with fewer moving parts?
- Is any future-proofing premature for the current scope?
- Are there opportunities to reuse an existing pattern instead of introducing a new one?

### 9. Non-Functional Requirements
- What is missing around performance?
- What is missing around reliability and observability?
- What is missing around security and rollout safety?
- What is missing around maintainability and operational support?
- If the spec relies on deployment ordering, backfills, migrations, or staged rollout, does it define an observable readiness gate rather than only a sequence of steps?

### 10. Error Handling and Unsupported Cases
- What should happen on invalid input?
- What should happen on partial failure or downstream dependency failure?
- Should the system error, retry, return blank, return partial results, or degrade gracefully?
- Are unsupported cases called out explicitly enough to avoid silent wrong behavior?

### 11. Verification and Testing
- Are acceptance criteria concrete and testable?
- What unit / integration / end-to-end tests are implied?
- What verification steps are missing?
- Is the spec precise enough to validate without guessing?

### 12. Risks and Open Questions
- What assumptions could turn out to be wrong?
- What decisions must be resolved before implementation?
- What can be deferred safely versus what blocks implementation?
- If rollout depends on worker/API deploy order, backfill completion, or migration state, is there a concrete no-guessing cutover gate?

---

## Output Format

Respond with the following structure:

### Verdict
Choose one:
- ✅ **Ready**
- ✅ **Ready with minor clarifications**
- ⚠️ **Needs revision**
- ❌ **Not implementation-ready**

### Major Gaps
List only high-impact issues.

### Ambiguities
For each important ambiguity:
- Quote the ambiguous statement
- Explain why it is ambiguous
- State what should be specified

### Missing Edge Cases
Bullet list.

### Contract / Data Issues
Bullet list.

### Technical / Architecture Risks
Bullet list.

### Suggested Revisions
For each important issue:
- **Issue:**
- **Why it matters:**
- **Suggested rewrite:**

### Open Questions for the Author
Numbered list.

### Implementation-Readiness Score
Give a score from 1-10 and explain briefly.

### Short Summary
One short paragraph only after the detailed review.

---

## Review Standard

Be strict. Prefer catching unclear assumptions now over being polite.
If something is fine, say so briefly and move on.
Do not invent new product requirements unless they are necessary to make the spec implementable; when you must assume something, label it as an assumption or open question.
Flag complexity only when a simpler approach would preserve correctness and materially reduce implementation or maintenance cost.

---

## Save the Review

After completing the review, write the full output to the file path determined in step 2.

Confirm to the user:
> "Review saved to `<output-path>`"

---

## After the Review

Ask the user:
> "Would you like me to rewrite the spec incorporating all critical fixes, or turn the open questions into an author checklist?"
