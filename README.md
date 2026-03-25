# AutoBuild

**A self-improving AI development machine.** Drop it into any project. Define your intent. AI builds iteratively with quality gates, security scanning, and multi-agent reviews. The system learns and gets better over time.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## How It Works

```
YOU                          AUTOBUILD                           RESULT
────                         ─────────                           ──────
Write intent        →   AI reads context + learnings      →   Quality-scored
"Build feature X"        Selects build program                code, reviewed
                         Implements iteratively                by 5 sub-agents
                         Runs quality pipeline (7 checks)
                         Security scan every iteration
                         Keeps best, discards rest
                         Records learnings for next time
```

### The 3-Step Workflow

1. **Intent** — Fill in a simple template: what you want, acceptance criteria, constraints
2. **Build** — Point your AI agent at `program.md` → it works autonomously
3. **Evaluate** — Review the output using structured checklists

## Quick Start

```bash
# 1. Clone AutoBuild
git clone https://github.com/SufZen/AutoBuild.git

# 2. Copy into your project
cp -r AutoBuild/core AutoBuild/agents AutoBuild/intents your-project/.autobuild/
cp AutoBuild/stacks/python.md your-project/.autobuild/quality-config.md  # pick your stack

# 3. Fill in project context
cp AutoBuild/setup/project-context.template.md your-project/.autobuild/project-context.md
# Edit project-context.md with your project's info

# 4. Create your first intent
cp your-project/.autobuild/intents/_template.md your-project/.autobuild/intents/active-intent.md
# Fill in what you want built

# 5. Tell your AI agent
"Read .autobuild/program.md and execute the AutoBuild workflow"
```

## Build Modes

| Mode | When to Use | How It Works |
| --- | --- | --- |
| **Standard** | Simple, clear tasks | Single-pass: implement → quality check → review |
| **Iterative** | Multiple valid approaches | Try N approaches on branches, keep highest-scoring one |
| **Overnight** | Large optimization space | Infinite autonomous loop — review results in the morning |
| **Optimize** | Target a specific metric | Hill-climb toward a measurable goal (latency, size, etc.) |

## Quality Pipeline

Every iteration is scored on 7 dimensions:

| Check | Weight | What It Catches |
| --- | --- | --- |
| Tests | 35% | Broken functionality |
| Lint / Format | 15% | Style violations |
| Security | 15% | Secrets, CVEs, injection |
| Type Safety | 10% | Type errors |
| Complexity | 10% | Unmaintainable code |
| Coverage | 5% | Untested code paths |
| Architecture | 10% | Convention violations |

Score ≥ 80 → keep. Score < 60 → discard. Security CRITICAL → always discard.

## Multi-Agent Architecture

5 specialist sub-agents ensure quality from every angle:

| Agent | Focus |
| --- | --- |
| **Builder** | Writes production-ready code following project conventions |
| **Security Reviewer** | Scans for secrets, CVEs, injection, auth gaps |
| **Architecture Reviewer** | Ensures compliance with project patterns and boundaries |
| **Quality Scorer** | Runs the 7-check quality pipeline and scores iterations |
| **Test Writer** | Identifies coverage gaps and writes meaningful tests |

## Self-Optimization

AutoBuild gets smarter over time through dual learning:

- **`learnings.md`** (per project) — project-specific patterns that the AI agent reads before every new task
- **`sharedlearnings.md`** (cross-project) — universal patterns that apply to all your projects

## Project Structure

```
AutoBuild/
├── core/                    # The engine
│   ├── program.md           # ← START HERE — agent operating manual
│   ├── quality-pipeline.md  # Quality scoring system
│   └── learnings-template.md
├── agents/                  # Sub-agent role cards
│   ├── builder.md
│   ├── security-reviewer.md
│   ├── architecture-reviewer.md
│   ├── quality-scorer.md
│   └── test-writer.md
├── setup/                   # Configuration templates
│   ├── install.md
│   ├── project-context.template.md
│   └── quality-config.template.md
├── stacks/                  # Pre-built quality configs
│   ├── python.md
│   ├── node-typescript.md
│   ├── go.md
│   └── rust.md
├── intents/                 # Task templates
│   ├── _template.md
│   ├── feature.md
│   ├── optimize.md
│   ├── refactor.md
│   └── fix.md
├── programs/                # Build strategies
│   ├── standard-build.md
│   ├── iterative-build.md
│   ├── overnight-build.md
│   └── optimization-loop.md
├── evaluation/              # Review checklists
│   ├── review-checklist.md
│   ├── security-checklist.md
│   └── architecture-checklist.md
├── examples/                # Real examples
│   └── realizeos-5/
├── docs/                    # BMAD artifacts
│   ├── PRD.md
│   └── architecture.md
└── sharedlearnings.md       # Cross-project knowledge
```

## Supported Stacks

Pre-built quality configurations for:
- **Python** — pytest, ruff, mypy, bandit, pip-audit
- **Node / TypeScript** — vitest/jest, eslint, tsc, npm audit
- **Go** — go test, golangci-lint, govulncheck
- **Rust** — cargo test, clippy, cargo audit

Custom stacks are easy to create — see `setup/quality-config.template.md`.

## Inspired By

- [**BMAD-METHOD**](https://github.com/bmad-code-org/BMAD-METHOD) — Structured agile AI development with specialized agents and workflows
- [**Autoresearch**](https://github.com/karpathy/autoresearch) by Andrej Karpathy — Autonomous experiment loops for iterative improvement

AutoBuild combines BMAD's structure with Autoresearch's autonomous iteration to create something greater than either alone.

## License

MIT
