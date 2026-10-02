# Capability Bus Platform instructions

These instructions apply to the whole coordination repository.

## Role

- This repository is the coordination and release-governance layer for `capability-bus-core` and `capability-bus-official-ui`.
- It does not own runtime code, Core authoritative state, UI implementation, or plugin implementation.
- Keep the existing Capability Bus architecture unchanged.
- Treat the Core repository as authoritative for contracts and runtime semantics.
- Treat the Official UI repository as authoritative for interaction, accessibility, and UI plugin packaging.

## Development gate

- The user delegates workflow authorization and cross-project task dispatch to this Platform repository under `docs/governance/platform-delegated-authority.md`.
- Platform may automatically authorize, create, continue, and verify owning-project work that stays within the approved architecture, current roadmap, active gate, and repository ownership boundary.
- Do not wait for per-task user approval unless a mandatory human checkpoint or hard stop in the delegated-authority policy applies.
- Actual implementation changes remain in the owning repository and owning project conversation.
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
- Platform's recorded delegated authority is sufficient to create and message Core/UI project conversations within the active roadmap and gate; do not infer authority for unrelated projects or out-of-scope work.
- Keep `docs/planning/current-work.md` current whenever work is authorized, dispatched, completed, rejected, corrected, or stopped at a gate. It is the human-readable handoff ledger; `projects.yaml` remains the machine-readable state.
- Before another agent starts work, it must verify the repository HEAD/worktree values in the current-work ledger and claim only an unassigned, gate-permitted lane. A stale ledger or narrative is not completion evidence.
- Produce human-review reports under `docs/reviews/human-checkpoints/` using the development-progress naming rule defined there.
- Keep actual code changes in the owning repository.

## Safety

- Never commit secrets or credentials.
- Default to offline-safe operation and least privilege.
- Preserve user changes in every repository.
- Stop at the mandatory human checkpoints and hard stops defined in `docs/governance/platform-delegated-authority.md`.
