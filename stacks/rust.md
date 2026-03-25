# Rust Stack — Quality Configuration

> Pre-built quality config for Rust projects.

## Check Commands

| Check | Command | Expected Output |
| --- | --- | --- |
| **Tests** | `cargo test` | Exit code 0 |
| **Lint** | `cargo clippy -- -D warnings` | Exit code 0 |
| **Format** | `cargo fmt --check` | Exit code 0 |
| **Type Check** | `cargo check` | Exit code 0 |
| **Security** | `cargo audit` | No vulnerabilities |
| **Coverage** | `cargo tarpaulin --out Stdout` | Coverage % |

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
rustup component add clippy
cargo install cargo-audit cargo-tarpaulin
```
