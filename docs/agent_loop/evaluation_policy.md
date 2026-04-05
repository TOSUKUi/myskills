# Evaluation Policy

Use this file to keep evaluation separate from implementation.

## Evaluation Order

1. Check the done definition.
2. Check scope and invariants.
3. Check regressions and missing tests.
4. Check docs and restart-material drift.
5. Decide pass, partial, or fail.

## Decision Meanings

- `pass`
  The accepted result meets the task scope and has no material drift.
- `partial`
  The result is usable, but follow-up work or write-back is still required.
- `fail`
  The result has a blocking defect, unmet requirement, or major mismatch.

## Findings Rule

Findings must come before summary.
Each finding should be concrete and tied to:

- a file
- a behavior
- a missing check
- or a documentation mismatch

## Drift Rule

Evaluation is not only about code.
Also check whether accepted facts now require updates to:

- current state
- restart notes
- critical path
- linked decision docs
