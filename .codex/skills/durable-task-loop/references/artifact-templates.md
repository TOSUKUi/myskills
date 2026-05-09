# Artifact Templates

Use these compact templates when a project does not already define its own.

## Restart Note

```md
# Restart Note

**Last updated:** YYYY-MM-DD  
**Active task:** <task-id> | none

## latest_outcome
- what finished
- key evidence
- reviewer result

## next_action
- exact next bounded task
- if brief missing, say so explicitly

## reload_set
1. path — why
2. path — why

## open_questions
- blocking or non-blocking questions only
```

## Implementation Brief

```md
# Implementation Brief: <task-id>

**Type:** planning | implementation | validation  
**Status:** planned | reviewed | complete

## Objective

## Scope
- item

## Non-Scope
- item

## Deliverables
- file or artifact

## Acceptance Criteria
- [ ] criterion

## Verification
- command or check
```

## Validation Result

```md
# Validation Result: <task-id>

## Inputs
- docs / files / environment checked

## Checks Run
- command

## Findings
| ID | Severity | Result | Notes |
|----|----------|--------|------|

## Verdict
- pass / pass with warnings / fail

## Recommended Next Action
- bounded follow-up if needed
```
