# Python Stack — Quality Configuration

> Pre-built quality config for Python projects using common tooling.

This preset still maps to AutoBuild's 7 canonical checks. Some checks use more than one command.

## Check Commands

| Check | Command | Expected Output |
| --- | --- | --- |
| **Tests** | `uv run pytest -v` | Exit code 0 |
| **Lint / Format** | `uv run ruff check . && uv run ruff format --check .` | Exit code 0 |
| **Type Check** | `uv run mypy src/` | Exit code 0 |
| **Security** | `uv run bandit -r src/ -ll && uv pip audit` | No critical findings |
| **Complexity** | `Manual review or tool-assisted check such as radon` | Complexity observations |
| **Coverage** | `uv run pytest --cov=src --cov-report=term` | Coverage % |
| **Architecture** | `Manual review against project-context.md` | Compliance observations |

## Weights

| Check | Weight |
| --- | --- |
| Tests | 35% |
| Lint / Format | 15% |
| Security (bandit + pip-audit) | 15% |
| Type Safety (mypy) | 10% |
| Complexity | 10% |
| Coverage | 5% |
| Architecture | 10% |

## Installation

Ensure `uv` is installed, then add these tools to your project:

```bash
uv add --dev pytest pytest-cov ruff mypy bandit pip-audit
```

## Notes

- `uv` is the 2026 standard for ultra-fast, reproducible execution.
- Adjust `uv run mypy src/` to match your project's source directory
- For Django projects, use `uv run manage.py test` instead of pytest
- For async projects, add `pytest-asyncio` to test dependencies
- If your shell does not support `&&`, replace the combined commands with the equivalent for your environment
