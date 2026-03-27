# Fix Intent

> Use this template when fixing a bug or resolving an issue.

## Goal

<!-- What is broken? Be specific. e.g., "API returns 500 error when user has no profile photo" -->

## Context

<!-- How to reproduce? When did it start? Any relevant error logs? -->

### Reproduction Steps

1. <!-- Step 1 -->
2. <!-- Step 2 -->
3. <!-- Observe: [what happens] instead of [what should happen] -->

### Error Details

<!-- Paste error messages, stack traces, logs here -->

## Scope

### IN

- <!-- Fix the bug -->
- <!-- Add regression test for this bug -->

### OUT

- <!-- Don't refactor surrounding code (use refactor intent) -->
- <!-- Don't add features while fixing -->

## Acceptance Criteria

- [ ] <!-- Bug no longer reproducible with the steps above -->
- [ ] <!-- Regression test added that catches this specific bug -->
- [ ] <!-- No other tests broken by the fix -->
- [ ] <!-- Root cause documented in the commit message -->

## Assumptions

- <!-- List root cause assumptions before debugging -->

## Constraints

### Security & Privacy

- <!-- Ensure fix doesn't bypass security checks -->

### Performance

- <!-- Fix must not regress performance -->

### Dependencies

- <!-- Must not introduce new dependencies for a bug fix -->

### Backward Compatibility

- <!-- Minimal change — fix the bug, nothing more -->

## Build Mode

**Mode:** `standard`
