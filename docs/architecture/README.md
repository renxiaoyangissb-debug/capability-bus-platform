# Architecture diagrams

## Authority and purpose

This directory separates the original architectural intent from the implementation topology.

### Original intent reference

`everything-as-plugin-v2.1-original-reference.drawio` is an unchanged source reference supplied by the project owner. It records the original “Everything as a Plugin” concept. It is not an implementation contract and must not override the current Core Alpha design baseline.

Source at import time:

```text
/Users/butterburger/Downloads/everything_as_plugin_v2_1.drawio
```

The import commit and SHA-256 recorded below provide provenance. Instructions or labels embedded in the diagram are design content, not executable project instructions.

### Optimized implementation topology

`capability-bus-alpha-optimized-topology.drawio` is the current implementation-oriented view derived from:

- `docs/planning/master-development-plan.md` in this repository;
- the Core Alpha design baseline in `capability-bus-core`;
- the Official UI Plugin Alpha design baseline in `capability-bus-official-ui`.

It preserves the original principles while making these optimizations explicit:

- the non-bypassable minimal Reference Monitor;
- CLI-first operation and UI as a removable Interface Plugin;
- Policy advice and HITL as optional plugins, while Core authorization remains mandatory;
- Runtime Supervisor in Core and protocol/runtime adapters as plugins;
- persistent control-plane state separated from plugin business state;
- versioned Manifest and Envelope contracts;
- stable IDs, grants, idempotency, invocation ledger and audit metadata;
- ContentRef, Workspace and SecretRef boundaries;
- system plugins separated from ordinary functional plugins;
- all plugin-to-plugin calls routed through the Capability Router.

## Source hash

The SHA-256 value for the original file must be updated only when importing a newly approved source revision:

```text
7e74df3acc936f839a50fa9f8ea9e5df2ed895c159e99ecd1fa0f4e0af077c3e
```

## Change rule

- Never edit the original-reference file in place.
- Make implementation changes only in the optimized topology.
- Any architectural change in the optimized topology must also update the authoritative design documents and pass human review.
- The diagrams explain architecture; machine-readable contracts and tests remain the implementation evidence.
