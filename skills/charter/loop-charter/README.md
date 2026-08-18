# Loop Charter — long autonomous runs

One folder per long-running loop: `loops/<name>/CHARTER.md` + `loops/<name>/STATE.md`,
written inside the TARGET project's repo (not here — this folder holds only the templates
and rules, shared by every loop you start).

## The three rules

1. **CHARTER is written once, never edited mid-run.** Goal, verify criteria, budget,
   boundaries. If the charter turns out wrong, that is a reason to STOP and re-charter,
   not to drift.
2. **STATE is flushed after every completed leg.** If STATE.md is always current, any
   cutoff — compaction, crash, closing the laptop — is organic by construction. Never let
   a finding live only in conversation context; if it matters, it goes in the file.
3. **Subagent dispatch (if you use one) follows the contract in CHARTER §Dispatch.** The
   dispatch prompt carries: goal + verify criterion for the subtask, pointers to evidence
   files (paths, not prose), a model/effort tier if your setup supports one, and the
   instruction to return a digest, not raw material — the coordinator judges digests, it
   doesn't re-read everything a subagent read.

## Launch

```
Read loops/<name>/CHARTER.md and loops/<name>/STATE.md, then continue the loop.
```

That one line is the entire launch AND relaunch prompt. The charter's own §Done-artifact and
§Verify-against are the stop condition — there is no separate command required. If your setup
has an evaluator/stop-hook mechanism, wire it to check those two fields; if not, just re-check
them yourself, for real, before ever declaring the loop done.

## Cutoff behavior

Don't monitor token counts or try to stop at some threshold — that's brittle and depends on
your specific tool. The only obligation is rule 2: state flushed per leg. If a session ends
mid-leg, the next one just re-runs that leg from STATE.md — which is why legs need to be
small enough to redo cheaply (~30 min of work each is a good ceiling).
