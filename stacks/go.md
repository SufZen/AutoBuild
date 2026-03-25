# Go Stack — Quality Configuration

> Pre-built quality config for Go projects.

This preset still maps to AutoBuild's 7 canonical checks. Some checks use more than one command.

## Check Commands

| Check | Command | Expected Output |
| --- | --- | --- |
| **Tests** | `go test ./...` | Exit code 0 |
| **Lint / Format** | `golangci-lint run` and `gofmt -l .` | Exit code 0 from linter and no output from `gofmt -l .` |
| **Type Check** | `go vet ./...` | Exit code 0 |
| **Security** | `govulncheck ./...` | No vulnerabilities |
| **Complexity** | `Manual review or project-specific tooling` | Complexity observations |
| **Coverage** | `go test -coverprofile=coverage.out ./... && go tool cover -func=coverage.out` | Coverage % |
| **Architecture** | `Manual review against project-context.md` | Compliance observations |

## Weights

| Check | Weight |
| --- | --- |
| Tests | 35% |
| Lint / Format | 15% |
| Security (govulncheck) | 15% |
| Type Safety (go vet) | 10% |
| Complexity | 10% |
| Coverage | 5% |
| Architecture | 10% |

## Installation

```bash
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
go install golang.org/x/vuln/cmd/govulncheck@latest
```

Run the lint and format parts of the combined check in whatever shell syntax your environment supports.
