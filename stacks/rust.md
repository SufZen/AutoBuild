# Rust Stack — Quality Configuration

> Pre-built quality config for Rust projects.

This preset still maps to AutoBuild's 7 canonical checks. Some checks use more than one command.

## Check Commands

| Check | Command | Expected Output |
| --- | --- | --- |
| **Tests** | `cargo test` | Exit code 0 |
| **Lint / Format** | `cargo clippy -- -D warnings && cargo fmt --check` | Exit code 0 |
| **Type Check** | `cargo check` | Exit code 0 |
| **Security** | `cargo audit` | No vulnerabilities |
| **Complexity** | `Manual review or project-specific tooling` | Complexity observations |
| **Coverage** | `cargo llvm-cov` | Coverage % |
| **Architecture** | `Manual review against project-context.md` | Compliance observations |

## Weights

| Check | Weight |
| --- | --- |
| Tests | 35% |
| Lint / Format (clippy + fmt) | 15% |
| Security (cargo audit) | 15% |
| Type Safety (cargo check) | 10% |
| Complexity | 10% |
| Coverage | 5% |
| Architecture | 10% |

## Installation

```bash
rustup component add clippy llvm-tools-preview
cargo install cargo-audit cargo-llvm-cov
```

If your shell does not support `&&`, replace the combined commands with the equivalent for your environment.
