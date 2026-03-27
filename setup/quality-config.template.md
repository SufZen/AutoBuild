# Quality Configuration

> Configure quality checks, weights, and commands for your project. Start from a stack preset or customize from scratch.

AutoBuild uses **7 canonical checks**: tests, lint/format, security, type safety, complexity, coverage, and architecture.

## Check Commands

Fill in the commands that your project uses for each check. Ensure you run them in the correct **Execution Environment** (e.g., inside Docker, with specific env vars).

| Check | Command | Expected Output |
| --- | --- | --- |
| **Tests** | `<!-- e.g., pytest -v -->` | Exit code 0 = pass |
| **Lint / Format** | `<!-- e.g., ruff check . && ruff format --check . -->` | Exit code 0 = clean and formatted |
| **Type Check** | `<!-- e.g., mypy src/ -->` | Exit code 0 = pass |
| **Security** | `<!-- e.g., bandit -r src/ -ll && pip-audit -->` | No critical findings; warnings documented |
| **Complexity** | `<!-- Manual or tool-assisted review, e.g., radon cc src/ -->` | Complexity observations recorded |
| **Coverage** | `<!-- e.g., pytest --cov=src --cov-report=term -->` | Coverage percentage |
| **Architecture** | `<!-- Manual review against project-context.md -->` | Compliance observations recorded |

## Weights

Customize how much each check contributes to the composite Quality Score.

| Check | Weight | Notes |
| --- | --- | --- |
| Tests | 35% | <!-- Increase for test-critical projects --> |
| Lint / Format | 15% | |
| Security | 15% | <!-- Increase to 25% for security-critical apps --> |
| Type Safety | 10% | <!-- Set to 0% if not using type checking --> |
| Complexity | 10% | |
| Coverage | 5% | |
| Architecture | 10% | |

**Total must equal 100%.**

## Retention and Readiness

AutoBuild uses two separate decisions:

| Decision | Rule |
| --- | --- |
| **Retention** | Keep branch state only if the iteration improves or maintains the best score and no critical security issue is present |
| **Readiness: ready to deliver** | ≥ 80 |
| **Readiness: acceptable with notes** | 60 - 79 |
| **Readiness: not ready** | < 60 |

## Custom Checks (Optional)

Add project-specific checks beyond the defaults:

<!-- Example:
| Check | Command | Weight | Score Method |
|-------|---------|--------|-------------|
| Bundle Size | `npm run build && stat dist/index.js` | 5% | < 100KB = full, < 200KB = half |
| API Response Time | `npm run bench` | 5% | < 200ms = full, < 500ms = half |
-->
