# DB Designer Constitution

<!--
  Sync Impact Report
  - Version change: 1.0.0
  - Modified principles: N/A (initial fill from template)
  - Added sections: Core Principles content, Additional Constraints, Development Workflow, Governance
  - Removed sections: None
  - Templates requiring updates:
    -  updated: .specify/templates/tasks-template.md
    -  updated: .specify/templates/plan-template.md (no change required)
    -  updated: .specify/templates/spec-template.md (no change required)
  - Follow-up TODOs:
    - TODO(RATIFICATION_DATE): original adoption date is unknown
-->

## Core Principles

### I. Code Quality Is Non-Negotiable

<!-- Example: I. Library-First -->

All production changes MUST:

- Keep code readable and maintainable (clear naming, small functions, cohesive modules).
- Avoid duplication unless justified; shared logic MUST be centralized.
- Use typed/public interfaces consistently (types/schemas/contracts) where the project supports it.
- Include robust error handling with actionable messages (no silent failures).
- Leave the area better than found: if you touch code, you MUST fix nearby lint/typing issues that
block understanding.
<!-- Example: Every feature starts as a standalone library; Libraries must be self-contained, independently testable, documented; Clear purpose required - no organizational-only libraries -->

### II. Testing Standards Define “Done”

<!-- Example: II. CLI Interface -->

Any change that alters behavior MUST be covered by tests.

- Unit tests MUST exist for pure logic and edge cases.
- Integration tests MUST exist for user journeys and cross-module boundaries.
- Bug fixes MUST add a regression test that fails before the fix.
- Tests MUST be deterministic and fast; slow or flaky tests MUST be fixed or quarantined with an
  explicit issue and ownership.
- CI (or local equivalent) MUST run tests and block merges on failures.
<!-- Example: Every library exposes functionality via CLI; Text in/out protocol: stdin/args → stdout, errors → stderr; Support JSON + human-readable formats -->

### III. UX Consistency Over Novelty

<!-- Example: III. Test-First (NON-NEGOTIABLE) -->

User-facing behavior MUST be consistent across the product.

- UI language, interaction patterns, and component behavior MUST follow established patterns in the
  repository (or be explicitly introduced as a new standard).
- Defaults MUST be sensible; error states MUST be clear and recoverable.
- Accessibility MUST be treated as a requirement (keyboard navigation, contrast, labels, etc.) for
  any new/changed UI.
- “Acceptance Scenarios” in specs MUST be kept in sync with actual behavior; if behavior changes,
the spec MUST be updated.
<!-- Example: TDD mandatory: Tests written → User approved → Tests fail → Then implement; Red-Green-Refactor cycle strictly enforced -->

### IV. Performance Budgets Are Requirements

<!-- Example: IV. Integration Testing -->

Performance regressions are treated as bugs.

- Any change on hot paths MUST include a measurement method (benchmark, profiling notes, or
  reproducible steps) and MUST not regress p95 latency/throughput beyond an agreed budget.
- Frontend interactions MUST remain responsive (avoid long main-thread blocks).
- Data operations MUST be scalable for expected sizes (avoid N+1, unbounded loops, and accidental
  quadratic behavior).
- Performance work MUST prioritize correctness first, then optimization with evidence.
<!-- Example: Focus areas requiring integration tests: New library contract tests, Contract changes, Inter-service communication, Shared schemas -->

### V. Small, Observable, Reversible Changes

<!-- Example: V. Observability, VI. Versioning & Breaking Changes, VII. Simplicity -->

Prefer incremental delivery.

- Changes MUST be scoped so they can be reviewed, tested, and reverted safely.
- Behavior-affecting changes MUST be observable (logs/metrics/events as appropriate) so failures can
  be detected quickly.
- Breaking changes MUST be explicit, documented, and include a migration/rollout plan.
<!-- Example: Text I/O ensures debuggability; Structured logging required; Or: MAJOR.MINOR.BUILD format; Or: Start simple, YAGNI principles -->

## Additional Constraints

<!-- Example: Additional Constraints, Security Requirements, Performance Standards, etc. -->

- All new dependencies MUST be justified (why needed, alternatives considered, expected impact).
- Security-sensitive changes MUST include threat considerations and safe defaults.
- Public APIs/exports MUST be documented at the boundary (inputs/outputs, errors, invariants).
<!-- Example: Technology stack requirements, compliance standards, deployment policies, etc. -->

## Development Workflow & Quality Gates

<!-- Example: Development Workflow, Review Process, Quality Gates, etc. -->

- Every change MUST have a clear owner and review.
- Reviews MUST verify: correctness, tests (added/updated), UX consistency, and performance impact.
- Refactors MUST not change behavior without explicit product approval and updated acceptance
  scenarios.
- If a principle is violated, it MUST be recorded in the plan/spec with a justification and a
follow-up task to remove the violation.
<!-- Example: Code review requirements, testing gates, deployment approval process, etc. -->

## Governance

<!-- Example: Constitution supersedes all other practices; Amendments require documentation, approval, migration plan -->

This constitution is the highest-level guidance for this repository.

- Amendments MUST be made via a committed change to this file, including rationale in the Sync
  Impact Report.
- Versioning follows SemVer:
  - MAJOR: principle removals or redefinitions; governance changes that loosen requirements.
  - MINOR: new principles/sections, or materially stronger requirements.
  - PATCH: clarifications and wording that do not change intent.
- Compliance is enforced by review: any PR that materially affects behavior MUST demonstrate
  compliance with the principles above (or include an explicit, time-boxed exception).

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE): original adoption date is unknown | **Last Amended**: 2026-04-05

<!-- Example: Version: 2.1.1 | Ratified: 2025-06-13 | Last Amended: 2025-07-16 -->
