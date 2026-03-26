# AutoBuild — Product Requirements Document

> Created using the BMad Method™ MTH-35 Full PRD Track — see [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD)

## 1. Overview

**AutoBuild** is a project-agnostic, portable framework that turns any AI coding agent into a structured, self-improving software builder.

**Who:** Developers using AI coding agents (Gemini, Claude Code, Cursor, Copilot, etc.)

**Problem:** AI coding agents are powerful but unstructured. Without guardrails, they produce inconsistent quality, ignore security, drift from architecture, and can't iteratively improve. Developers spend as much time reviewing and correcting AI output as they would writing the code themselves.

**Solution:** A set of markdown-based instructions ("programs") that give AI agents a structured, iterative development process with built-in quality gates, security checks, and self-improving feedback loops.

**Tagline:** *You set the intent. AI builds iteratively. You evaluate the result.*

## 2. User Stories

| # | As a... | I want to... | So that... |
|---|---------|-------------|-----------|
| 1 | Developer | Define what I want built in a simple template | The AI has clear constraints and acceptance criteria |
| 2 | Developer | Let the AI try multiple approaches and keep the best one | I get the optimal implementation, not just the first one |
| 3 | Developer | Set up overnight autonomous runs | I wake up to completed features and experiments |
| 4 | Developer | See a quality score for every iteration | I can compare approaches objectively |
| 5 | Developer | Know that security is checked on every iteration | I don't ship vulnerabilities |
| 6 | Developer | Use this on any project regardless of tech stack | I don't need to learn a new tool per project |
| 7 | Developer | Have AutoBuild learn from past work | The system gets faster and better over time |
| 8 | Developer | Get a structured review checklist at the end | I can evaluate quality quickly and thoroughly |

## 3. Functional Requirements

### 3.1 Intent System
- **FR-01:** Template-based intent creation (feature / optimize / refactor / fix)
- **FR-02:** Each intent defines: goal, scope (IN/OUT), acceptance criteria, constraints, build mode
- **FR-03:** Build modes: `standard` (single-pass), `iterative` (multi-approach), `overnight` (autonomous loop), `optimize` (metric hill-climb)

### 3.2 Core Program (Agent Operating Manual)
- **FR-04:** Single entry point (`program.md`) that orchestrates everything
- **FR-05:** Auto-selects the right build program based on intent's build mode
- **FR-06:** Loads project context before any work
- **FR-07:** Git workflow: branch per intent, sub-branches per experiment approach

### 3.3 Quality Pipeline
- **FR-08:** Multi-layer quality scoring (not just "tests pass")
- **FR-09:** Configurable quality checks: tests, lint, types, security, performance, coverage, complexity
- **FR-10:** Configurable weights per project (some projects care more about security, others about performance)
- **FR-11:** Composite Quality Score computed after each iteration
- **FR-12:** Branch retention decision based on score improvement and security outcome
- **FR-12a:** Delivery readiness band reported separately from branch retention

### 3.4 Build Programs
- **FR-13:** Standard build: plan → implement → quality check → self-review → present
- **FR-14:** Iterative build: try N approaches on separate branches → score each → present best
- **FR-15:** Overnight build: autonomous loop (autoresearch-style), runs until interrupted or plateau criteria are met, logs everything
- **FR-16:** Optimization loop: given a specific metric, hill-climb toward optimal value

### 3.5 Evaluation System
- **FR-17:** Structured evaluation checklist for human review (correctness, security, architecture, quality)
- **FR-18:** Results log (TSV) tracking every experiment attempt with scores
- **FR-19:** Comparison view: "approach A scored 85, approach B scored 72"

### 3.6 Self-Optimization (Dual Learning Layer)
- **FR-20:** After each completed intent, record project-specific patterns in `learnings.md`
- **FR-21:** Learnings are loaded as context for future intents on the same project
- **FR-22:** Over time, the system builds a project-specific "playbook"
- **FR-23:** Cross-project systematic learnings are recorded in `sharedlearnings.md` (in AutoBuild root)
- **FR-24:** Shared learnings include workflow optimizations, universal patterns, and quality insights
- **FR-25:** Both learnings files are loaded by the agent — project-specific first, shared second

### 3.7 Multi-Agent Architecture
- **FR-26:** System defines specialist sub-agents, each with a focused role
- **FR-27:** Sub-agents: Builder (implements), Security Reviewer, Architecture Reviewer, Quality Scorer, Test Writer
- **FR-28:** The main `program.md` orchestrates sub-agents when to activate
- **FR-29:** Each sub-agent has its own checklist and evaluation criteria
- **FR-30:** Sub-agents can be invoked as separate AI sessions or as role-switching within a single session

### 3.8 Pre-Built Quality Stacks
- **FR-31:** Include quality config presets for common stacks:
  - Python: pytest, ruff, mypy, bandit, coverage
  - Node/TypeScript: vitest/jest, eslint, tsc, npm audit, c8/istanbul
  - Go: go test, golangci-lint, govulncheck
  - Rust: cargo test, clippy, cargo audit
- **FR-32:** Users can start from a preset and customize weights/commands
- **FR-33:** Presets must still map onto the same seven canonical checks, even when one check uses multiple commands

## 4. Non-Functional Requirements

| NFR | Requirement |
|-----|------------|
| **Portability** | Pure markdown — zero dependencies, no install scripts |
| **Agent-agnostic** | Must work with any AI coding agent (Gemini, Claude, Cursor, etc.) |
| **Zero lock-in** | Removing AutoBuild from a project leaves no trace except the docs |
| **Speed** | Adding AutoBuild to a new project < 5 minutes |
| **Clarity** | A developer unfamiliar with AutoBuild should understand the system from README alone |

## 5. Information Architecture

```
.autobuild/                   ← Dropped into ANY project root
├── program.md                ← THE entry point (agent reads this first)
├── quality-pipeline.md       ← Scoring rules and readiness bands
├── project-context.md        ← Filled in once per project
├── quality-config.md         ← Quality weights and tool configuration
│
├── agents/                   ← Sub-agent role cards
├── programs/                 ← Build-mode instructions
│
├── intents/
│   ├── _template.md          ← Copy and fill per task
│   └── active-intent.md      ← Current work (only one active at a time)
│
├── results/
│   ├── results.tsv           ← Experiment log
│   └── learnings.md          ← Accumulated knowledge
│
└── evaluation/
    ├── review-checklist.md
    ├── security-checklist.md
    ├── architecture-checklist.md
    └── latest-review.md      ← Most recent evaluation output
```

### Data Flow

```
1. Human writes intent → intents/active-intent.md
2. Agent reads:
   - program.md (operating manual)
   - project-context.md (project specifics)
   - quality-config.md (what to measure)
   - active-intent.md (what to build)
   - learnings.md (past knowledge)
   - shared learnings source if project-context declares one
3. Agent builds iteratively (per selected program)
4. Each iteration → quality pipeline → score → branch retention decision + delivery readiness
5. Results logged to results.tsv
6. Best result presented with evaluation/latest-review.md plus supporting checklists
7. Human reviews and approves/requests changes
8. Learnings saved for next time
```

## 6. Technical Constraints

- Must be pure markdown (no Python, no npm, no executables)
- Must not require any specific AI agent or IDE
- Must not require any specific CI/CD system
- Programs must be agent-interpretable (clear, unambiguous instructions)
- Git is the only hard requirement (for branch/commit history)

## 7. Success Metrics

| Metric | Target |
|--------|--------|
| Time to add AutoBuild to a new project | < 5 minutes |
| Quality score improvement over iterations (iterative mode) | Measurable improvement on ≥ 80% of intents |
| Security issues caught before human review | 100% of common patterns (hardcoded secrets, missing validation) |
| Developer time spent on review vs. writing from scratch | < 30% (review) vs. 100% (manual) |

## 8. Resolved Decisions

1. ✅ **Pre-built quality stacks** — Yes. Include presets for Python, Node/TS, Go, Rust.
2. ✅ **Dual learnings** — Both. Per-project `learnings.md` + cross-project `sharedlearnings.md`.
3. ✅ **Multi-agent** — Yes. Specialist sub-agents: Builder, Security Reviewer, Architecture Reviewer, Quality Scorer, Test Writer.
