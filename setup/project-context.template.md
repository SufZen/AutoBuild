# Project Context

> Fill this in once per project. This is your project's "constitution" — AI agents read it before every task.

## Project Overview

- **Name:** <!-- Project name -->
- **Description:** <!-- One sentence: what does this project do? -->
- **Primary language:** <!-- e.g., Python 3.12, TypeScript 5.x, Go 1.22 -->
- **Framework:** <!-- e.g., FastAPI, Next.js, Gin, Actix -->

## Tech Stack

| Layer | Technology |
| --- | --- |
| Language | <!-- e.g., Python 3.12 --> |
| Framework | <!-- e.g., FastAPI --> |
| Database | <!-- e.g., PostgreSQL 16 --> |
| ORM | <!-- e.g., SQLAlchemy 2.x --> |
| Testing | <!-- e.g., pytest + pytest-asyncio --> |
| Linting | <!-- e.g., ruff --> |
| Type Checking | <!-- e.g., mypy --> |
| CI/CD | <!-- e.g., GitHub Actions --> |

## Code Conventions

### File Structure

```
<!-- Describe your project's directory structure -->
<!-- e.g., src/ for source, tests/ for tests, docs/ for documentation -->
```

### Naming Conventions

- **Files:** <!-- e.g., snake_case.py, kebab-case.ts -->
- **Functions:** <!-- e.g., snake_case -->
- **Classes:** <!-- e.g., PascalCase -->
- **Constants:** <!-- e.g., UPPER_SNAKE_CASE -->
- **Variables:** <!-- e.g., camelCase / snake_case -->

### Import Ordering

<!-- e.g., stdlib → third-party → local, sorted alphabetically -->

### Documentation Style

<!-- e.g., Google-style docstrings, JSDoc, rustdoc -->

## Architecture Decisions

<!-- List key architectural decisions AI agents should respect -->

1. <!-- e.g., "All API endpoints must go through the middleware chain" -->
2. <!-- e.g., "Data access only through repository classes, never direct SQL" -->
3. <!-- e.g., "Configuration loaded from environment variables, never hardcoded" -->

## Anti-Patterns (NEVER DO)

1. <!-- e.g., "Never import from internal modules of third-party packages" -->
2. <!-- e.g., "Never use synchronous I/O in async contexts" -->
3. <!-- e.g., "Never catch generic Exception without re-raising" -->

## Implementation Rules

1. <!-- e.g., "Every public function must have type annotations" -->
2. <!-- e.g., "Every new feature must have unit tests" -->
3. <!-- e.g., "All database changes require a migration file" -->
