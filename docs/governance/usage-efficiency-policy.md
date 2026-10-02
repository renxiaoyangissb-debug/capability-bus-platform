# Capability Bus usage-efficiency policy

Effective date: 2026-10-03 (Asia/Shanghai)

## Objective

Minimize model quota and elapsed time while preserving repository ownership, architecture gates, reproducible tests, and independent Platform verification.

## Defaults

| Work type | Model/reasoning policy | Concurrency |
|---|---|---|
| Platform coordination, status, ledger, targeted evidence reads | economical available model or current model with `low` reasoning | one active coordinator |
| Core or UI implementation | owning project's capable coding model with `medium` reasoning | one implementation lane |
| Named architecture/security/debugging impasse | temporarily use `high` reasoning for one scoped turn | no additional lane |

High reasoning and parallel implementation are opt-in exceptions, not defaults.

## Per-turn budget

- Soft ceiling: 12 model/tool round trips in an owning-project turn.
- Perform one batched discovery read, then edit coherent file groups.
- Prefer one combined targeted-check command over many single-file commands.
- Limit command output to the lines needed for the decision.
- At the ceiling, stop at a recoverable checkpoint with HEAD, dirty paths, completed work, failing check, and exact resume action.
- Do not continue merely to provide more commentary or polish an intermediate report.

## Test budget

- During implementation, run the smallest affected test slice.
- The owning project runs the full suite once when its candidate is ready.
- Platform runs one independent full acceptance suite per candidate.
- If acceptance fails, the owner runs the failed slice while correcting and one final full suite. Platform reruns the affected acceptance evidence once.
- Dependency installation, fresh-clone, packaging, and integrity checks run only when the candidate or changed boundary requires them.

## Context and monitoring

- Dispatch prompts reference committed documents and exact paths; they do not restate all historical evidence.
- Do not request full thread history with command outputs. Read the final report first, then request only a targeted missing excerpt.
- Do not poll unchanged work. Use a single event-driven wait for completion or required user action.
- Do not merge unrelated new work into a long in-progress turn. Finish or stop the current turn, then start a small scoped turn.
- Compact or start a fresh owning-project conversation at phase boundaries when accumulated history materially enlarges every tool round trip.

## Evidence and ledgers

- Narrative status is not completion evidence; a Git commit plus scoped test results remains required.
- `work-buoy.yaml` is updated at dispatch, a material commit/test milestone, correction, long wait, pause, or gate transition—not for every commentary message.
- `current-work.md` and `projects.yaml` are updated together at material state changes.
- Platform may combine the evidence update and state transition in one commit.

## Resume rule

Before resuming paused work, Platform must show the user the intended model, reasoning level, active lane, test plan, and expected verification boundary. No paused lane resumes automatically.
