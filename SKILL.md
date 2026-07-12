---
name: tightship
description: Run a tight ship with any model — project-agnostic operating discipline for high-stakes autonomous work — repo-as-record, contract→adversarial-review→slices, delegation with self-verification, compact-proofing, model economy. Invoke at session start on any serious project; pair with a project skill for domain bindings.
---

# Tightship: frontier-quality sessions on any model

The quality bar lives in this procedure, not the model. Follow it mechanically; escalate
judgment, never improvise it.

## 1. The repo is the record
THE RECORD = the committed artifacts that carry project truth across sessions: (1) a
state doc (what this is, current position, in-flight resume procedures), (2) an
append-only decision log, (3) a work/findings log, (4) the wave plans/contracts with
their review findings, (5) git history + the code and tests. Membership test: if a
future cold session needs it and can't derive it from the code, it belongs in the
record. Conversations, model memory, and scratch files are NOT the record — they're
where truth gets discovered, not where it lives.
- Boot every session (and every post-compaction continuation) by reading the project's
  state docs before any action. Your memory, summaries, and recall are hints; the
  written record wins every conflict.
- **The record can drift — reality outranks it.** The precedence chain is: reality >
  record > memory. Boot doesn't just read the record, it spot-verifies its load-bearing
  claims (git log for "was it done", query live systems for "is it running", re-run the
  gate for "was it green"). When record and reality disagree, reconciling the record is
  the FIRST action — never execute a resume procedure you've caught lying, and never
  patch reality to match a stale doc.
- **Changes land WITHOUT the skill** (teammates, PR merges, other tools, skill-less
  sessions). Boot reconciles mechanically: `git log`/`git status` since the record's
  last update — anything the record doesn't mention gets a one-entry catch-up BEFORE the
  wave starts. Record silence never means "nothing happened."
- **Branches and merges:** record updates ride the same branch/PR as the code they
  describe. Merge conflicts in append-only logs resolve by KEEPING BOTH sides (appends
  don't contradict); the state doc describes NOW, so never textually merge two versions
  — rewrite its current-position block from post-merge reality.
- An undocumented action is an unfinished action: state docs update in the SAME commit
  as the state change. In-flight work is documented as a ZERO-CONTEXT RESUME PROCEDURE:
  numbered steps, literal commands, ids, failure playbooks, budget position, the stop
  condition — precise enough for a cold model to continue.
- Session-local files (scratch dirs, /tmp) die with the session; durable outputs go in
  the repo or the user's chosen delivery location.
- **Bare repo (no state docs yet)?** Bootstrapping the record IS the first action:
  create a minimal `docs/STATE.md` (what this project is, current position, what's in
  flight), a `docs/DECISIONS.md` (append-only: date, decision, why, flag for review),
  and a findings/work log — then proceed. Don't demand ceremony the project hasn't
  earned: one page total is enough. The rule is only that from this moment on, state
  changes update the record in the same commit.

## 2. The wave shape
A WAVE is the unit of work: ONE coherent goal taken from written plan to closed-out
delivery — bigger than a commit, smaller than a sprint; small enough for one plan, big
enough to deserve a review.
Anything non-trivial runs as: written contract (scope, slices, sources, budget, stop
conditions, review-findings section) → adversarial review → fold findings → re-verify to
zero MUST-FIX → build slice-by-slice → closeout (what shipped vs the contract, carried
items, spend actuals). Money/scope numbers come from the user ONCE, up front, with priced
options and a recommendation — never assumed, never renegotiated silently mid-wave.

## 3. Adversarial review — the quality equalizer
- 2+ named personas with DIFFERENT lenses attack the contract/plan before building.
- Claim-class discipline: a blocking finding (MUST-FIX) requires proof — a code citation
  or a runnable test. Claims about what an opaque external system will do are HYPOTHESES:
  settle them with a small real probe, not a reviewer's confidence. The gate is
  symmetric — you can't cut a feature on an untested "won't work" either.
- Reviewers write findings INTO the contract file (survives compaction). Fold blocking
  findings into the plan, then have the SAME reviewer re-verify the folds. Mid-build
  judgment calls beyond the written plan get the same review BEFORE committing.

## 4. Delegation with self-verification
- Delegate mechanical slices to background agents with tight specs: exact files owned
  (and which files OTHER agents own), the plan section, the pinned constraints verbatim,
  "do not commit", the gate list, "report ≤8 lines and stop".
- Parallelize only disjoint-file work; pre-assign anything agents could collide on.
- YOU verify: read the diff hunks that carry the invariants; run the full test/gate
  battery yourself; confirm tests actually EXECUTED (count them) — never trust an
  agent's "all green" prose. One commit per slice with its log entry.

## 5. Long-running and live operations
- Background waiters (poll-in-a-loop inside a background shell), never foreground sleep
  chains; never two state-changing commands in one script.
- Before any run that costs real money: arm the budget/limit mechanism first; rebuild
  stale binaries (workers read config/code at construction).
- On failure: root-cause in DATA (query the rows, read the actual error, extract the
  artifact) before any fix. A failure that pattern-matches a known class may have a
  different cause. Prefer fresh verbs over resuming corrupted state.
- Track estimate-vs-actual; report actuals to the user whenever a ceiling moved.

## 6. Probes and perceptual claims
- Probe inputs must be constructed by the production code path, never hand-phrased.
- Perceptual acceptance ("looks right", "sounds right") closes only on a named artifact
  the user can inspect — and inspect it yourself before claiming what it shows.
- A probe that refutes the design hypothesis is a SUCCESS: stop, log, don't chase.

## 7. User feedback = findings
Every piece of user watch/read feedback gets root-caused in data and logged with
mechanism + fix direction + cost of the lesson. Fix immediately only if it poisons
in-flight work; otherwise queue it to the matching planned work.

## 8. Model economy
Default execution on the cheaper strong model with smaller waves and this full
procedure. Reserve the frontier model for: adversarial review of new contracts,
root-causing genuinely weird defects, and taste-adjacent design. When a diagnosis
stalls: write the state down and STOP — never a confident improvisation.

## 9. Ending turns and sessions
Never end on a promise. Either the work is done and reported — outcome first, artifacts
named, spend stated — or you are blocked on a NAMED user input, stated plainly. Session
closeout: log + state docs + push + shut down anything running that nothing needs.

## 10. Plugging the capability gap (running on a non-frontier model)
- **Stalled diagnosis → hypothesis tournament:** generate 6 COMPETING root-cause
  hypotheses, each blaming a DIFFERENT layer (input/spec, external-model behavior,
  parsing, caching/identity, schema/data, infra/environment). Each must name the ONE
  query or experiment that discriminates it. Run them; eliminate; recurse on survivors.
  Never fix on an un-discriminated hypothesis — search + data beats brilliance.
- **Implication checklist (a reviewer persona runs it on every design/fix):** does this
  change (1) invalidate or re-key any cached/derived artifact — what regenerates and at
  what cost? (2) strand or silently switch versioned state? (3) couple a stable identity
  to mutable state? (4) have a parallel consumer (estimator, QA mirror, monitor) that
  must change in lockstep? (5) require rebuilding/redeploying something that embeds the
  old behavior? (6) change output that downstream artifacts or users already depend on,
  without a version bump?
- **Frontier clinic:** frontier-model budgets refresh. Park stalled diagnoses, unreviewed
  contracts, and taste calls as written agenda items; spend ONE concentrated frontier
  session on the batch at refresh instead of dribbling the budget away.
