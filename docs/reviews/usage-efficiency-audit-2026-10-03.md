# Three-project Codex usage-efficiency audit

Audit date: 2026-10-03 (Asia/Shanghai)

## Scope and method

This audit reads the local `token_usage_record` telemetry for the registered Platform, Core, and Official UI conversations and the account usage-window endpoint. Thread token totals are raw model telemetry; they are not a direct conversion to plan allowance or credits because OpenAI applies product-, model-, cache-, and task-dependent accounting.

Telemetry snapshot:

| Conversation | Model / effort | Raw total tokens | Cached input | Model samples | Completed tool actions |
|---|---:|---:|---:|---:|---:|
| Platform `01a0fba2-…` | `gpt-5.6-sol` / `high` | 17,844,013 | 17,129,984 | 177 | 156 |
| Core `01a0fba0-c91d-…` | `gpt-5.6-sol` / `high` | 9,877,440 | 9,335,296 | 71 | 63 |
| Official UI `01a0fba0-e14d-…` | `gpt-5.6-sol` / `high` | 14,825,402 | 14,371,968 | 111 | 197 |
| **Combined** |  | **42,546,855** | **40,837,248** | **359** | **416** |

Of 42,349,308 input tokens, 40,837,248 were cached input (96.4%). Output was 197,547 tokens, including 52,197 reported reasoning tokens. The dominant cost driver was therefore repeated long context across many model/tool round trips, not the visible final answers.

Account snapshot at the audit point:

- five-hour window: 36% used, resetting at 2026-10-03 06:55:34 CST;
- weekly window: 88% used, resetting at 2026-10-08 00:32:40 CST;
- plan: Plus; credit balance: 0; one full rate-limit reset credit was available.

## Root causes

1. All three conversations used `high` reasoning for routine coordination, implementation, shell inspection, and ledger work.
2. The three long-lived conversations carried large system, repository, governance, and prior-output context into every model sample. Prompt caching reduced repeated uncached input, but did not remove repeated context processing.
3. Work was split into many small tool steps: 359 model samples and 416 completed command/MCP/file-change actions.
4. UI work was especially tool-heavy: one correction turn used 66 command executions; an earlier partial turn used 74. Repeated offline bootstrap, lock, build, package, and integrity checks increased elapsed time and samples.
5. Platform repeated full evidence reads, validation, ledger patches, commits, and status waits around multiple narrative milestones. One Platform turn also absorbed three user messages and a context compaction, reaching 4.32 million raw tokens.
6. Parallel Core/UI execution reduced some wall-clock time but increased aggregate quota consumption. Sequential Platform verification then added another full context-heavy pass.

## Corrective controls

- One active implementation lane by default.
- `low` reasoning for coordination and mechanical verification; `medium` for normal implementation; scoped `high` only for a named hard problem.
- Soft ceiling of 12 model/tool round trips per owning-project turn.
- Focused tests during development; one full owner candidate suite and one Platform acceptance suite.
- One event-driven completion wait; no frequent polling or full-history-with-output reads.
- Short dispatch prompts referencing committed specifications.
- Phase-boundary context reset when history has become large.
- Material-state ledger commits only; commentary-only progress does not create a new verification cycle.

These controls are normative in `docs/governance/usage-efficiency-policy.md`, root `AGENTS.md`, and `docs/governance/orchestration-protocol.md`.

## Official guidance used

- [OpenAI reasoning guide](https://developers.openai.com/api/docs/guides/reasoning): lower reasoning effort favors speed and lower token usage; reasoning tokens occupy context and are counted as output.
- [OpenAI deployment checklist](https://developers.openai.com/api/docs/guides/deployment-checklist): select model and reasoning effort by workload, measure latency/token cost per successful task, and avoid unnecessary multi-agent overhead.
