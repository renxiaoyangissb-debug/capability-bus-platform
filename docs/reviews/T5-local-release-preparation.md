# T5 local release preparation

Date: 2026-10-03 (Asia/Shanghai)  
Scope: local-only; no public upload, push, remote release, deployment, signing, or credential use

## Exact local candidates

- Core `c64bc3fe36b7abb1cf33789bf8beee73e9bb4722`, local annotated tag `v0.1.0-alpha.4`, tag object `b0f3e71986e51ce26baf1960ecce8bcae11a1f93`.
- Official UI `d77a9a92f7abd435c3d5447db8179e4325f3e79d`, local annotated tag `v0.1.0-alpha.1`, tag object `4956be04b7bfa7cd242df54dc6a34a9ded6e181c`.

Core differs from the accepted runtime commit `4858520` only by the release-evidence test stabilization in `c64bc3f`. Platform independently inspected that change, ran the candidate verifier, rebuilt and isolated-installed the wheel, and passed 105/105 tests; two Unix-socket tests were skipped by the sandbox. Runtime code, versions, architecture, and F1/F2 contracts were unchanged.

Official UI retained the accepted implementation commit. Its local preparation passed contract snapshot verification, type checking, 28/28 tests, production build, license/source-boundary checks, and 31/31 plugin-package integrity checks.

## Local bundle

The ignored local directory `.release-local/capability-bus-alpha4-local-2026-10-03/` contains component artifacts, component metadata/checksum manifests, a bundle metadata file, and a root checksum manifest. The root manifest is the authoritative local byte inventory. Artifacts are intentionally not tracked by Git.

## Publication boundary

All metadata declares `public_upload=false`. No Git tag was pushed, no package registry was contacted, no remote Release was created, and no deployment or credential operation occurred. Any publication remains a separate human-authorized action.
