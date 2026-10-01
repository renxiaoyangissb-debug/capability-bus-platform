# Cross-project orchestration protocol

## Purpose

Coordinate Core and Official UI without merging their ownership domains or creating a third implementation authority.

## Authority

- Core contracts and runtime behavior: `capability-bus-core`.
- UI interaction and packaging: `capability-bus-official-ui`.
- Roadmap, freeze records, compatibility matrix, integration evidence: this repository.
- Source implementation and historical evidence: `personal-ai-control-plane`.

## Operating cycle

1. Read each repository's Git HEAD, worktree status, milestone file, and latest verified tests.
2. Compare evidence with the master development plan and active gate.
3. Produce a narrowly scoped handoff instruction for the owning project.
4. Wait for explicit user authorization before messaging another conversation or starting a new development phase.
5. On return, verify commits and tests instead of accepting narrative completion.
6. Update the gate record only when its objective evidence is complete.

## Freeze gates

### Human review

No formal development begins until the user reviews the prepared repositories and explicitly authorizes development.

### F1 Contract Freeze

F1 requires a working Core alpha.1 loop, machine-readable contracts, examples, mock/fake Core, conformance tests, and a signed-off freeze record. UI business implementation starts only after F1 acceptance.

### F2 Core Alpha API Freeze

F2 requires the complete alpha.3 Core, compatibility rules, security boundaries, replacement tests, and integration contract evidence. After F2, alpha.4 accepts fixes and compatibility work rather than scope expansion.

## Hard stops

Stop and request human direction when:

- a change alters the fixed architecture;
- source code or provenance cannot be verified;
- a project requests private database/API access across ownership boundaries;
- a destructive migration or broad permission expansion is required;
- a frozen contract needs a breaking change without versioning and migration;
- formal development has not been explicitly approved.
