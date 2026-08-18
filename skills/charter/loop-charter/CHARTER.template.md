# CHARTER — <loop name>

Written once at launch. Not edited mid-run. If wrong → stop, re-charter, restart.

## Goal
<one sentence — what exists when this loop is done>

## Done-artifact
<the nameable, verifiable artifact: test output, committed file, screenshot, a named report file>

## Verify against
<the external truth source — never the artifact's own claims: real data / test suite /
reference site / spec file at path X>

## Legs (goal → verify pairs)
1. <step> → verify: <check>
2. <step> → verify: <check>
3. ...
(Each leg ≤ ~30 min of work — must be cheap to redo from STATE.md.)

## Budget
- Loop exit: 3 consecutive failures on one leg → write failure analysis to STATE.md, stop.
- Token/time ceiling: <e.g. "one overnight run" / a specific token budget if your tool tracks one>

## Boundaries (do NOT)
- <e.g. no pushes, no schema changes, no touching payment/auth logic>
- No scope expansion: deliver the charter, flag adjacent findings to STATE.md §Parked.

## Dispatch contract (subagents, if you use them)
- Tier: cheap/fast for mechanical legs, your strongest model for judgment-heavy ones;
  money/auth/security/irreversible legs never go below your strongest model.
- Prompt carries: subtask goal + verify, file-path evidence pointers, "return a digest."
- Coordinator judges digests only — never reads the raw material a subagent already read.

## Rules pointers
- <project-specific conventions or docs worth linking here>
