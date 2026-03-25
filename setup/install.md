# How to Install AutoBuild

> Add AutoBuild to any project in under 5 minutes.

## Step 1: Copy Files

Copy the following folders into your project root as `.autobuild/`:

```
.autobuild/
├── program.md              ← from core/program.md
├── quality-pipeline.md     ← from core/quality-pipeline.md
├── agents/                 ← entire agents/ folder
├── intents/
│   └── _template.md        ← from intents/_template.md
├── results/
│   ├── results.tsv         ← create empty (headers only)
│   └── learnings.md        ← from core/learnings-template.md
└── evaluation/             ← create empty folder
```

## Step 2: Configure Your Project

### 2a. Fill in Project Context

Copy `setup/project-context.template.md` to `.autobuild/project-context.md` and fill in:

- Your tech stack (language, framework, database)
- Your code conventions (naming, file structure, patterns)
- Your anti-patterns (what to avoid)

### 2b. Configure Quality

1. Pick your stack preset from `stacks/` (e.g., `stacks/python.md`)
2. Copy it to `.autobuild/quality-config.md`
3. Customize the weights if needed (or use defaults)

## Step 3: Initialize Results Log

Create `results/results.tsv` with this header:

```tsv
timestamp	branch	intent	approach	quality_score	tests	lint	security	types	complexity	coverage	architecture	status	description
```

## Step 4: Start Building

1. Copy an intent template (e.g., `intents/feature.md`)
2. Fill it in and save as `.autobuild/intents/active-intent.md`
3. Point your AI coding agent at `.autobuild/program.md`
4. Tell the agent: "Read program.md and execute the AutoBuild workflow"

## Shared Learnings (Optional)

If you use AutoBuild across multiple projects, keep `sharedlearnings.md` in a central location and reference it. Cross-project patterns accumulate there.

## Quick Reference

| File | Purpose | When to Edit |
| --- | --- | --- |
| `program.md` | Agent operating manual | Rarely (if customizing the workflow) |
| `project-context.md` | Your project's rules | Once at setup, update as project evolves |
| `quality-config.md` | Quality weights and commands | Once at setup, tune as needed |
| `intents/active-intent.md` | Current work | Every new task |
| `results/learnings.md` | Growing knowledge | Auto-updated by the agent |
