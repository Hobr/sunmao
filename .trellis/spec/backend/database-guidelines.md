# Database Guidelines

> The persistence conventions currently evidenced by the repository.

---

## Current State

Sunmao has no persistence implementation yet. The workspace has no Rust crates,
database dependencies, schema files, migrations, query code, or database tests.
Consequently, the repository has not selected an ORM or query library and has
no established transaction, table, column, index, or migration convention.

The relevant manifest is intentionally empty:

```toml
# Cargo.toml
[workspace.dependencies]
```

This is evidence of an unmade decision, not permission to assume SQLx, Diesel,
SeaORM, an embedded store, or a particular database engine.

## Query Patterns

No query pattern exists. Do not introduce a generic repository layer, database
trait, connection pool wrapper, or transaction helper solely to conform to an
imagined project architecture. A persistence feature should establish only the
abstractions its concrete use cases require.

## Migrations

There is no migration directory or migration command. The first task that adds
persistent schema must define and verify:

- the database engine and Rust integration;
- the canonical migration location and naming format;
- how migrations are created, applied, and rolled back;
- whether startup applies migrations or deployment runs them separately;
- how schema changes are tested.

Record the resulting commands and at least two real migration or query paths in
this file as part of that same task.

## Naming And Transactions

Table, column, constraint, index, and transaction-boundary conventions are not
yet established. Keep names native to the selected database and domain during
the first implementation, then document the repeated pattern rather than
inventing one in advance.

## Unsupported Assumptions

- Do not cite generated `.trellis/` storage as application persistence.
- Do not add a database dependency only to satisfy this specification.
- Do not claim migrations are covered by `just test`; there are currently no
  database tests or migration recipes.
- Do not describe a naming or transaction rule without a repository path that
  demonstrates it.
