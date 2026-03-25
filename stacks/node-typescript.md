# Node / TypeScript Stack — Quality Configuration

> Pre-built quality config for Node.js and TypeScript projects.

## Check Commands

| Check | Command | Expected Output |
| --- | --- | --- |
| **Tests** | `npm test` or `npx vitest run` | Exit code 0 |
| **Lint** | `npx eslint .` | Exit code 0 |
| **Format** | `npx prettier --check .` | Exit code 0 |
| **Type Check** | `npx tsc --noEmit` | Exit code 0 |
| **Security** | `npm audit --audit-level=high` | No HIGH+ vulnerabilities |
| **Coverage** | `npx vitest run --coverage` | Coverage % |

## Weights

| Check | Weight |
| --- | --- |
| Tests | 35% |
| Lint / Format | 15% |
| Security (npm audit) | 15% |
| Type Safety (tsc) | 10% |
| Complexity | 10% |
| Coverage | 5% |
| Architecture | 10% |

## Installation

```bash
npm install -D typescript eslint prettier vitest @vitest/coverage-v8
```

## Notes

- For JavaScript-only projects, set Type Safety weight to 0% and redistribute
- For Next.js, use `next lint` instead of `eslint .`
- For Bun, replace `npm` commands with `bun` equivalents
