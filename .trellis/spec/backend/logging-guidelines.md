# Logging Guidelines

> The observability conventions currently evidenced by Sunmao backend code.

---

## Current State

Sunmao has no runtime crate and no logging or telemetry dependency. There are no
application log statements, subscriber setup, structured fields, trace context,
metrics, or production output configuration. The project therefore has no
established logging library, schema, or level policy.

Terminal messages emitted by generated `.trellis/scripts/` are development-tool
output and are not evidence for the backend's observability design.

## Log Levels

No debug, info, warning, or error level semantics are defined. The task that
introduces runtime logging must choose levels from concrete operational events
and document representative call sites. Do not select a library or create a
global facade only to fill this document.

## Structured Logging

No structured log contract exists. In particular, there are no established
field names for request, workflow, state-machine, tenant, node, or trace
identifiers. Define those fields alongside the runtime boundary that produces
them, then add the repeated schema here after it is implemented and tested.

## What To Log

There is no repository-backed event inventory yet. The first observability work
should identify the events operators need to diagnose that feature, including
the lifecycle boundary and failure behavior, without claiming a global policy.

## Sensitive Data

The repository does not yet define configuration, credentials, payloads, or a
field-level redaction policy. Runtime logging must not expose secrets or raw
credentials. Once those data types exist, this section must name the sensitive
fields and point to the code that redacts or omits them.

## Evidence Needed To Establish A Convention

Update this guide when logging is introduced, with at least:

- the dependency and initialization path;
- two representative event call sites at different levels;
- the structured fields shared between them;
- a test or captured output that verifies filtering or redaction.
