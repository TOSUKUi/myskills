# Skills

This directory contains the repository-local skills used to keep Codex work narrow, artifact-driven, and restart-friendly.

Each skill is defined by its own `SKILL.md`. That file is the contract: what the skill is for, what inputs it expects, how it should run, and what it should produce. Supporting material lives next to it in `references/`, `profiles/`, `prompts/`, `scripts/`, and `agents/` when needed.

## Why These Skills Exist

This repo is organized around a task-scoped agent loop rather than one long, stateful session. The skills here exist to enforce a few development constraints:

- keep live context small
- separate research, implementation, evaluation, and write-back
- pass compact artifacts instead of raw transcripts
- keep `AGENTS.md` thin and push durable detail into dedicated docs

In practice, that means a task should usually move through explicit handoff artifacts such as `task_record`, `research_pack`, `implementation_brief`, `evaluation_report`, and `restart_note`.

## Skill Index

### `bootstrap-project-state`

Refreshes or fills the repo's thin constitution and durable state docs from checked-in evidence.

Use it when:

- `AGENTS.md` is still a template
- `docs/project/current_state.md` is stale or empty
- `docs/project/critical_path.md` no longer reflects reality
- you need to reconstruct project state from local repository evidence without guessing

Primary outputs:

- updated `AGENTS.md`
- updated `docs/project/current_state.md`
- updated `docs/project/critical_path.md`

### `ddgr`

Provides a terminal-first workflow for web search using the local `ddgr` CLI.

Use it when:

- you explicitly want DuckDuckGo results from the shell
- you want reproducible search commands
- you need JSON output for downstream filtering
- you want candidate URLs before handing them to a page inspection tool

This is a discovery tool, not a full browsing or page-reading workflow.

### `swarm-meeting`

Runs a structured, multi-role design review for a technical decision.

Use it when:

- a task needs option comparison before implementation
- the user asks for an architecture debate or tradeoff review
- an MVP path needs pressure-testing
- a vague "let's discuss this" request should become a bounded decision session

Typical outputs:

- problem framing
- role memos
- options considered
- recommended direction
- concrete next actions

### `swarm-research-pack`

Builds a compact evidence pack from the smallest useful set of local artifacts.

Use it when:

- the task spans multiple files or docs
- a design discussion needs a common evidence base
- the implementation context is too broad to load directly
- you want to separate confirmed facts from assumptions before coding

This skill is for compression, not for durable write-back.

### `task-implementer`

Executes a bounded change from an `implementation_brief` or similar scoped handoff.

Use it when:

- the task is already framed
- file scope is known or mostly known
- the goal is implementation, not re-planning
- you want the smallest credible change with targeted verification

This skill should stay narrow and only expand context when blocked.

### `task-evaluator`

Reviews an implementation result against scope, risks, checks, and doc consistency.

Use it when:

- implementation is complete enough for review
- you want a pass/fail or accept/partial-reject decision
- you need explicit findings before write-back
- you want to catch missing tests, regressions, or drift in restart material

This is an evaluation phase skill, not a coding phase skill.

### `task-supervisor-loop`

Coordinates an entire task across research, implementation, evaluation, and write-back phases.

Use it when:

- you want one orchestrating workflow for the whole task
- the task is large enough that phase boundaries matter
- live context needs to stay small across a longer effort
- fresh-session handoffs are preferable to one accumulated transcript

This is the highest-level workflow skill in the repo.

### `task-writeback`

Applies the minimum durable updates after a task has been evaluated and accepted.

Use it when:

- the implementation result is accepted
- `restart_note` needs to be updated
- `current_state` changed in a meaningful way
- `critical_path` changed because sequencing or dependencies changed

This skill exists to preserve durable truth without turning repo docs into logs.

## How They Fit Together

The default repo workflow is:

1. Normalize the task.
2. Build a `research_pack` only if the task is broad or ambiguous.
3. Produce an `implementation_brief`.
4. Implement in a narrow context.
5. Evaluate separately.
6. Write back only the durable deltas.

Mapped to skills, that usually becomes:

1. `bootstrap-project-state` only when project-state docs need initialization or refresh
2. `swarm-research-pack` when the evidence set is too broad
3. `swarm-meeting` when a design decision is still unresolved
4. `task-implementer` for the actual bounded change
5. `task-evaluator` for acceptance review
6. `task-writeback` for restart notes and durable docs

If you want one wrapper around that lifecycle, use `task-supervisor-loop`.

## Choosing The Right Skill

Use this quick routing guide:

- Need to reconstruct repo state from local evidence: `bootstrap-project-state`
- Need a compact evidence set before reasoning: `swarm-research-pack`
- Need a structured architecture or product decision review: `swarm-meeting`
- Need to make the scoped change: `task-implementer`
- Need to judge the result against the brief: `task-evaluator`
- Need to update restart material and durable docs: `task-writeback`
- Need to orchestrate the full lifecycle: `task-supervisor-loop`
- Need terminal-native web search: `ddgr`

## Directory Layout

- `SKILL.md`
  The skill contract. Start here.
- `references/`
  Checklists, policy notes, and additional rules.
- `profiles/`
  Named operating modes for skills that support multiple shapes of work.
- `prompts/`
  Short role prompts, mainly for meeting-style workflows.
- `scripts/`
  Local helper scripts used by some skills.
- `agents/`
  Supporting material for role-specific or delegated workflows.

## Notes For Contributors

- Keep each skill narrow. If a skill tries to do planning, implementation, evaluation, and write-back all at once, it is probably too broad.
- Put durable operational rules in docs under `docs/`, not in a skill unless the rule is truly skill-specific.
- Treat `SKILL.md` as an interface contract. Update it when workflow expectations change.
- Prefer adding a small reference or helper script over bloating the main skill instructions.
- Keep examples concrete and artifact-oriented.

For repo-wide workflow rules, start with [AGENTS.md](/Users/amemiya/work/swarm-meeting/AGENTS.md) and the docs under [docs/agent_loop](/Users/amemiya/work/swarm-meeting/docs/agent_loop/overview.md).
