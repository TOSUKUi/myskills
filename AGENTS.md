# AGENTS.md

This file is the thin constitution for this repository.

It should stay short.
Do not turn it into a full state ledger, runbook, or long task history.

## Purpose

- Define the project at a high level.
- Define where durable truth lives.
- Define how task-scoped agent work should proceed.
- Define what must be written back before a task ends.

## Project Summary

Fill this in with 3-6 lines only.

- Product:
- Main user:
- Current delivery phase:
- Primary technical constraints:
- Main risk:

## Source Of Truth

Use these files as the durable reload set.

- [docs/project/current_state.md](docs/project/current_state.md)
- [docs/project/critical_path.md](docs/project/critical_path.md)
- [docs/agent_loop/overview.md](docs/agent_loop/overview.md)
- [docs/agent_loop/handoff_contracts.md](docs/agent_loop/handoff_contracts.md)
- [docs/agent_loop/evaluation_policy.md](docs/agent_loop/evaluation_policy.md)
- [docs/agent_loop/write_back_policy.md](docs/agent_loop/write_back_policy.md)

Keep detailed feature contracts, ADRs, and runbooks outside this file.

## Agent Loop

Default task loop:

1. Create or refresh a short `task_record`.
2. Build a `research_pack` only when the task spans multiple files or ambiguous docs.
3. Create an `implementation_brief`.
4. Run implementation in a narrow task-scoped session.
5. Evaluate against the done definition in a separate review phase.
6. Write back only the minimum durable updates needed for the next fresh session.

## Live Vs Durable Context

Live context should contain:

- the active task
- the current phase
- the current artifact being worked on
- only the minimum nearby code or docs needed right now

Durable context should contain:

- current state
- critical path
- accepted task outcomes
- evaluation decisions
- restart notes

Do not keep raw transcripts as the primary source of truth.

## Required Artifacts

The default task-scoped artifacts are:

- `task_record`
- `research_pack`
- `implementation_brief`
- `evaluation_report`
- `restart_note`
- `plan_delta` only when sequencing changed

See [docs/agent_loop/handoff_contracts.md](docs/agent_loop/handoff_contracts.md).

## Resume Order

When context is lost, resume in this order:

1. Read this file.
2. Read [docs/project/current_state.md](docs/project/current_state.md).
3. Read [docs/project/critical_path.md](docs/project/critical_path.md).
4. Read the active task's latest `restart_note`.
5. Load only the artifacts needed for the next phase.

## Write-Back Rule

Before ending a meaningful task step:

- update the `restart_note`
- update `current_state` only if accepted facts changed
- update `critical_path` only if sequencing or dependencies changed
- keep changes short and factual

## Anti-Patterns

Do not use this file for:

- long task histories
- copied implementation summaries
- full runbooks
- repeated feature contracts
- speculative future plans without task impact
