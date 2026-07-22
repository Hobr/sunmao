# Directory Structure

> The current Rust workspace layout and the boundaries that are actually present.

---

## Current State

As of 2026-07-22, this repository is a Rust workspace scaffold. `Cargo.toml`
declares a virtual workspace, but `workspace.members` is empty and there is no
`src/`, `crates/`, application package, route, service, or test tree yet.

Do not infer a crate layout or module architecture from the generated
`.trellis/` tooling. It supports the development workflow; it is not Sunmao
backend code.

## Repository Layout

```text
sunmao/
|-- Cargo.toml                 # Workspace metadata and shared dependencies
|-- rust-toolchain.toml        # Pinned compiler, components, and WASI target
|-- flake.nix                  # Reproducible development shell
|-- justfile                   # Workspace-level developer commands
|-- deny.toml                  # Dependency policy
|-- .rustfmt.toml              # Rust formatting configuration
|-- .editorconfig              # Cross-file whitespace rules
|-- .pre-commit-config.yaml    # Repository checks
|-- .github/dependabot.yml     # Cargo, Nix, and Actions updates
|-- .trellis/                  # AI workflow metadata and specifications
`-- README.md                  # Project summary and development commands
```

Generated and machine-local paths such as `target/`, `.cargo/`, `.direnv/`,
and `.worktrees/` are ignored by `.gitignore` and are not source locations.

## Workspace Organization

The only established package boundary is the Cargo workspace root:

```toml
# Cargo.toml
[workspace]
members = [
]
resolver = "3"
```

When the first Rust crate is introduced, it must be added to
`workspace.members`; otherwise the existing `cargo ... --workspace` recipes do
not build, check, test, or document it. The repository does not yet establish
whether crates belong at the root, under `crates/`, or under another directory.
Choose that layout as part of the first implementation design, then update this
file with real module paths and examples in the same change.

Shared package metadata already belongs in `[workspace.package]`, and the root
has an empty `[workspace.dependencies]` table ready for dependencies shared by
multiple workspace members. There is not yet enough code to establish whether
all dependencies must be centralized there.

## Naming And Formatting

- Rust code targets edition 2024 and Rust 1.97.1, as declared in `Cargo.toml`.
- Rust source is formatted by rustfmt with a maximum width of 100 characters.
- Files use UTF-8, LF line endings, spaces, a final newline, and no trailing
  whitespace. `.editorconfig` uses four-space indentation by default and two
  spaces for Nix, Markdown, and YAML.
- No project-specific crate, module, route, service, or test naming convention
  exists yet. Follow Rust's compiler-enforced naming rules locally, but do not
  describe a broader naming scheme as a Sunmao convention until examples exist.

## Repository-Backed Examples

Workspace-wide commands are intentionally rooted at the repository:

```just
build:
    cargo build --workspace

check:
    cargo check --workspace
```

With no workspace member, these recipes currently fail because Cargo cannot
find a package. Registering the first crate must make the applicable workspace
recipes executable.

The pinned toolchain includes the compiler tools used by those commands and the
`wasm32-wasip2` target. See `rust-toolchain.toml` and `flake.nix` for the two
matching declarations.
