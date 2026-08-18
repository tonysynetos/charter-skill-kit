# Charter — long autonomous run kit for Claude Code

A `/charter` skill for Claude Code: set up, resume, or close a long-running autonomous task
that will outlive a single session (an overnight run, a big migration, a multi-hour audit).
It works by writing two plain markdown files per loop — `CHARTER.md` (the goal, verify
criteria, and boundaries, written once) and `STATE.md` (rewritten after every step) — so any
new session, even a completely fresh one after a crash or context reset, can pick the loop
back up cold just by reading those two files. No special infra, no server, just markdown +
discipline.

## What's in this folder

```
skills/
  charter/
    SKILL.md              <- the skill itself
    loop-charter/
      README.md           <- the 3 rules the mechanism depends on
      CHARTER.template.md <- copy this per loop
      STATE.template.md   <- copy this per loop
agents/
  verifier.md             <- optional but recommended: a fresh-context "does this meet
                              spec" checker, used when closing out a loop
```

## Install (2 minutes)

1. Copy `skills/charter/` into your Claude Code skills folder, so you end up with
   `~/.claude/skills/charter/SKILL.md` (Mac/Linux) or
   `C:\Users\<you>\.claude\skills\charter\SKILL.md` (Windows).
2. (Recommended) Copy `agents/verifier.md` into `~/.claude/agents/verifier.md`. This gives
   Claude Code a ready-made subagent for the "close the loop" step. If you skip this, Close
   mode still works — Claude just opens a fresh session/conversation and pastes in the
   charter + diff by hand instead of dispatching a subagent.
3. Restart Claude Code (or start a new session) so it picks up the new skill.

That's it — no other skills, MCPs, or commands required. This version was deliberately
stripped down from the original so it has zero dependency on anything outside this folder.

## How to use it

- **Start a loop:** say something like *"charter this: <describe the task>"* or *"set up an
  overnight run for X"*. Claude will interrogate the goal until it's concretely verifiable
  (the "Done Drill"), then write `loops/<name>/CHARTER.md` + `loops/<name>/STATE.md` in your
  project and hand you a one-line launch prompt.
- **Resume a loop** (new session, after a crash, after closing your laptop): say *"resume the
  <name> loop"*. Claude reads STATE.md and continues from exactly where it left off — no
  recap needed.
- **Close a loop:** say *"close the <name> loop"*. Claude runs a fresh-context verification
  pass against the charter (using the bundled `verifier` agent if you installed it), and only
  marks it closed if that verification actually passes.

## Why the two files matter

- `CHARTER.md` is written once and never edited mid-run. If you find yourself wanting to
  change it, that's a sign to stop and re-charter, not to drift the goal underneath the work.
- `STATE.md` is the loop's real memory — rewritten after every completed step. Conversation
  context (chat history) is treated as disposable; if something important isn't in STATE.md,
  it didn't really happen as far as the next session is concerned.

## What changed from the original internal version

The version this was built from also plugged into a personal task-compiler skill and a
"/goal" stop-enforcement command. Neither is included here: the task-compiler step is folded
directly into the skill's "Compile the legs" step (so you don't need it as a separate skill),
and "/goal" turned out not to be a real, installable command even in the original setup — the
actual stop-enforcement is just "re-check the charter's own Done-artifact and Verify-against
fields for real before declaring done," which this version does directly.
