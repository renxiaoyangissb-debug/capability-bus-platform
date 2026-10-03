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

## Low-consumption execution profile

The default profile optimizes successful work per unit of quota, not maximum concurrency.

1. Activate only one owning-project implementation lane at a time. Keep dependent lanes blocked until their public contract baseline is independently verified.
2. Send a short handoff that references authoritative repository documents instead of embedding the complete program history.
3. Use `low` reasoning for Platform coordination and read-only checks, and `medium` for Core/UI implementation. Escalate a single turn to `high` only for a named hard problem, then return to the default.
4. Batch discovery into one read pass, implementation into a small number of coherent patches, and verification into focused checks followed by one full candidate suite.
5. Use a soft ceiling of 12 model/tool round trips per owning-project turn. If unfinished, record exact HEAD, dirty paths, completed scope, and the next command before stopping.
6. Wait once for completion or a required-user-action event. Do not use frequent status polling or repeatedly read complete thread histories and command outputs.
7. Platform independently reruns the full suite once per candidate. A correction reruns the failed slice first and the full suite once after the correction is ready.
8. Update ledgers at resumable state transitions, material commits/test milestones, corrections, and gates. Do not create separate commits for commentary-only progress.

The current measured baseline and rationale are recorded in [the 2026-10-03 usage-efficiency audit](../reviews/usage-efficiency-audit-2026-10-03.md).

## Freeze gates

### Delegated development authority

The user has approved the architecture and delegated in-scope workflow authorization to Platform. Platform can authorize routine T1–T5 work and project-conversation dispatch after prerequisite evidence is verified. The source ownership/reuse/distribution checkpoint, F1, and F2 have been accepted. The next planned mandatory human acceptance is the release candidate.

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
