# Quality Configuration

> Configure quality checks, weights, and commands for your project. Start from a stack preset or customize from scratch.

## Check Commands

Fill in the commands that your project uses for each check:

| Check | Command | Expected Output |
| --- | --- | --- |
| **Tests** | `<!-- e.g., pytest -v -->` | Exit code 0 = pass |
| **Lint** | `<!-- e.g., ruff check . -->` | Exit code 0 = clean |
| **Type Check** | `<!-- e.g., mypy src/ -->` | Exit code 0 = pass |
| **Security** | `<!-- e.g., bandit -r src/ -->` | No HIGH/CRITICAL findings |
| **Coverage** | `<!-- e.g., pytest --cov=src --cov-report=term -->` | Coverage percentage |
| **Format** | `<!-- e.g., ruff format --check . -->` | Exit code 0 = formatted |

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

## Thresholds

| Threshold | Value |
| --- | --- |
| **Keep** (solid quality) | ≥ 80 |
| **Keep with notes** (acceptable) | 60 - 79 |
| **Discard** (quality too low) | < 60 |

## Custom Checks (Optional)

Add project-specific checks beyond the defaults:

<!-- Example:
| Check | Command | Weight | Score Method |
|-------|---------|--------|-------------|
| Bundle Size | `npm run build && stat dist/index.js` | 5% | < 100KB = full, < 200KB = half |
| API Response Time | `npm run bench` | 5% | < 200ms = full, < 500ms = half |
-->
