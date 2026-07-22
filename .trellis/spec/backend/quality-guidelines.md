# Quality Guidelines

> Repository-backed quality gates for Rust backend work.

---

## Toolchain Baseline

- Use Rust 1.97.1 with edition 2024 and Cargo resolver 3, as pinned by
  `rust-toolchain.toml` and `Cargo.toml`.
- Keep Rust source within rustfmt's 100-character maximum width.
- Use the root `justfile` recipes so checks apply to the whole workspace.
- Run commands from the repository root; the recipes and configuration files
  are rooted there.

The Nix development shell and `rust-toolchain.toml` both provide rustfmt,
Clippy, rust-analyzer, Cargo, Rust source, LLVM tools, and the
`wasm32-wasip2` target. `flake.nix` additionally provides protobuf, pre-commit,
`just`, and `cargo-binstall`.

## Authoritative Commands

| Purpose | Command | Repository definition |
|---|---|---|
| Debug build | `just build` | `cargo build --workspace` |
| Release build | `just release` | `cargo build --workspace --release` |
| Type/build check | `just check` | `cargo check --workspace` |
| Format and hooks | `just fmt` | `cargo fmt --all`, then all pre-commit hooks |
| Test | `just test` | `cargo nextest run --workspace --all-features` |
| Coverage | `just coverage` | `cargo llvm-cov nextest --workspace --html` |
| Documentation | `just doc` | `cargo doc --workspace --no-deps` |
| Dependency policy | `cargo deny check` | `deny.toml` |

The workspace currently has no members or tests. `just check`, `just test`, and
`just fmt` all fail because Cargo cannot find a package or formatting target;
the first crate task must make the applicable workspace commands executable.

## Required Patterns

- Register every new crate in `workspace.members` so workspace-wide commands
  include it.
- Format Rust with the repository rustfmt configuration; do not hand-format
  around the 100-character limit.
- Add tests for observable behavior introduced by a change. Once tests exist,
  `just test` is the repository-level runner.
- Keep dependencies compatible with `cargo deny check`. The current policy
  allows crates.io as the registry, permits workspace crates, warns on duplicate
  versions and unknown sources, and has no allowed Git sources.
- Update the relevant `.trellis/spec/backend/` guide when a change establishes
  the first real architecture, database, error, or logging convention.

## Repository Hygiene

Pre-commit enforces trailing-whitespace cleanup, final newlines, no byte-order
marks, case-conflict and merge-conflict checks, valid YAML, consistent line
endings, and a large-file guard. For Rust files it also configures rustfmt,
`cargo deny`, `cargo check`, Clippy, and nextest checks.

`.editorconfig` requires UTF-8 and LF line endings. It uses four-space
indentation by default and two spaces for Nix, Markdown, and YAML.

## Known Scaffold Gaps

These are current repository facts, not conventions to copy:

- `just check`, `just test`, and `just fmt` cannot complete while
  `workspace.members` is empty. A passing pre-commit run currently skips all
  Rust-specific hooks because there are no tracked `.rs` files.
- `.pre-commit-config.yaml` invokes `just clippy`, but `justfile` does not
  currently define a `clippy` recipe. Rust-source changes will expose this
  mismatch when that hook runs.
- `deny.toml` is largely the generated template. Its license allow-list is
  empty, so do not claim a project dependency-license policy beyond the values
  explicitly configured there.
- `.gitignore` ignores `Cargo.lock`; the repository has not yet established
  whether its future workspace members are libraries, binaries, or both.
- There are no CI workflow files, source files, or tests yet. Pre-commit is the
  only checked-in validation hook; Dependabot is limited to dependency update
  scheduling.

Resolve a gap in the task that first depends on it, and update this guide with
the resulting repository-backed convention.

## Testing Requirements

The checked-in test command is `just test`, which runs nextest for the workspace
with all features. Feature-specific tasks may run narrower commands during
iteration. The repository does not yet contain enough code to establish a
project-specific rule about test shape, fixtures, or required coverage.

No unit/integration directory layout, mocking pattern, fixture strategy, or
coverage threshold exists yet. Do not invent one until the codebase provides
real examples.

## Review Checklist

- The changed crate is included in workspace-level build, check, and test runs.
- Formatting and `.editorconfig` rules hold.
- New behavior has a test with a meaningful failure condition.
- Dependency additions pass the configured Cargo policy and are placed at the
  correct workspace/package scope.
- Public contracts, state transitions, failure paths, and compatibility impact
  are explicit in code and tests.
- Documentation describes only conventions demonstrated by repository paths.
