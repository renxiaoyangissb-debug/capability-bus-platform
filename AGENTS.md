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
- Keep `docs/planning/work-buoy.yaml` synchronized at every resumable checkpoint: before dispatch, after a material commit or verified test milestone, before a long wait, after a correction request, and when entering or leaving a gate. It must identify the exact resume action and any in-flight or dirty state.
- Before another agent starts work, it must verify the repository HEAD/worktree values in the current-work ledger and claim only an unassigned, gate-permitted lane. A stale ledger or narrative is not completion evidence.
- Produce human-review reports under `docs/reviews/human-checkpoints/` using the development-progress naming rule defined there.
- Keep actual code changes in the owning repository.

## Usage and execution efficiency

- Follow `docs/governance/usage-efficiency-policy.md` for every Platform, Core, and Official UI dispatch.
- Default to one active implementation conversation. Parallel implementation requires an explicit user decision to trade additional quota for lower wall-clock latency.
- Use low reasoning for coordination, status, ledger, and mechanical verification; use medium reasoning for normal implementation. High reasoning is exceptional and must be justified by a specific unresolved architecture, security, or debugging problem.
- Keep one owning-project turn to a soft ceiling of 12 model/tool round trips. Batch related file reads, edits, and checks; at the ceiling, leave a compact dirty-state checkpoint instead of continuing an open-ended loop.
- Run focused tests while implementing. Run a complete project suite once for the owning-project candidate and once for Platform acceptance; do not repeat unchanged full suites at narrative milestones.
- Use event-driven completion waits. Do not repeatedly poll active conversations, request full histories with tool outputs, or replay large logs when a commit, concise final report, or targeted excerpt is sufficient.
- Treat token telemetry as an efficiency signal: cached input is still repeated context processing. Minimize the number of model samples even when prompt caching is effective.

## Safety

- Never commit secrets or credentials.
- Default to offline-safe operation and least privilege.
- Preserve user changes in every repository.
- Stop at the mandatory human checkpoints and hard stops defined in `docs/governance/platform-delegated-authority.md`.
