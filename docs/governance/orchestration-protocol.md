# Cross-project orchestration protocol

## Purpose

Coordinate Core and Official UI without merging their ownership domains or creating a third implementation authority.

## Authority

- Core contracts and runtime behavior: `capability-bus-core`.
- UI interaction and packaging: `capability-bus-official-ui`.
- Roadmap, freeze records, compatibility matrix, integration evidence: this repository.
- Source implementation and historical evidence: `personal-ai-control-plane`.
- Workflow authorization, scoped project-conversation dispatch, evidence verification, and routine phase progression: this repository, under [Platform delegated authority](platform-delegated-authority.md).

## Operating cycle

1. Read each repository's Git HEAD, worktree status, milestone file, and latest verified tests.
2. Compare evidence with the master development plan and active gate.
3. Produce and dispatch a narrowly scoped handoff instruction to the owning project when the active gate permits it.
4. On return, verify commits and tests instead of accepting narrative completion.
5. Write or update the progress-named human checkpoint report.
6. Advance routine phases automatically when objective evidence is complete and no mandatory human checkpoint applies.
7. Stop and request human direction only at a mandatory human checkpoint or hard stop.

## Freeze gates

### Delegated development authority

The user has approved the architecture and delegated in-scope workflow authorization to Platform. Platform can authorize routine T1–T5 work and project-conversation dispatch after prerequisite evidence is verified. The current source ownership/licensing question is a mandatory human checkpoint before any source-code extraction.

### F1 Contract Freeze

F1 requires a working Core alpha.1 loop, machine-readable contracts, examples, mock/fake Core, conformance tests, and a signed-off freeze record. Platform prepares and verifies the F1 report, then stops for human acceptance before UI business implementation starts.

### F2 Core Alpha API Freeze

F2 requires the complete alpha.3 Core, compatibility rules, security boundaries, replacement tests, and integration contract evidence. Platform prepares and verifies the F2 report, then stops for human acceptance. After F2 acceptance, alpha.4 accepts fixes and compatibility work rather than scope expansion.

## Hard stops

Stop and request human direction when:

- a change alters the fixed architecture;
- source ownership, reuse, or distribution authority is missing or ambiguous;
- source code or provenance cannot be verified;
- a project requests private database/API access across ownership boundaries;
- a destructive migration or broad permission expansion is required;
- a frozen contract needs a breaking change without versioning and migration;
- F1, F2, or release-candidate acceptance is required;
- credentials, production data, public network exposure, or another irreversible external effect requires owner action;
- verified evidence cannot resolve an architectural or safety conflict.
