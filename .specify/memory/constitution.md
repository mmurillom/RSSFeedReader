<!--
Sync Impact Report
- Version change: none → 1.0.0
- Modified principles: (new) Security-First (NON-NEGOTIABLE); Test-First & Testable; Maintainability & Simplicity; Observability & Metrics; Versioning, Compatibility & Quality Gates
- Added sections: Additional Constraints; Development Workflow
- Removed sections: none
- Templates updated: .specify/templates/plan-template.md ✅ updated; .specify/templates/spec-template.md ✅ updated; .specify/templates/tasks-template.md ✅ updated; .specify/templates/checklist-template.md ⚠ pending (no direct changes required)
- Follow-up TODOs: None
-->

# RSSFeedReader Constitution

## Core Principles

### I. Security-First (NON-NEGOTIABLE)
- Every feature that accepts external input or adds network/persistence capability MUST include a brief threat assessment and explicit mitigation steps.
- Secrets MUST NOT be committed to source; use environment variables or secret stores for runtime configuration.
- No network calls or external data fetching are allowed in the MVP surface. Any feature that introduces network access MUST be approved in a minor/major amendment and accompanied by tests and a migration plan.
- All security-sensitive events (e.g., failed authentication, feed fetch failures when enabled) MUST be logged in structured form for later analysis.

**Rationale:** Even a simple demo app can introduce sensitive behavior later. Early threat-modeling and explicit constraints reduce rework and user risk.

### II. Test-First & Testable (NON-NEGOTIABLE)
- Tests MUST be written before feature implementation (unit tests, contract tests, and integration tests where appropriate). Tests MUST fail before implementation and pass in CI before merging.
- Each user story MUST include at least one automated test that demonstrates the primary acceptance criterion (e.g., adding a subscription updates the list).
- Contract tests MUST be added for any API surface that other components depend on.

**Rationale:** Enforcing a Test-First discipline prevents regressions and keeps the MVP reliably demonstrable.

### III. Maintainability & Simplicity (MUST)
- Prefer small, well-documented modules with single responsibility. Use clear interfaces and dependency injection where appropriate.
- Follow the project's language-specific style and analyzer rules (e.g., .NET analyzers). Code MUST include comments for non-obvious decisions and public APIs MUST be documented.
- Avoid premature optimization and unnecessary features (YAGNI). Keep the MVP scope minimal: add subscriptions in-memory; defer persistence and fetching to Extended-MVP.

**Rationale:** Small, well-scoped changes reduce cognitive load and accelerate iteration.

### IV. Observability & Metrics (SHOULD)
- Implement structured logging and minimal observability hooks (log level control, correlation IDs where applicable) even in the MVP.
- Define measurable success criteria for features (e.g., Subscriptions added per minute in tests, UI response times) and include basic assertions in tests.

**Rationale:** Early observability makes debugging and validation faster and informs future performance work.

### V. Versioning, Compatibility & Quality Gates (MUST)
- The project uses semantic versioning for released artifacts. Major version bumps are REQUIRED for breaking changes that impact consumers or workflows.
- Every PR MUST include: a short description, tests, a changelog entry (if behavior changes), and CI green status (linters, analyzers, tests).
- Breaking changes MUST be accompanied by a migration plan and an explicit governance approval (see Governance section).

**Rationale:** Explicit versioning and gates preserve compatibility and make releases predictable.

## Additional Constraints
- **Technology Stack (MUST):** Backend: ASP.NET Core; Frontend: Blazor WebAssembly. The tech stack from `StakeholderDocuments/TechStack.md` is considered authoritative for stack choices.
- **MVP Constraints:** The MVP runs locally for a single user, stores subscriptions in memory, and does not fetch or parse feeds.
- **Cross-Platform:** Development and testing MUST work on Windows, macOS, and Linux.
- **Data & Privacy:** No personal or sensitive data is collected in the MVP. Any future data persistence MUST document retention, access control, and encryption-at-rest choices.

## Development Workflow
- **PR Requirements (MUST):** All changes MUST be submitted as a Pull Request, reference an issue/spec, include tests, and pass CI checks.
- **Review Process (MUST):** At least one reviewer (preferably a maintainer) MUST approve non-trivial changes. Security-sensitive or breaking changes require two approvals and a documented migration plan.
- **CI Gates (MUST):** CI MUST run the test suite, static analysis, formatting checks, and a basic security scan (e.g., dependency vulnerability checks) before merges.
- **Issue-to-PR Traceability:** Every PR MUST reference an issue or spec that outlines the acceptance criteria.

## Governance
- **Amendments:** Propose changes to this constitution via a documented PR targeting `.specify/memory/constitution.md`. The PR MUST explain the rationale, the version bump proposal, and a migration plan for breaking changes.
- **Approval Rules:** Non-breaking editorial changes (typos, clarifications) MAY be approved by one maintainer and result in a PATCH bump. Additive changes (new principles or procedural changes) REQUIRE at least one maintainer approval and SHOULD be a MINOR bump. Breaking governance changes REQUIRE two maintainers and a MAJOR bump.
- **Versioning Policy:** Follow semantic versioning (MAJOR.MINOR.PATCH). Update `**Version**` and `**Last Amended**` on each accepted amendment. `RATIFICATION_DATE` is the date this document was first adopted.
- **Compliance Review:** Conduct a light compliance review at each sprint/major milestone to confirm the project still meets the core principles.

**Version**: 1.0.0 | **Ratified**: 2026-01-30 | **Last Amended**: 2026-01-30
