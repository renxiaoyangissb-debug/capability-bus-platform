# Capability Bus Platform instructions

These instructions apply to the whole coordination repository.

## Role

- This repository is the coordination and release-governance layer for `capability-bus-core` and `capability-bus-official-ui`.
- It does not own runtime code, Core authoritative state, UI implementation, or plugin implementation.
- Keep the existing Capability Bus architecture unchanged.
- Treat the Core repository as authoritative for contracts and runtime semantics.
- Treat the Official UI repository as authoritative for interaction, accessibility, and UI plugin packaging.

## Development gate

- The project is in preparation and human-review state.
- Do not start formal implementation, create implementation tasks, modify sibling repositories, or declare a milestone active until the user explicitly approves development.
- Read `docs/planning/master-development-plan.md`, `docs/governance/orchestration-protocol.md`, and `docs/reviews/development-readiness-review.md` before proposing work.
- Preserve F1 Contract Freeze and F2 Core Alpha API Freeze gates.
- UI business implementation must not start before F1 evidence is accepted.

## Reuse rule

- Existing general-purpose implementation must be extracted from `personal-ai-control-plane`, with source commit, path, hash, tests, and adaptation rationale recorded.
- “Reading old code and rewriting from memory” is not reuse.
- Product-specific RSS, Hermes, Review, and SiYuan behavior must not enter the bare Core.

## Coordination

- Record cross-project status using verified Git commits and test evidence.
- Do not treat another conversation's narrative as proof of completion.
- Sending work to another Codex conversation requires explicit user authorization.
- Keep actual code changes in the owning repository.

## Safety

- Never commit secrets or credentials.
- Default to offline-safe operation and least privilege.
- Preserve user changes in every repository.
- Stop at architectural conflicts, unverifiable source state, destructive migrations, or missing human approval.
