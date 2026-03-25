# Shared Learnings

Cross-project systematic patterns discovered through AutoBuild. These apply universally regardless of tech stack or project.

This file is loaded by the orchestrator (`program.md`) alongside project-specific learnings. Update this file when you discover a pattern that would benefit all projects, not just the current one.

---

<!-- Append entries below. Newest first. -->

## Initial Patterns

### Always Validate Environment Variables at Startup
- **Context:** Applies to any project using environment variables for config
- **Pattern:** Validate all required environment variables exist at application startup, not at first use. Fail fast with a clear error message.
- **Why:** Catching missing config at startup prevents confusing runtime errors hours into execution.

### One Logical Change Per Commit
- **Context:** All build modes
- **Pattern:** Each commit should contain exactly one logical change. If a feature requires database migration + code + tests, these are one commit (they're one logical change). But two unrelated fixes should be two commits.
- **Why:** Makes git history useful for debugging, reversion, and code review.

### Test What, Not How
- **Context:** Test writing across all stacks
- **Pattern:** Tests should verify observable behavior (inputs → outputs, side effects), not internal implementation details (what functions were called, in what order).
- **Why:** Implementation-coupled tests break on every refactor, providing false negatives and discouraging improvement.
