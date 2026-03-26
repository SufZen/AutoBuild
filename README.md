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
                         Keeps better iterations               with a structured
                         Records learnings for next time       final review
```

### The 3-Step Workflow

1. **Intent** — Fill in a simple template: what you want, acceptance criteria, constraints
2. **Build** — Install AutoBuild into `.autobuild/`, then point your AI agent at `.autobuild/program.md`
3. **Evaluate** — Review `evaluation/latest-review.md` alongside the shipped checklists

## Repo vs Installed Entry Point

- In this repo, the source entry point is `core/program.md`
- In your project, the installed entry point is `.autobuild/program.md`

## Quick Start

```bash
# 1. Clone AutoBuild
git clone https://github.com/SufZen/AutoBuild.git

# 2. Create the .autobuild structure in your project
mkdir your-project/.autobuild
# Also create: agents/, programs/, intents/, results/, evaluation/

# 3. Copy the core workflow files
cp AutoBuild/core/program.md your-project/.autobuild/program.md
cp AutoBuild/core/quality-pipeline.md your-project/.autobuild/quality-pipeline.md
cp AutoBuild/core/learnings-template.md your-project/.autobuild/results/learnings.md
cp -r AutoBuild/agents/* your-project/.autobuild/agents/
cp -r AutoBuild/programs/* your-project/.autobuild/programs/
cp -r AutoBuild/intents/* your-project/.autobuild/intents/
cp -r AutoBuild/evaluation/* your-project/.autobuild/evaluation/

# 4. Configure the project
cp AutoBuild/setup/project-context.template.md your-project/.autobuild/project-context.md
cp AutoBuild/stacks/python.md your-project/.autobuild/quality-config.md  # pick your stack

# 5. Initialize the results log
# Create your-project/.autobuild/results/results.tsv with the header shown in setup/install.md

# 6. Create your first intent
cp your-project/.autobuild/intents/_template.md your-project/.autobuild/intents/active-intent.md

# 7. Tell your AI agent
"Read .autobuild/program.md and execute the AutoBuild workflow"
```

For a more detailed setup walkthrough, see `setup/install.md`. For the full user guide covering the complete lifecycle, see [`docs/user-guide.md`](docs/user-guide.md).

## Build Modes

| Mode | When to Use | How It Works |
| --- | --- | --- |
| **Standard** | Simple, clear tasks | Single-pass: implement → quality gate → review |
| **Iterative** | Multiple valid approaches | Try N approaches on branches, compare them, keep the best branch state |
| **Overnight** | Large optimization space | Autonomous loop until interrupted or plateaued, then review results |
| **Optimize** | Target a specific metric | Hill-climb toward a measurable goal (latency, size, etc.) |

## Quality Pipeline

Every iteration is scored on 7 dimensions:

| Check | Weight | What It Catches |
| --- | --- | --- |
| Tests | 35% | Broken functionality |
| Lint / Format | 15% | Style and formatting violations |
| Security | 15% | Secrets, CVEs, injection, unsafe config |
| Type Safety | 10% | Type errors |
| Complexity | 10% | Unmaintainable code |
| Coverage | 5% | Untested code paths |
| Architecture | 10% | Convention and boundary violations |

Two decisions happen after each iteration:

- **Retention decision:** keep the branch state only if the score improved or held steady and there is no critical security failure
- **Readiness decision:** `>= 80` ready to deliver, `60-79` acceptable with notes, `< 60` not ready for delivery

## Multi-Agent Architecture

5 specialist sub-agents ensure quality from every angle:

| Agent | Focus |
| --- | --- |
| **Builder** | Writes production-ready code following project conventions |
| **Security Reviewer** | Scans for secrets, CVEs, injection, auth gaps |
| **Architecture Reviewer** | Ensures compliance with project patterns and boundaries |
| **Quality Scorer** | Runs the 7-check quality pipeline and reports branch retention plus delivery readiness |
| **Test Writer** | Identifies coverage gaps and writes meaningful tests |

## Self-Optimization

AutoBuild gets smarter over time through dual learning:

- **`results/learnings.md`** (per project) — project-specific patterns that the AI agent reads before every new task
- **`sharedlearnings.md`** (cross-project) — universal patterns that apply to all your projects when you provide a shared source path in `project-context.md`

## Project Structure

```
AutoBuild/
├── core/                    # The engine
│   ├── program.md           # Source entry point for the distributable workflow
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
├── evaluation/              # Review checklists shipped into .autobuild/
│   ├── review-checklist.md
│   ├── security-checklist.md
│   └── architecture-checklist.md
├── examples/                # Real examples
│   └── realizeos-5/
├── docs/                    # BMAD artifacts
│   ├── PRD.md
│   └── architecture.md
└── sharedlearnings.md       # Optional cross-project knowledge source
```

Installed into a project:

```
your-project/
└── .autobuild/
    ├── program.md
    ├── quality-pipeline.md
    ├── project-context.md
    ├── quality-config.md
    ├── agents/
    ├── programs/
    ├── intents/
    │   ├── _template.md
    │   └── active-intent.md
    ├── results/
    │   ├── results.tsv
    │   └── learnings.md
    └── evaluation/
        ├── review-checklist.md
        ├── security-checklist.md
        ├── architecture-checklist.md
        └── latest-review.md
```

## Supported Stacks

Pre-built quality configurations for:
- **Python** — pytest, ruff, mypy, bandit, pip-audit
- **Node / TypeScript** — vitest/jest, eslint, tsc, npm audit
- **Go** — go test, golangci-lint, govulncheck
- **Rust** — cargo test, clippy, cargo audit

Custom stacks are easy to create — see `setup/quality-config.template.md`.

## Acknowledgments

AutoBuild borrows concepts and methodology (not code) from two open-source projects:

- [**BMAD-METHOD**](https://github.com/bmad-code-org/BMAD-METHOD) by BMad Code, LLC — Structured agile AI development with specialized agents and workflows. AutoBuild's multi-agent architecture and structured workflow phases were inspired by the BMad Method™ approach.
- [**Autoresearch**](https://github.com/karpathy/autoresearch) by Andrej Karpathy — Autonomous experiment loops for iterative improvement. AutoBuild's overnight build mode was inspired by autoresearch's continuous experimentation pattern.

BMad™, BMad Method™, and BMad Core™ are trademarks of BMad Code, LLC. AutoBuild is not endorsed by or affiliated with BMad Code, LLC or Andrej Karpathy.

See [NOTICE](NOTICE) for full attribution details.

## License

MIT
