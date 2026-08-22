# CHARTER — <loop name>

Written once at launch. Not edited mid-run. If wrong → stop, re-charter, restart.

## Goal
<one sentence — what exists when this loop is done>

## Done-artifact
<the nameable, verifiable artifact: test output, committed file, screenshot, a named report file>

## Verify against
<the external truth source — never the artifact's own claims: real data / test suite /
reference site / spec file at path X>

## Verifier capability (declare BEFORE writing any verify step)
<what the verifying agent ACTUALLY has: tools, credentials, network, DB access>
Default: a read-only verifier subagent typically has no database or MCP access — this
kit's bundled `agents/verifier.md` ships with Read/Grep/Glob/Bash only. A verify step
that needs capability the verifier lacks is invalid on its face. Either grant it
explicitly (name the agent and the credentials) or rewrite the step to judge from
persisted evidence + code.

## Legs (goal → verify pairs)
1. <step> → verify: <check>
2. <step> → verify: <check>
3. ...
(Each leg ≤ ~30 min of work — must be cheap to redo from STATE.md.)

## Budget
- Loop exit: 3 consecutive failures on one leg → write failure analysis to STATE.md, stop.
- Token/time ceiling: <e.g. "one overnight run" / a specific token budget if your tool tracks one>

## Unattended pre-flight (nobody watching — all three answered before launch)
1. Stop condition exists and is named: <§Budget loop-exit + the done-artifact>
2. Every verify step's required capability is actually held, per §Verifier capability:
   <which steps were checked against it>
3. Absence is detected, not just failure: <what notices if this run produces NOTHING — a
   run that writes no file and throws no error must still get flagged>
Any of the three unanswered → do not launch unattended.

## Boundaries (do NOT)
- <e.g. no pushes, no schema changes, no touching payment/auth logic>
- No scope expansion: deliver the charter, flag adjacent findings to STATE.md §Parked.

## Dispatch contract (subagents, if you use them)
- Tier: cheap/fast for mechanical legs, your strongest model for judgment-heavy ones;
  money/auth/security/irreversible legs never go below your strongest model.
- Prompt carries: subtask goal + verify, file-path evidence pointers, "return a digest."
- Coordinator judges digests only — never reads the raw material a subagent already read.
- Two-proof split — the default dispatch pattern. A leg that skips it should say why,
  here:
  - Builder proves "the code does what it claims" — via tests. Give it NO live
    credentials, so it can't claim a live write succeeded when it only ran a test.
  - Coordinator proves "the real system actually did it" — a live check against the
    real system, run by the coordinator itself, never delegated. Persist the raw
    output to disk (path recorded in STATE) — an unpersisted live check is an
    inference, not evidence.
- Per leg: <who builds → what the coordinator's live check is>

## Rules pointers
- <project-specific conventions or docs worth linking here>
