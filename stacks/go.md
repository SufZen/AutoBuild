# Go Stack — Quality Configuration

> Pre-built quality config for Go projects.

## Check Commands

| Check | Command | Expected Output |
| --- | --- | --- |
| **Tests** | `go test ./...` | Exit code 0 |
| **Lint** | `golangci-lint run` | Exit code 0 |
| **Format** | `gofmt -l .` | No output (all files formatted) |
| **Type Check** | `go vet ./...` | Exit code 0 |
| **Security** | `govulncheck ./...` | No vulnerabilities |
| **Coverage** | `go test -coverprofile=coverage.out ./... && go tool cover -func=coverage.out` | Coverage % |

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
