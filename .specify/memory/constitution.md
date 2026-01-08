<!--
Sync Impact Report
- Version change: 1.0.0 → 1.1.0
- Modified principles: I. Focus on Consensus-Building → expanded with consensus-over-code rule
- Added sections: Engineering Standards; Session Design & Interop
- Removed sections: none
- Templates requiring updates:
  - .specify/templates/plan-template.md (⚠ pending): Add Constitution Check gates per principles
  - .specify/templates/spec-template.md (✅ aligned): No mandatory changes detected
  - .specify/templates/tasks-template.md (⚠ pending): Align testing guidance with Engineering Standards
  - .specify/templates/commands/ (⚠ pending): Verify command docs reference constitution generically
- Follow-up TODOs:
  - TODO(RATIFICATION_DATE): Set original adoption date
  - TODO(GOVERNANCE_RULES): Define amendment/versioning/compliance procedures
-->

# fractalgram2 Constitution

## Core Principles

### I. Focus on Consensus-Building
This application exists to facilitate building consensus on what to do. It does not
determine which session is authoritative within any particular organization. The
product surface and data model MUST optimize for running effective consensus sessions
and exporting their results, not adjudicating authority or executing decisions.

- People determine consensus, not code. Code just helps it.
  - If the code says one thing but the consensus of the people in the room prefers the other,
    the consensus of the people is right; it is always right.
  - This enables dealing with edge cases like technical difficulties in an efficient
    way, and prevents the app from having too much power.

### II. Benevolent Dictator Within Sessions; Decentralization via Easy Forking
Each session designates a single facilitator with control over session flow (e.g.,
advance/rewind steps) but NOT the outcomes themselves. Decentralization is achieved
by allowing anyone run alternative sessions at any time.

### III. Limit Trust in the Backend
Prefer client-side processing for computations and decision logic. The backend SHOULD
primarily store and relay data. Sensitive or verifiable computations SHOULD be
performed client-side where feasible. Protocols MUST be designed to minimize reliance
on privileged backend correctness beyond storage and transport.

### IV. Do One Thing, Do It Well
Each feature MUST have a single, clear purpose, with simple, composable boundaries.
Avoid unnecessary abstraction and configuration. Prefer small, cohesive modules over
broad frameworks. Remove or split features when they exceed a single responsibility.

### V. Assume Interoperability: Results Are Needed by Other Apps
Session outputs MUST be designed for consumption by other systems. Provide stable,
well-documented schemas and export formats. Backward-compatible changes are preferred;
breaking changes MUST include versioned artifacts and clear migration paths.

## Engineering Standards

- **Code Quality**: Enforce linting, formatting, and static analysis. Code MUST be
  readable and maintainable, with clear ownership and small, cohesive modules.
- **Testing Standards**: Critical user journeys and core logic MUST have automated
  tests. Where practical, write tests before or alongside code. Each user story MUST
  be independently testable. Contract/interface changes MUST include corresponding
  tests.
- **User Experience Consistency**: Apply a consistent design system (components,
  spacing, typography). Accessibility and responsiveness are REQUIRED. Flows MUST be
  predictable and discoverable.
- **Performance (Smooth UX)**: Aim for perceptually smooth interactions. Targets:
  p95 interaction latency < 100ms for UI updates; initial meaningful paint within
  2s on a typical device/network; avoid main-thread long tasks (>50ms). Measure,
  budget, and regress only with justification.

## Governance

TODO(GOVERNANCE_RULES): Governance intentionally deferred per request. Define
amendment procedure, semantic versioning policy, and compliance review later.

**Version**: 1.1.0 | **Ratified**: TODO(RATIFICATION_DATE) | **Last Amended**: 2026-01-08
