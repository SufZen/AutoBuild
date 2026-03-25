# Example: RealizeOS 5 — Project Context

> This is an example of a filled-in project-context.md for a real project.

## Project Overview

- **Name:** RealizeOS V5
- **Description:** An AI-powered personal operating system with multi-agent architecture for task management, optimization, and intelligent assistance.
- **Primary language:** Python 3.12
- **Framework:** FastAPI (async)

## Tech Stack

| Layer | Technology |
| --- | --- |
| Language | Python 3.12 |
| Framework | FastAPI (async) |
| Database | SQLite (dev) / PostgreSQL (prod) |
| ORM | SQLAlchemy 2.x (async) |
| Testing | pytest + pytest-asyncio |
| Linting | ruff |
| Type Checking | mypy |
| CI/CD | GitHub Actions |

## Code Conventions

### File Structure

```
realize_core/          # Core engine
  agents/              # Agent definitions
  skills/              # Skill plugins
  optimizer/           # Experiment engine
  security/            # Auth, RBAC, scanning
  storage/             # Storage providers
tests/                 # Test suite
docs/                  # Documentation
developer_resources/   # Dev guides
```

### Naming Conventions

- **Files:** snake_case.py
- **Functions:** snake_case
- **Classes:** PascalCase
- **Constants:** UPPER_SNAKE_CASE
- **Variables:** snake_case

## Architecture Decisions

1. All agents are defined as YAML skill files in `realize_core/skills/`
2. Security middleware chain must process every request
3. Configuration via environment variables, never hardcoded
4. Storage abstraction — all file I/O through storage providers
5. Database access only through SQLAlchemy models + async sessions

## Anti-Patterns (NEVER DO)

1. Never use synchronous I/O in async endpoint handlers
2. Never import from `_internal` modules of third-party packages
3. Never catch generic `Exception` without re-raising
4. Never hardcode model names — use the LLM routing system
5. Never bypass the security middleware

## Implementation Rules

1. Every public function must have type annotations
2. Every new feature must have unit tests
3. All database changes require an Alembic migration
4. All API endpoints must go through the middleware chain
5. Prompts must use the template system, never inline strings
