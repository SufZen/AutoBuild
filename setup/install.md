# How to Install AutoBuild

> Add AutoBuild to any project in under 5 minutes.

## Step 1: Bootstrap via AI (Recommended)

The fastest way to install AutoBuild is to let your AI agent do it for you.

1. Drop `setup/bootstrap.md` from the AutoBuild repository into your project root.
2. Ask your AI: **"Read bootstrap.md and initialize AutoBuild for me."**

The AI will intelligently copy the orchestrator files, analyze your tech stack, and generate your `project-context.md` and `quality-config.md` automatically.

---

## Step 2 (Alternative): Manual Installation

If you prefer to install manually:

1. Create a `.autobuild/` folder in your project root, then copy `core/program.md`, `core/quality-pipeline.md`, and the entire `agents/`, `programs/`, `intents/`, and `evaluation/` directories into it.
2. Copy `setup/project-context.template.md` to `.autobuild/project-context.md` and fill in your tech stack.
3. Pick your stack preset from `stacks/` (e.g., `stacks/python.md`), copy it to `.autobuild/quality-config.md`, and tweak.
4. Create an empty `results/results.tsv` file with the correct headers and copy `core/learnings-template.md` to `results/learnings.md`.

## Step 3: Start Building

1. Copy an intent template (e.g., `.autobuild/intents/feature.md`)
2. Fill it in and save as `.autobuild/intents/active-intent.md`
3. Point your AI coding agent at `.autobuild/program.md`
4. Tell the agent: "Read program.md and execute the AutoBuild workflow"

## Shared Learnings (Optional)

If you use AutoBuild across multiple projects, keep `sharedlearnings.md` in a central location and record that path in `.autobuild/project-context.md`. The orchestrator should load it only when a shared source is explicitly provided.

## Quick Reference

| File | Purpose | When to Edit |
| --- | --- | --- |
| `program.md` | Agent operating manual | Rarely (if customizing the workflow) |
| `project-context.md` | Your project's rules | Once at setup, update as project evolves |
| `quality-config.md` | Quality weights and commands | Once at setup, tune as needed |
| `intents/active-intent.md` | Current work | Every new task |
| `results/learnings.md` | Growing knowledge | Auto-updated by the agent |
| `evaluation/*.md` | Review checklists and generated review output | Checklists rarely, latest review every run |
