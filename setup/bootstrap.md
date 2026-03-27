# AutoBuild Bootstrapper

> Drop this file into your new project, then ask your AI: "Read bootstrap.md and initialize AutoBuild for me."

## System Instructions for AI Agent

You are tasked with onboarding this project into the AutoBuild execution loop.

### 1. File Structure Setup

Create a `.autobuild/` directory in the current workspace.
Copy the following files/directories from the AutoBuild source repository into `.autobuild/`:

- `core/program.md` -> `.autobuild/program.md`
- `core/quality-pipeline.md` -> `.autobuild/quality-pipeline.md`
- `agents/` -> `.autobuild/agents/`
- `programs/` -> `.autobuild/programs/`
- `intents/` -> `.autobuild/intents/`
- `evaluation/` -> `.autobuild/evaluation/`

### 2. Result Log Initialization

Create an empty `results.tsv` file in `.autobuild/results/` with the following header exactly:

`timestamp    branch    intent    approach    quality_score    tests    lint    security    types    complexity    coverage    architecture    status    description`

Copy `core/learnings-template.md` to `.autobuild/results/learnings.md`.

### 3. Automatic Project Profiling

Analyze the current project's source code, manifest files (e.g., package.json, uv.lock, go.mod), and directory structure.
Based on your findings:

1. Copy the `setup/project-context.template.md` outline and fill it out intelligently, saving it as `.autobuild/project-context.md`. Dedicate specific attention to noting the **Run Environment** (e.g., local shell, docker container).
2. Select the most appropriate stack template from `stacks/` (e.g., `python.md`, `node-typescript.md`) and copy it to `.autobuild/quality-config.md`.

Once complete, notify the user that AutoBuild is ready, and they can generate their first intent by editing `.autobuild/intents/active-intent.md`.
