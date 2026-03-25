# AutoBuild — User Guide

> Everything you need to go from **zero to your first AI-built feature** in under 10 minutes.

---

## Table of Contents

1. [What is AutoBuild?](#1-what-is-autobuild)
2. [Installation](#2-installation)
3. [Configuration](#3-configuration)
4. [Writing Your First Intent](#4-writing-your-first-intent)
5. [Running a Build](#5-running-a-build)
6. [Build Modes](#6-build-modes)
7. [Understanding Quality Scores](#7-understanding-quality-scores)
8. [Reading Results](#8-reading-results)
9. [The Multi-Agent System](#9-the-multi-agent-system)
10. [Learnings & Self-Improvement](#10-learnings--self-improvement)
11. [Tips & Best Practices](#11-tips--best-practices)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. What is AutoBuild?

AutoBuild is a **portable, markdown-based framework** that turns any AI coding agent (Gemini, Claude, Cursor, Copilot, etc.) into a structured, self-improving software builder.

You define **what** you want built. The AI follows AutoBuild's structured workflow to **plan, build, review, and iterate** — producing higher-quality code than unstructured prompting.

**Key concepts:**

| Concept | What It Means |
|---------|---------------|
| **Intent** | A description of what you want built (like a mini-spec) |
| **Build program** | The strategy the AI follows (single-pass, multi-approach, autonomous loop, or metric optimization) |
| **Quality pipeline** | Seven automated checks that score every iteration 0–100 |
| **Sub-agents** | Specialist reviewers (security, architecture, tests, quality) that inspect the AI's work |
| **Learnings** | Knowledge accumulated from past builds that makes future builds better |

**AutoBuild is not an application.** It's a set of markdown instructions. There's nothing to install, no dependencies, no runtime. You copy the files into your project and point your AI agent at them.

---

## 2. Installation

### Prerequisites

- A project with **Git** initialized
- An AI coding agent (any will work)

### Step-by-Step

**1. Create the `.autobuild/` directory** in your project root.

**2. Copy the workflow files** from the AutoBuild repo:

```
.autobuild/
├── program.md              ← from core/program.md
├── quality-pipeline.md     ← from core/quality-pipeline.md
│
├── agents/                 ← entire agents/ folder
│   ├── builder.md
│   ├── security-reviewer.md
│   ├── architecture-reviewer.md
│   ├── quality-scorer.md
│   └── test-writer.md
│
├── programs/               ← entire programs/ folder
│   ├── standard-build.md
│   ├── iterative-build.md
│   ├── overnight-build.md
│   └── optimization-loop.md
│
├── intents/                ← entire intents/ folder
│   └── _template.md
│
├── results/
│   ├── results.tsv         ← create with headers (see below)
│   └── learnings.md        ← from core/learnings-template.md
│
└── evaluation/             ← entire evaluation/ folder
    ├── review-checklist.md
    ├── security-checklist.md
    └── architecture-checklist.md
```

**3. Create `results/results.tsv`** with this header row:

```tsv
timestamp	branch	intent	approach	quality_score	tests	lint	security	types	complexity	coverage	architecture	status	description
```

**4. Configure** — see the next section.

> **Important:** In the AutoBuild repo, the source entry point is `core/program.md`. In your project, the installed entry point is `.autobuild/program.md`. They're the same file — just different locations.

---

## 3. Configuration

### 3a. Project Context

Copy `setup/project-context.template.md` → `.autobuild/project-context.md` and fill in:

- **Project name and description** — one sentence
- **Tech stack** — language, framework, database, testing, linting tools
- **Code conventions** — file structure, naming, import ordering
- **Architecture decisions** — rules the AI must respect
- **Anti-patterns** — things the AI must never do

This file is your project's "constitution." The AI reads it before every task and follows its rules.

**Example snippet:**

```markdown
## Project Overview
- **Name:** Acme API
- **Description:** REST API for the Acme e-commerce platform
- **Primary language:** Python 3.12
- **Framework:** FastAPI
- **Shared learnings source (optional):** /path/to/shared/sharedlearnings.md

## Anti-Patterns (NEVER DO)
1. Never use synchronous I/O in async contexts
2. Never catch generic Exception without re-raising
3. Never import from internal modules of third-party packages
```

### 3b. Quality Config

1. Pick a **stack preset** from `stacks/` (e.g., `stacks/python.md`)
2. Copy it → `.autobuild/quality-config.md`
3. Customize the weights if needed

Available presets: **Python**, **Node/TypeScript**, **Go**, **Rust**.

Each preset maps onto AutoBuild's **7 canonical quality checks** — you just fill in your project's specific commands:

| Check | What It Measures |
|-------|------------------|
| Tests | Do all tests pass? |
| Lint / Format | Is the code clean and formatted? |
| Type Safety | Do types check out? |
| Security | Any vulnerabilities, secrets, or audit failures? |
| Complexity | Is the code reasonably simple? |
| Coverage | How much code is tested? |
| Architecture | Does it follow your project's conventions? |

**Weights must total 100%.** Adjust them to reflect your project's priorities — a security-critical app might weight Security at 25%, while a data pipeline might weight Tests at 50%.

---

## 4. Writing Your First Intent

An **intent** is a small spec that tells the AI what to build. AutoBuild ships four intent templates:

| Template | When to Use |
|----------|-------------|
| `feature.md` | Adding new capabilities |
| `fix.md` | Fixing bugs |
| `refactor.md` | Restructuring without changing behavior |
| `optimize.md` | Improving a measurable metric |

### Quick Start

1. Copy a template — e.g., `cp .autobuild/intents/_template.md .autobuild/intents/active-intent.md`
2. Fill in the sections:

```markdown
## Goal
Add rate limiting to all public API endpoints.

## Scope
### IN
- Implement sliding-window rate limiter middleware
- Add rate limit headers to responses
- Add tests for rate limiting behavior

### OUT
- Do not change authentication flow
- Do not add Redis (use in-memory for now)

## Acceptance Criteria
- [ ] All public endpoints return 429 after 100 requests/minute
- [ ] Rate limit headers (X-RateLimit-*) present on every response
- [ ] Tests cover normal flow, limit-hit, and limit-reset scenarios

## Build Mode
**Mode:** `standard`
```

3. Save it as `.autobuild/intents/active-intent.md`

> **Only one intent is active at a time.** Complete the current one before starting the next.

---

## 5. Running a Build

1. Open your AI coding agent
2. Point it at your project
3. Tell it:

> "Read `.autobuild/program.md` and execute the AutoBuild workflow."

That's it. The agent will:

1. **Load context** — read your project rules, quality config, active intent, and any learnings
2. **Analyze** — understand the intent, identify affected files and risks
3. **Plan** — break the work into steps, present the plan for your approval
4. **Build** — implement the plan, running quality checks after each step
5. **Review** — invoke specialist sub-agents (security, architecture, tests)
6. **Evaluate** — generate a review report and present it to you
7. **Learn** — record patterns and insights for next time

### What Happens on Each Iteration

```
Code change → Run quality pipeline → Score 0-100
    ↓
  Score improved or held steady? No CRITICAL security issue?
    YES → Keep this state (git commit)
    NO  → Discard (git reset)
    ↓
  Is the score >= 80? → Ready to deliver
  Score 60-79?        → Acceptable with notes
  Score < 60?         → Not ready, keep iterating
```

---

## 6. Build Modes

### Standard (`standard`)

**Single-pass build.** Best for simple, well-understood tasks.

- Implement → quality check → sub-agent reviews → present
- Up to 3 fix-and-retry cycles if quality checks fail
- Escalates to you if still failing after 3 attempts

**Use when:** The right approach is obvious and the task is simple/medium complexity.

### Iterative (`iterative`)

**Try multiple approaches, keep the best.** Best when multiple valid implementations exist.

- Agent proposes 2–3 approaches, presents them for your selection
- Each approach gets its own Git branch (`autobuild/intent/approach-N`)
- All approaches are scored and compared side-by-side
- The winner is polished through full sub-agent review

**Use when:** You're choosing between caching strategies, algorithm choices, or architectural patterns.

### Overnight (`overnight`)

**Autonomous loop.** Set it up, go to sleep, review results in the morning.

- The AI makes one small improvement at a time
- Each change is scored; improvements are kept, regressions are discarded
- Stops after 10 consecutive discards (plateau reached)
- Generates a morning summary with all iterations logged

**Critical rules the agent follows:**
- Never asks for input — fully autonomous
- Never pushes to main — all work on a feature branch
- Always reverts failures immediately
- Always logs every iteration

**Use when:** You have a large optimization space and want the AI to explore while you're away.

### Optimize (`optimize`)

**Metric-focused hill climbing.** Given a specific measurement, systematically improve it.

- You define: what to measure, the command to measure it, direction (minimize/maximize), and a target
- The agent makes one change per iteration, measures the metric, and keeps improvements
- Stops when the target is reached or 5 consecutive iterations show no improvement

**Use when:** You want to reduce response latency, shrink bundle size, improve test coverage, etc.

---

## 7. Understanding Quality Scores

Every iteration produces a **composite quality score** from 0 to 100 based on seven weighted checks.

### The Seven Checks

| Check | Default Weight | What It Measures |
|-------|---------------|------------------|
| Tests | 35% | All tests pass (binary: full or zero) |
| Lint / Format | 15% | Code is clean and formatted |
| Type Safety | 10% | No type errors |
| Security | 15% | No vulnerabilities, no secrets, clean audit |
| Complexity | 10% | Code isn't overly complex |
| Coverage | 5% | Proportional to test coverage % |
| Architecture | 10% | Follows project conventions |

### Two Decisions Per Score

AutoBuild makes two **separate** decisions from each score:

**1. Branch Retention** — Should the AI keep this code state?

| Condition | Action |
|-----------|--------|
| Score improved or held steady AND no CRITICAL security issue | **Keep** (commit) |
| Score regressed OR CRITICAL security issue | **Discard** (reset) |

**2. Delivery Readiness** — Is this result ready for you?

| Score Range | Readiness |
|-------------|-----------|
| ≥ 80 | **Ready to deliver** |
| 60 – 79 | **Acceptable with notes** (minor issues documented) |
| < 60 | **Not ready** (more work needed) |

> These are independent decisions. A branch can be *kept* (because it improved) but *not yet ready to deliver* (because the score is still 65).

---

## 8. Reading Results

### `results/results.tsv`

Every iteration is logged as a tab-separated row:

```
timestamp  branch  intent  approach  quality_score  tests  lint  security  types  complexity  coverage  architecture  status  description
```

- **status** is either `keep` or `discard` (branch action)
- **quality_score** is 0–100
- Each individual check column records pass/fail/warnings

This gives you a complete history of every experiment the AI tried.

### `evaluation/latest-review.md`

Generated at the end of each build. Contains:
- Final quality score with per-check breakdown
- Security review findings
- Architecture compliance assessment
- Human review checklist (you check the boxes)
- Overall verdict: APPROVE / REQUEST CHANGES / NEEDS DISCUSSION

### `results/learnings.md`

Growing knowledge base — see Section 10.

---

## 9. The Multi-Agent System

AutoBuild defines **five specialist sub-agents**, each with a focused role. The orchestrator (`program.md`) activates them at the right time.

```
┌───────────────────────────────────────┐
│           ORCHESTRATOR                │
│          (program.md)                 │
├──────────┬───────────┬────────────────┤
│ Builder  │ Security  │ Architecture   │
│ Agent    │ Reviewer  │ Reviewer       │
├──────────┼───────────┼────────────────┤
│ Quality  │ Test      │                │
│ Scorer   │ Writer    │                │
└──────────┴───────────┴────────────────┘
```

| Agent | When Activated | What It Does |
|-------|---------------|--------------|
| **Builder** | Phase 3 (BUILD) | Writes production-ready code following your project conventions |
| **Security Reviewer** | Phase 4 (REVIEWS) | Scans for secrets, CVEs, missing input validation, auth gaps |
| **Architecture Reviewer** | Phase 4 (REVIEWS) | Checks compliance with your architecture rules in `project-context.md` |
| **Quality Scorer** | Phase 3 + 4 | Runs the quality pipeline and reports score, retention, and readiness |
| **Test Writer** | Phase 4 (REVIEWS) | Strengthens test coverage for new/changed code |

**How they work in practice:**

- With a **single AI session** (e.g., chatting with Gemini): the agent "role-switches" — reading each sub-agent's instructions when it's their turn
- With **multiple AI sessions** (e.g., overnight mode): each sub-agent can run as a separate session for parallelism

Any **CRITICAL** finding from Security or Architecture review sends the code back to the Builder for fixes before delivery.

---

## 10. Learnings & Self-Improvement

AutoBuild has a **dual learning system** that improves with every build.

### Project Learnings (`results/learnings.md`)

After each completed intent, the agent records what worked and what didn't:

```markdown
## 2026-03-25: Rate Limiting Implementation
- FastAPI middleware approach cleanest — decorator pattern caused import cycles
- In-memory rate limiter sufficient for current scale
- Pattern: always test with burst requests, not just steady-state
```

These learnings are loaded before every new intent on the same project. The AI avoids past mistakes and leverages proven patterns.

### Shared Learnings (`sharedlearnings.md`)

Universal patterns that apply across all projects:

- "Always validate environment variables at startup"
- "One logical change per commit"
- "Test behavior, not implementation"

If you use AutoBuild across multiple projects, keep `sharedlearnings.md` in a central location and set the path in your `project-context.md`.

---

## 11. Tips & Best Practices

### Writing Good Intents

- **Be specific** — "Add caching to the user endpoint" is better than "Make the app faster"
- **Define clear acceptance criteria** — Testable, binary (done or not done)
- **Set explicit scope boundaries** — OUT items prevent scope creep
- **Choose the right build mode** — Don't use iterative for simple tasks; don't use standard when there are genuinely multiple valid approaches

### Quality Config Tuning

- Start with a preset, tune weights after 3–5 builds
- If the AI keeps getting low architecture scores, make your `project-context.md` more specific
- If security scores are consistently low, check that the security scanning command in your config is working

### Git Workflow

- AutoBuild always works on branches (`autobuild/[intent-name]`), never on main
- In iterative mode, sub-branches are `autobuild/[intent-name]/approach-N`
- Each kept improvement is a separate commit, easy to cherry-pick or revert
- You merge to main only after reviewing and approving

### Getting the Most from Learnings

- After a tricky build, take 2 minutes to review and refine the auto-generated learnings
- Move universal patterns (not project-specific) to `sharedlearnings.md`
- Delete learnings that are no longer relevant

---

## 12. Troubleshooting

### "The agent doesn't follow the workflow"

Make sure you're pointing it at `.autobuild/program.md` specifically. A common mistake is pointing the agent at the README or a random file. The instruction should be:

> "Read `.autobuild/program.md` and execute the AutoBuild workflow."

### "The quality score is always 0"

Check that your `quality-config.md` has correct commands. Run each command manually first to verify they work in your project. Common issues:
- Wrong source directory (e.g., `mypy src/` but your code is in `app/`)
- Tools not installed (`pip install pytest ruff mypy bandit`)

### "The agent skips sub-agent reviews"

The agent should activate sub-agents in Phase 4. If it's skipping them, remind it to re-read `program.md` — specifically Phase 4: SUB-AGENT REVIEWS. Each sub-agent card in `agents/` has its own instructions.

### "The agent modifies files outside the intent scope"

Your `project-context.md` should list architecture rules. Add an explicit anti-pattern: *"Never modify files outside the intent's declared scope."* The Architecture Reviewer will flag violations.

### "Results aren't being logged"

Verify `results/results.tsv` exists and has the correct header row. The Quality Scorer agent appends rows — if the file is missing, it can't log.

---

## Quick Reference Card

| I want to... | Do this |
|--------------|---------|
| Start a new task | Copy an intent template → fill it in → save as `active-intent.md` |
| Change quality weights | Edit `.autobuild/quality-config.md` |
| Add a project rule | Edit `.autobuild/project-context.md` |
| Review past experiments | Open `results/results.tsv` |
| See what the AI learned | Read `results/learnings.md` |
| Review the latest build | Open `evaluation/latest-review.md` |
| Use a different build strategy | Change the `Build Mode` in your intent |
| Share learnings across projects | Add to `sharedlearnings.md`, set path in `project-context.md` |
