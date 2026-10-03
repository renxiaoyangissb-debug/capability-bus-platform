# Capability Bus Platform

Coordination and release-governance project for:

- `capability-bus-core`
- `capability-bus-official-ui`

This repository contains the overall roadmap, contract-freeze gates, compatibility records, review checklists, and cross-project release evidence. It is not a runtime dependency and contains no Core or UI implementation.

## Current status

**F2 HUMAN ACCEPTANCE REQUIRED — CORE T4 AND UI U3 VERIFIED**

The user accepted Core `78127bb9192aa6ddf8f15b544c553c149415c5a5` as the F1 Contract Freeze baseline. Platform independently verified corrected Core T4 at `272f6325506c4ce7c4de5c100fb3f03a28701318` and Official UI U3 at `39b5904499c0ecfa0dff1d57683812c150e377e4`. The complete Alpha management contract and controlled-write UI evidence are ready for the mandatory F2 human checkpoint. No T5 integration work may begin before explicit F2 acceptance.

The saved plugin integration guide is [docs/guides/plugin-integration-interface-v0.1.md](docs/guides/plugin-integration-interface-v0.1.md).

## Required reading

1. `docs/planning/master-development-plan.md`
2. `docs/governance/orchestration-protocol.md`
3. `docs/reviews/development-readiness-review.md`
4. `docs/planning/current-work.md`
5. `docs/planning/work-buoy.yaml`
6. `projects.yaml`

`docs/planning/current-work.md` is the live, human-readable handoff and backlog ledger. `docs/planning/work-buoy.yaml` is the compact interruption/recovery checkpoint. `projects.yaml` is the machine-readable program and gate state.

## Architecture diagrams

- `docs/architecture/everything-as-plugin-v2.1-original-reference.drawio`: unchanged original architectural-intent reference; not an implementation contract.
- `docs/architecture/capability-bus-alpha-optimized-topology.drawio`: implementation topology aligned with the current Core Alpha and Official UI Plugin design baselines.
- `docs/architecture/README.md`: provenance, authority and diagram change rules.

## Project layout

```text
capability-bus-platform/
├── AGENTS.md
├── README.md
├── projects.yaml
└── docs/
    ├── governance/orchestration-protocol.md
    ├── governance/platform-delegated-authority.md
    ├── guides/plugin-integration-interface-v0.1.md
    ├── architecture/
    │   ├── README.md
    │   ├── everything-as-plugin-v2.1-original-reference.drawio
    │   └── capability-bus-alpha-optimized-topology.drawio
    ├── planning/master-development-plan.md
    ├── reviews/development-readiness-review.md
    └── reviews/human-checkpoints/
```
