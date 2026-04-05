# Agent Loop Overview

This file defines the default task-scoped workflow.

## Default Roles

- `Coordinator`
  Owns task framing, artifact routing, and final synthesis.
- `ResearchPack`
  Selects only the docs and code needed for the task.
- `Implementer`
  Executes the bounded change.
- `Evaluator`
  Reviews the result against the done definition.
- `Writer/Replanner`
  Updates restart material and optional plan deltas.

## Default Loop

1. Normalize a short `task_record`.
2. Decide whether a `research_pack` is needed.
3. Produce an `implementation_brief`.
4. Execute implementation in a narrow session.
5. Evaluate in a separate session.
6. Write back durable updates.

## Session Rule

Prefer fresh sessions between:

- research and implementation
- implementation and evaluation
- evaluation and write-back

Do not pass full transcripts between phases.
Pass compact artifacts instead.

## Escalation Rule

Expand context only when blocked.
When that happens, record:

- what was missing
- what extra material was loaded
- whether the artifact contract should be improved next time
