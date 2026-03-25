# Python Stack — Quality Configuration

> Pre-built quality config for Python projects using common tooling.

## Check Commands

| Check | Command | Expected Output |
| --- | --- | --- |
| **Tests** | `python -m pytest -v` | Exit code 0 |
| **Lint** | `ruff check .` | Exit code 0 |
| **Format** | `ruff format --check .` | Exit code 0 |
| **Type Check** | `mypy src/` | Exit code 0 |
| **Security** | `bandit -r src/ -ll` | No MEDIUM+ findings |
| **Coverage** | `python -m pytest --cov=src --cov-report=term` | Coverage % |
| **Dep Audit** | `pip-audit` | No known vulnerabilities |

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

Ensure these tools are available:

```bash
pip install pytest pytest-cov ruff mypy bandit pip-audit
```

## Notes

- Adjust `mypy src/` to match your project's source directory
- For Django projects, use `python manage.py test` instead of pytest
- For async projects, add `pytest-asyncio` to test dependencies
