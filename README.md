# Capability Bus Platform

Coordination and release-governance project for:

- `capability-bus-core`
- `capability-bus-official-ui`

This repository contains the overall roadmap, contract-freeze gates, compatibility records, review checklists, and cross-project release evidence. It is not a runtime dependency and contains no Core or UI implementation.

## Current status

**PLATFORM WORKFLOW AUTHORITY ACTIVE — WAITING FOR SOURCE-RIGHTS CONFIRMATION**

The user approved the architecture and delegated in-scope workflow authorization to Platform. See [the delegated-authority policy](docs/governance/platform-delegated-authority.md), [the initial T0 authorization](docs/governance/t0-preparation-authorization.md), and [the accepted T0 human review report](docs/reviews/human-checkpoints/T0-source-reuse-audit-human-review.md). Platform advances routine work automatically and stops only at recorded mandatory human checkpoints. The active checkpoint is confirmation of source reuse/distribution rights before T1 extraction. UI stays in U0 until human acceptance of F1.

## Required reading

1. `docs/planning/master-development-plan.md`
2. `docs/governance/orchestration-protocol.md`
3. `docs/reviews/development-readiness-review.md`
4. `projects.yaml`

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
    ├── architecture/
    │   ├── README.md
    │   ├── everything-as-plugin-v2.1-original-reference.drawio
    │   └── capability-bus-alpha-optimized-topology.drawio
    ├── planning/master-development-plan.md
    ├── reviews/development-readiness-review.md
    └── reviews/human-checkpoints/
```
