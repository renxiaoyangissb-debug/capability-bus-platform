# Capability Bus Platform

Coordination and release-governance project for:

- `capability-bus-core`
- `capability-bus-official-ui`

This repository contains the overall roadmap, contract-freeze gates, compatibility records, review checklists, and cross-project release evidence. It is not a runtime dependency and contains no Core or UI implementation.

## Current status

**T5 / ALPHA.4 RELEASE CANDIDATE ACCEPTED**

Platform independently verified Core `4858520224a98b6528cb6cf47e7aa2e517bb62dc` and Official UI `d77a9a92f7abd435c3d5447db8179e4325f3e79d` as the exact T5 / alpha.4 pair. On 2026-10-03, the user replied `接受发布候选`; the decision and limitations are recorded in the [release-candidate human review](docs/reviews/human-checkpoints/T5-release-candidate-human-review.md). No public upload or deployment is implied.

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
