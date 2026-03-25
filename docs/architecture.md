# AutoBuild — Architecture Document

> BMAD MTH-36 Architecture Workflow

## Architecture Decision Records

### ADR-001: Pure Markdown, No Code
**Decision:** AutoBuild is a set of markdown files with no executable code.
**Rationale:** Maximum portability. Works with any AI agent, any IDE, any project.
**Tradeoff:** Less automation (no script to run quality pipeline) — but the programs instruct the AI agent to run the right commands.

### ADR-002: Single Entry Point
**Decision:** `program.md` is the ONE file the AI agent is pointed to.
**Rationale:** Simplicity. The agent reads `program.md`, which tells it to load everything else.
**Tradeoff:** `program.md` will be the longest file — but it's the agent's "operating manual" so this is appropriate.

### ADR-003: Git-Based Experiment Isolation
**Decision:** Each intent gets a branch. Each approach in iterative mode gets a sub-branch.
**Rationale:** Clean experiment isolation, easy comparison, nothing destructive to main.
**Pattern:**
```
main
└── autobuild/intent-name
    ├── autobuild/intent-name/approach-1  → score: 78
    ├── autobuild/intent-name/approach-2  → score: 85 ← merge this one
    └── autobuild/intent-name/approach-3  → score: 72
```

### ADR-004: Composite Quality Score
**Decision:** Quality is a weighted composite score, not a single pass/fail.
**Rationale:** "Tests pass" is necessary but insufficient. Security, lint, complexity, performance all matter.
**Configuration:** Weights live in `quality-config.md`, user-configurable per project.
**Clarification:** The score drives two outcomes: branch retention and delivery readiness.

### ADR-005: Dual Self-Optimization via Learnings Files
**Decision:** Two-tier learnings system:
- `learnings.md` (per project) — project-specific patterns, conventions, gotchas
- `sharedlearnings.md` (in AutoBuild root) — universal workflow patterns, cross-project insights
**Rationale:** Project patterns don't transfer ("use Redis for this app"), but systematic patterns DO ("always check env vars before hardcoding").
**Tradeoff:** Agent must load both files, adding context. But the systematic improvements compound across all projects.

### ADR-006: Multi-Agent Sub-Agent Architecture
**Decision:** Define 5 specialist sub-agents that the orchestrator invokes:
```
┌──────────────────────────────────────┐
│          ORCHESTRATOR                │
│         (program.md)                 │
├──────────┬──────────┬────────────────┤
│ Builder  │ Security │ Architecture   │
│ Agent    │ Reviewer │ Reviewer       │
├──────────┼──────────┼────────────────┤
│ Quality  │ Test     │                │
│ Scorer   │ Writer   │                │
└──────────┴──────────┴────────────────┘
```
**Rationale:** Specialist focus produces higher quality than one generalist doing everything.
**Implementation:** Each sub-agent is a markdown "role card" with instructions. Can be:
- Separate AI sessions (for overnight/parallel work)
- Role-switching within a single session (for interactive work)

### ADR-007: Pre-Built Quality Stack Presets
**Decision:** Include ready-made `quality-config.md` files for Python, Node/TS, Go, Rust.
**Rationale:** Reduces setup friction from "figure out all the tools" to "pick your stack, customize weights."
**Pattern:** User copies a preset, modifies weights to match their priorities. Presets may use multiple commands inside one canonical quality check.

## Component Design

### 1. Program Engine (`core/program.md`)

The agent's operating manual. Structured as phases:

```
Phase 0: LOAD CONTEXT
  → Read project-context.md
  → Read quality-config.md
  → Read active-intent.md
  → Read learnings.md (if exists)
  → Read shared learnings source if declared in project-context.md

Phase 1: ANALYSIS
  → Understand the intent
  → Identify affected files, dependencies, risks
  → Estimate complexity (simple / medium / complex)

Phase 2: PLANNING
  → Break intent into implementation steps
  → Define "done when" criteria from acceptance criteria
  → Select build program based on mode
  → Present plan for approval (unless overnight mode)

Phase 3: BUILD
  → Execute the selected build program
  → Each iteration runs through quality pipeline
  → Decide branch retention and delivery readiness
  → Log every attempt to results.tsv

Phase 4: SUB-AGENT REVIEWS
  → Security Reviewer, Architecture Reviewer, Quality Scorer, Test Writer
  → Any CRITICAL finding or architectural violation → back to Phase 3

Phase 5: EVALUATE & PRESENT
  → Self-review using shipped evaluation checklists
  → Generate latest-review.md
  → Present to human

Phase 6: LEARN
  → Record what worked and what didn't
  → Update learnings.md
  → Close the intent
```

### 2. Quality Pipeline (`core/quality-pipeline.md`)

Defines HOW to measure quality. The agent runs these checks:

```
QUALITY PIPELINE:
┌─────────────────────┬──────────────────────┬────────┐
│ Check               │ How (agent runs)     │ Weight │
├─────────────────────┼──────────────────────┼────────┤
│ Tests               │ Project test command │  35%   │
│ Lint / Format       │ Project lint command │  15%   │
│ Type Safety         │ Type checker         │  10%   │
│ Security            │ Secret scan + audit  │  15%   │
│ Complexity          │ Count functions/LOC  │  10%   │
│ Coverage            │ Coverage tool        │   5%   │
│ Architecture        │ Check conventions    │  10%   │
└─────────────────────┴──────────────────────┴────────┘

Score = Σ(check_result × weight) → 0-100
```

Projects configure their specific commands in `quality-config.md`.

Retention vs readiness:
- Retention answers: should this branch state be kept?
- Readiness answers: is this result ready to deliver now?

### 3. Results System

**`results.tsv`** — Tab-separated log of every experiment:
```
timestamp	branch	intent	approach	quality_score	tests	lint	security	types	complexity	coverage	architecture	status	description
2026-03-25T14:00	autobuild/caching	caching	approach-1	78	pass	pass	1-warning	pass	medium	82%	compliant	keep	Redis-based caching
2026-03-25T14:30	autobuild/caching	caching	approach-2	85	pass	pass	clean	pass	low	88%	compliant	keep	LRU with TTL
```

`status` records branch action only: `keep` or `discard`.

**`learnings.md`** — Accumulated knowledge:
```markdown
## 2026-03-25: Caching Implementation
- LRU with TTL outperformed Redis for this project's scale
- Security scan caught a hardcoded Redis password in approach-1
- Pattern: always use environment variables for connection strings
```

## File Map

```
AutoBuild (repo — the distributable framework)
├── README.md
├── sharedlearnings.md          # Cross-project learnings (grows over time)
│
├── core/
│   ├── program.md              # Orchestrator — agent operating manual
│   ├── quality-pipeline.md     # Quality scoring system
│   └── learnings-template.md   # Template for learnings file
│
├── agents/                     # Sub-agent role cards
│   ├── builder.md              # Implements code
│   ├── security-reviewer.md    # Security analysis
│   ├── architecture-reviewer.md # Architecture compliance
│   ├── quality-scorer.md       # Runs quality pipeline
│   └── test-writer.md          # Writes/strengthens tests
│
├── setup/
│   ├── install.md              # How to add to a project
│   ├── project-context.template.md  # Project config template
│   └── quality-config.template.md   # Quality weights template
│
├── stacks/                     # Pre-built quality presets
│   ├── python.md               # pytest, ruff, mypy, bandit
│   ├── node-typescript.md      # vitest/jest, eslint, tsc, npm audit
│   ├── go.md                   # go test, golangci-lint, govulncheck
│   └── rust.md                 # cargo test, clippy, cargo audit
│
├── intents/
│   ├── _template.md
│   ├── feature.md
│   ├── optimize.md
│   ├── refactor.md
│   └── fix.md
│
├── programs/
│   ├── standard-build.md
│   ├── iterative-build.md
│   ├── overnight-build.md
│   └── optimization-loop.md
│
├── evaluation/
│   ├── review-checklist.md
│   ├── security-checklist.md
│   └── architecture-checklist.md
│
├── examples/
│   └── realizeos-5/
│
└── docs/
    ├── PRD.md
    ├── architecture.md
    └── user-guide.md          # Full user guide and reference
```

When installed into a project:
```
your-project/
├── .autobuild/
│   ├── program.md              # Orchestrator (from core/)
│   ├── quality-pipeline.md     # Quality system (from core/)
│   ├── project-context.md      # Filled in by user
│   ├── quality-config.md       # From stacks/ preset, customized
│   │
│   ├── agents/                 # Sub-agent role cards (from agents/)
│   │   ├── builder.md
│   │   ├── security-reviewer.md
│   │   ├── architecture-reviewer.md
│   │   ├── quality-scorer.md
│   │   └── test-writer.md
│   │
│   ├── programs/               # Build instructions (from programs/)
│   │   ├── standard-build.md
│   │   ├── iterative-build.md
│   │   ├── overnight-build.md
│   │   └── optimization-loop.md
│   │
│   ├── intents/
│   │   ├── _template.md
│   │   └── active-intent.md    # Current work
│   │
│   ├── results/
│   │   ├── results.tsv
│   │   └── learnings.md        # Project-specific learnings
│   │
│   └── evaluation/
│       ├── review-checklist.md
│       ├── security-checklist.md
│       ├── architecture-checklist.md
│       └── latest-review.md
│
└── ... (your project files)
```

Note: shared learnings are optional. If used, their source path should be declared in `project-context.md`.
