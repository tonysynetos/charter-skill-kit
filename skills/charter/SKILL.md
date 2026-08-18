---
name: charter
description: Set up, resume, or close a chartered long autonomous run (CHARTER.md + STATE.md). Use when you say "charter this", "/charter", "set up a long run", "overnight run", "run this autonomously", "start a loop for X", "hand this off overnight", or when a task is big enough that it will span sessions/compaction and needs a durable goal + state file. Also fires for "resume the <name> loop" and "close the <name> loop". Not for small tasks finishable in one sitting — answer plainly and skip the ceremony.
---

# Charter — long autonomous runs

Templates and rules of the convention live in this skill's own `loop-charter/` folder
(`loop-charter/README.md` is the source of truth for the three rules; don't restate them
here — apply them).

Three modes. Pick by phrasing: create (default) / resume / close.

## Mode: Create

**Done-artifact of this mode:** `loops/<name>/CHARTER.md` + `loops/<name>/STATE.md` written
in the TARGET project's repo, and a launch line handed back to the user.

### Step 1 — The Done Drill (before any template filling)

"Complete" is almost always harder to define than it first looks — do NOT accept the goal
as given. Interrogate it one question at a time: ask, propose a recommended default answer,
wait for a confirmation or correction, then ask the next one. Keep going until all five below
have concrete answers:

1. **Artifact-first.** "When this is done, what file/test-output/screenshot/commit exists
   that doesn't exist now?" If it can't be named, the goal isn't ready — keep drilling.
2. **External bar.** "What non-self source proves it?" (real data, a test suite, a reference
   site, a spec file at a path). The artifact's own claims never count.
3. **Stranger test.** "Could a fresh verifier — a separate session or subagent given ONLY
   the charter and the diff, no conversation history — return PASS / NEEDS WORK?" If judging
   requires conversation context, the criteria are not written down yet.
4. **Negative space.** "What observation, seen later, would make you reopen this?" Invert it
   into an explicit check (this catches the 'not-observed ≠ absent' trap: every negative
   claim must name where you looked, not just that you didn't see it).
5. **Scope fence.** "What is explicitly NOT included?" → charter §Boundaries + STATE §Parked.
   Unfenced scope is how loops never finish.

Trivial-case exemption: if all five answer themselves in one read, say so and fill the
templates without the interview.

### Step 2 — Compile the legs

Break the drilled goal into an ordered list of legs. For each leg, state:
- The concrete action.
- The verify check — an observable, external check ("tests pass", "the number matches the
  export", "the screenshot shows X"), never "look at what I built."
- Keep each leg small enough to redo cheaply if a session gets cut off (~30 min of work is a
  good ceiling).

Also set, once, for the whole loop:
- **Loop-exit budget:** 3 consecutive failures on one leg → stop and write a short failure
  analysis (what was tried, what failed, current hypothesis) instead of grinding past it.
- **Model/effort tier per leg, if your setup supports dispatching subagents at different
  tiers:** cheap/fast for mechanical steps, your strongest model for judgment-heavy ones.
  Never route money, security, auth, or irreversible-action legs to a cheap tier — always
  your strongest model, with careful review.

These become charter §Legs and §Budget.

### Step 3 — Write the files

Copy `CHARTER.template.md` / `STATE.template.md` from this skill's `loop-charter/` folder
into `<target-repo>/loops/<name>/`, filled in. STATE starts at "Leg 0 — not started" with the
launch line as §Next step. Write the files with your normal file-write tool — don't round-trip
the content through a shell pipeline that could mangle encoding.

### Step 4 — Launch

Hand the user exactly:

```
Read loops/<name>/CHARTER.md and loops/<name>/STATE.md, then run the loop.
```

That's the entire launch and relaunch prompt. There's no special "stop-enforcement" command
required — the charter's own §Done-artifact and §Verify-against ARE the stop condition. Before
ever declaring the loop done, re-read those two fields from CHARTER.md and check them for
real (re-run the test, re-open the file, re-check the number) — never accept "I'm done" as
its own evidence. If your setup has some way to force that check externally (a stop hook, an
evaluator, a second session/agent that only sees the charter and the diff), use it — it isn't
required for the mechanism to work, just a good backstop if you have one.

If the run will mutate git and something else might be touching the same repo at the same
time, do the work in an isolated git worktree so the two don't collide.

## Mode: Resume

Read `loops/<name>/CHARTER.md` + `STATE.md`. Don't recap what's already in STATE back to the
user — execute §Next step immediately. If STATE looks stale versus git (check `git log` since
the last flush timestamp), trust git and reconcile STATE first — files beat prose.

## Mode: Close

1. Run the charter's final verification: a fresh-context verifier — a separate session or
   subagent given ONLY the charter and the diff/artifact paths, never the build conversation
   or a summary of it — that answers "does this meet the charter?" This skill bundles a ready
   `verifier` agent definition (see the package's `agents/verifier.md`); use it if your setup
   has custom subagents, or just open a fresh chat/session and paste in the charter + diff if
   it doesn't.
2. PASS → mark STATE "CLOSED <date>, verified by <artifact path>", move the loop folder to
   `loops/_closed/` in the same repo, then do your normal end-of-session wrap-up (commit, and
   a short summary of what shipped). Reuse whatever session wind-down habit you already have —
   don't invent a new one just for this.
3. NEEDS WORK → the verifier's findings become new legs appended to STATE §Next step; the loop
   continues. Do not soften the bar just to close faster.

## Rules that keep this honest

- CHARTER is never edited mid-run; STATE is flushed after every leg (the two invariants —
  breaking either voids the "organic cutoff" guarantee: any session, once compacted, crashed,
  or ended, can be picked back up cold from STATE alone).
- 3 consecutive failures on one leg → stop and escalate per charter §Budget; a broken plan is
  a reason to stop, not to improvise around the charter.
- Money/auth/security/irreversible legs: your strongest model, working directly, with careful
  review — never delegated down to a cheaper tier, regardless of what a tier tag would
  otherwise say.
