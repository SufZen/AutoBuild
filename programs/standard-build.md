# Standard Build Program

> Single-pass build with quality gates. Use for simple, well-understood tasks.

## When to Use

- Complexity is SIMPLE or MEDIUM
- There is one clear, obvious implementation approach
- The acceptance criteria are specific and verifiable

## Workflow

### Step 1: Implement

Invoke the **Builder Agent** (`agents/builder.md`):
- Implement the plan step by step
- Follow `project-context.md` conventions
- Commit after each logical step
- Message format: `autobuild: [intent-name] — [step description]`

### Step 2: Quality Gate

Invoke the **Quality Scorer** (`agents/quality-scorer.md`):
- Run the full quality pipeline
- Score must meet the threshold (default: ≥ 60)

**If score < threshold:**
- Review the failing checks
- Have the Builder Agent fix the issues
- Re-run the quality pipeline
- Repeat up to 3 times
- If still failing after 3 attempts → escalate to human

### Step 3: Sub-Agent Reviews

Run all sub-agent reviews (Phase 4 of `program.md`):
- Security Reviewer → any CRITICAL blocks delivery
- Architecture Reviewer → any violation blocks delivery
- Test Writer → add missing tests

### Step 4: Final Score

Invoke **Quality Scorer** one final time (post-reviews, post-test-writing):
- This is the definitive score
- Log to `results/results.tsv`

### Step 5: Present

Generate `evaluation/latest-review.md` and present to human.

## Completion

A standard build is complete when:
- All acceptance criteria are met
- Quality Score ≥ threshold
- No CRITICAL security findings
- No architecture violations
- Human has reviewed and approved
