---
name: durable-task-loop
description: Run a restartable one-task-at-a-time supervisor loop using durable docs, fresh subagent sessions, bounded briefs, reviewer gates, and minimal write-back. Use when the agent should act as a thin orchestrator, avoid carrying long live context, and resume each task from restart notes or reload sets.
---

# Durable Task Loop

Use this skill when you want a thin facilitator that keeps almost no long-lived context and advances work through small bounded tasks.

## When to Use

Use this skill when:
- durable docs should be the only source of truth between sessions
- each task should run in a fresh subagent session
- implementation must be separated from review and write-back
- the project needs restart notes, reload sets, and brief-driven execution
- the orchestrator should avoid holding long transcripts in memory

## Required Inputs

Provide or identify these project-specific artifacts:
- operating-rules doc (`AGENTS.md` or equivalent)
- restart note (`restart_note_current.md` or equivalent)
- reload set named in the restart note
- active task brief (`implementation_brief_*.md`, planning brief, or research pack)
- optional next-task queue / task record files

If the project uses different filenames, map them explicitly before starting.

## Facilitator Role

Act as a thin supervisor, not the main implementer.

Rules:
- keep almost no durable context in live memory
- re-read the durable docs at each boundary
- use fresh subagent sessions for bounded tasks
- pass compact artifacts, not raw transcripts
- after each task, treat updated docs as the only resume context

## Standard Read Order

1. Read the operating-rules doc from top to bottom.
2. Read the current restart note.
3. Read the reload set in listed order.
4. Read the active brief if one exists.
5. If no active brief exists, normalize the next bounded task before implementation.
6. If the task is broad, ambiguous, or multi-area, create a compact research pack first.
7. Only then start planning or implementation.

## Bounded Task Policy

Keep exactly one active bounded task at a time.

A bounded task should clearly state:
- what it must satisfy
- specification
- scope
- non-scope
- deliverables
- verification plan

If a task is too large, split it before coding.

## Phase Loop

### 1. Normalize
Create or refine the next bounded task.

Output:
- task id
- short objective
- scope boundary
- non-scope
- deliverables
- acceptance criteria

If a brief is missing, create it before implementation.

### 2. Research Gate
Create a compact research pack only when needed.

Use a research pack when the task:
- touches multiple docs or code areas
- has ambiguous requirements
- depends on scattered evidence
- needs contract or behavior confirmation before coding

### 3. Implement or Validate
Run the bounded task in a fresh subagent session.

Guardrails:
- stay inside the active brief
- do not absorb adjacent backlog into the same session
- run only task-scoped checks
- keep notes short and file-based

### 4. Reviewer Pass
Always run a reviewer or auditor pass before write-back.

Reviewer checks:
- scope adherence
- obvious bugs
- contract drift
- doc consistency
- whether fix-up edits are required before closure

### 5. Minimum Durable Write-Back
Update only the smallest durable context needed to resume.

Typical write-back targets:
- restart note
- active brief status
- next action
- reload set
- open questions
- optional task record / queue entry

Do not bloat the operating-rules doc.

### 6. Close Boundary
After write-back:
- commit if the project workflow requires commits per bounded task
- discard live context
- resume next time only from updated durable docs

## Subagent Pattern

Prefer fresh subagent sessions for each bounded task.

If the project already specifies model routing, follow the project rule.
If model routing has not yet been specified, do **not** silently choose a permanent split for the user and do **not** invent environment-specific preset model names. Ask a short clarification question first, for example:
- "Thinking/planning/recon 用と coding/write-back 用のモデル振り分けを決めますか?"
- "reviewer / auditor 用のモデルも固定しますか?"
- "この環境で使えるモデル名に合わせて role ごとの割当を指定してください"

Only after the user answers, or the project docs define it, should you treat the routing as standard.

If no routing has been specified yet, refer only to abstract roles:
- thinking / recon / scoping model
- coding / edits / write-back model
- review / audit model

If the project already defines a facilitator agent or chain, prefer that project-native wrapper.

## Escalation Rule

Escalate only when at least one is true:
- no clear next bounded task can be justified from durable docs
- required evidence is missing and cannot be derived cheaply
- implementation repeatedly fails within the allowed scope
- the active brief is contradictory or materially incomplete
- validation reveals a blocking issue outside current task scope

When escalating, report:
- why the loop stopped
- the last completed bounded task
- the smallest next decision needed from the user

## Output Contract

Use this section order unless the project requires another format:

```markdown
## Current State
## Active or Next Bounded Task
## Scope Boundary
## Subagent Plan
## Result
## Reviewer Result
## Write-Back Result
## Next Resume Point
## Escalation Status
```

## Artifact Templates

See `references/artifact-templates.md` for compact templates for restart notes, briefs, and validation results.
