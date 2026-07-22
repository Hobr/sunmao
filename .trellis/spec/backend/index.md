# Backend Development Guidelines

> Repository-backed guidance for Sunmao backend work.

---

## Current Baseline

Sunmao is currently an empty Rust 1.97.1 workspace scaffold. These guides
separate conventions already enforced by checked-in configuration from runtime
decisions that have not yet been made. Do not turn an explicitly unestablished
area into a generic convention.

## Guidelines Index

| Guide | Description | Status |
|---|---|---|
| [Directory Structure](./directory-structure.md) | Current workspace layout and package-boundary evidence | Scaffold baseline documented |
| [Database Guidelines](./database-guidelines.md) | Persistence decisions and evidence still missing | No runtime convention yet |
| [Error Handling](./error-handling.md) | Error contracts and boundaries still missing | No runtime convention yet |
| [Quality Guidelines](./quality-guidelines.md) | Toolchain, commands, checks, and known gaps | Enforced baseline documented |
| [Logging Guidelines](./logging-guidelines.md) | Logging and observability decisions still missing | No runtime convention yet |

## Pre-Development Checklist

For every Rust backend task:

1. Read [Directory Structure](./directory-structure.md) and
   [Quality Guidelines](./quality-guidelines.md).
2. Read the database, error, or logging guide when the task touches that
   boundary.
3. Check the guide's **Current State** before selecting a library or abstraction;
   an unestablished convention is a design decision for the task, not a default.
4. Search the current repository before changing a shared value or adding a
   helper.
5. When implementation establishes a repeated pattern, update the corresponding
   guide with real source and test paths in the same change.

Also read `.trellis/spec/guides/index.md` and follow the applicable shared
thinking guide for cross-layer changes or potential reuse.

## Quality Check

Before committing a backend change:

1. Run the relevant root `just` recipe from
   [Quality Guidelines](./quality-guidelines.md).
2. Run `git diff --check` and confirm that changed paths are covered by the
   intended workspace or pre-commit checks.
3. Re-read the affected guide and verify that every claimed convention has a
   real repository path or is explicitly labeled as unestablished.
4. For a new runtime boundary, add behavior tests and update the corresponding
   database, error, or logging guide with source paths before declaring the
   task complete.

This scaffold has no Cargo workspace members or runtime tests. The workspace
commands currently fail because Cargo has no package or formatting target, and
pre-commit skips Rust-specific hooks because there are no tracked `.rs` files.
The first crate task must turn the applicable commands into real quality gates.

## Documentation Standard

- Write all specification documentation in English.
- Document current behavior, including known gaps, rather than aspirational
  architecture.
- Cite repository paths and commands. Never use generated `.trellis/scripts/`
  as evidence for Sunmao runtime conventions.
- Replace scaffold-state sections only after application code supplies real
  examples.
