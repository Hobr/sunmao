# Error Handling

> The error contracts currently evidenced by Sunmao backend code.

---

## Current State

There is no Sunmao Rust crate or runtime code yet, so the project has no custom
error type, propagation convention, retry policy, public API error envelope, or
mapping from internal failures to client responses. `Cargo.toml` also has no
workspace dependencies from which an error library choice could be inferred.

Generated Python code under `.trellis/` belongs to the workflow runtime and must
not be used as the error-handling model for the Rust backend.

## Error Types

No shared error enum or wrapper is established. Do not add an `AppError`, error
trait hierarchy, or catch-all conversion before concrete callers show that the
abstraction is useful. The first implementation should keep errors at the
narrowest meaningful boundary and update this document once repeated patterns
exist.

## Propagation And Recovery

The repository does not yet define when to propagate, translate, retry, or
terminate on an error. When runtime code introduces these decisions, document
them with real paths covering at least:

- one originating error and its context;
- one boundary where the error is translated or exposed;
- the observable behavior asserted by a test.

Until those examples exist, review error behavior against the feature's own
requirements rather than an assumed global policy.

## API Error Responses

No HTTP, RPC, CLI, worker, or message-processing interface exists, so there is
no standard status-code mapping or response schema. A public interface task must
define its error contract explicitly and add compatibility tests before this
section can describe a project-wide format.

## Unsupported Assumptions

- Do not infer an API protocol from the README's high-level project description.
- Do not treat `anyhow`, `thiserror`, or another crate as selected; none appears
  in `[workspace.dependencies]`.
- Do not log and discard an error merely because no shared error type exists.
- Do not cite `.trellis/scripts/` behavior as application error handling.
