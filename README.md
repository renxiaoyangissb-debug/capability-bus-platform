# Capability Bus Platform

Coordination and release-governance project for:

- `capability-bus-core`
- `capability-bus-official-ui`

This repository contains the overall roadmap, contract-freeze gates, compatibility records, review checklists, and cross-project release evidence. It is not a runtime dependency and contains no Core or UI implementation.

## Current status

**F1 ACCEPTED — CORE T3 AND UI U2 VERIFIED**

The user accepted Core `78127bb9192aa6ddf8f15b544c553c149415c5a5` as the F1 Contract Freeze baseline. Platform independently verified Core T3 at `d3556226ec6e8221781e4267ca2aa0b6ca523ebd` and Official UI U2 at `f17320cf39f18d507a4e40b0f7047b88a14582ae`. The current explicit dispatch stops before Core T4, UI U3, and F2. F2 remains the next mandatory human checkpoint after its prerequisites are completed.

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
