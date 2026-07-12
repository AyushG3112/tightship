# tightship

**Run a tight ship with any model.**

A [Claude Code](https://claude.com/claude-code) skill that turns AI coding sessions from
"hope it got it right" into a disciplined process: plans get attacked before they're
built, claims get tested before they're trusted, and everything important gets written
down where the next session can find it.

The core idea: **quality comes from the procedure, not the model.** A mid-tier model
following a tight process ships work you can trust. A frontier model winging it doesn't.

## Why this exists

AI sessions don't usually fail because the model can't do the work. They fail because
the model is **confidently wrong at exactly the moments nobody is checking**:

- It writes a plan that sounds great and has one fatal flaw nobody looked for.
- It assumes "the API surely works like this" and builds three layers on the assumption.
- A sub-agent reports "all tests green" and nothing actually ran.
- It hits a weird error, guesses the cause, and "fixes" the wrong thing.
- The session ends with "Next, I'll…" — and next never comes.

More intelligence doesn't remove these moments. A procedure does. Tightship is that
procedure, in one file the model reads at the start of a session.

## Install

Available in every project on your machine:

```bash
mkdir -p ~/.claude/skills/tightship && \
curl -fsSL https://raw.githubusercontent.com/AyushG3112/tightship/main/SKILL.md \
  -o ~/.claude/skills/tightship/SKILL.md
```

Or commit it to one repo for your whole team: copy `SKILL.md` into
`.claude/skills/tightship/`.

## Use

Type `/tightship` as the **first message** of a session, then give it your goal:

```
/tightship
Add rate limiting to the public API. Budget: this afternoon.
```

Works on a repo with **no documentation at all**: the session's first act is creating a
one-page record (a state doc + a decision log) and it grows only as the project earns
it. Nothing to set up — sessions find "what changed since last time" from git history
on their own.

If you notice a session getting sloppy mid-way — committing without running tests,
guessing at a bug, ending its turn with a promise — type `/tightship` again. It
re-anchors.

## What happens when a session boots

Before touching your task, a tightship session orients itself — every time, in order:

1. **Read the record** — the project's state doc, decision log, and any in-flight plan.
   (No record yet? It writes a one-page starter and moves on.)
2. **Verify against reality** — the record is a claim, not a fact: `git log` confirms
   what was actually done, live systems confirm what's actually running, tests confirm
   what's actually green. When record and reality disagree, reality wins and the record
   gets fixed *first*.
3. **Catch up on what happened without it** — teammates' commits, PR merges, manual
   edits since the record's last entry get a one-line catch-up note, so record silence
   never gets mistaken for "nothing happened." No setup needed for "since when": the
   record files are in git, so the session finds the last commit that touched them
   (`git log -1 -- docs/STATE.md`) and diffs from there (`git log <that>..HEAD`).
4. **Then the wave starts** — with the session knowing exactly where the project stands,
   instead of assuming it from a stale conversation.

Thirty seconds of orientation is what makes multi-day, multi-session, multi-model work
land as one continuous effort.

## What it looks like in practice

### Example 1: the plan gets attacked before it gets built

Without tightship:

> **You:** Add rate limiting to the API.
> **Model:** Great idea! I'll add a middleware using in-memory counters… *[builds it]*
> …Done!
>
> *(Three days later you discover it breaks the moment you run two server instances.)*

With tightship, the model must have its own plan reviewed before building. It spawns a
reviewer that plays skeptic, and the reviewer must **prove** objections, not vibe them:

> **Reviewer (infrastructure skeptic):** BLOCKING — the plan uses in-memory counters,
> but `deploy.yaml` line 14 sets `replicas: 3`. Counters won't be shared across
> instances; limits will be 3× too loose. Fix: shared store or sticky routing.
>
> **Model:** *(folds the fix into the plan, reviewer confirms it, THEN builds)*

The bug never gets written. That's the whole trick: the cheapest place to catch an
error is before it's code.

### Example 2: "prove it or probe it"

Models — and their reviewers — love confident claims about external systems:

> **Reviewer:** This won't work. The payments API doesn't support partial refunds.

Under tightship, that claim isn't allowed to kill the feature, because nobody *proved*
it. Claims about external systems get settled by a **small, cheap, real test** — call
the sandbox API once and look — not by whoever argues most confidently. And it cuts
both ways: you can't build on top of "it surely works" either. Reality is the referee.

### Example 3: trust nothing you didn't verify

When work is delegated to sub-agents, the main session doesn't accept "done, all tests
pass" as prose. It re-runs the test suite itself, checks that tests actually *executed*
(a suite that silently skipped everything also prints green), and reads the diff of
anything load-bearing. Cheap paranoia, applied every time, catches the one time in
twenty that matters.

### Example 4: weird bug? Tournament, not guesswork

When a session hits a bug it can't explain, the tempting move is to guess and patch.
Tightship forces a **hypothesis tournament** instead: write six *competing* explanations,
each blaming a different layer (input, external service, parsing, caching, data,
environment); for each, name the one query or experiment that would prove it wrong; run
them and let the evidence eliminate. Fixes only happen on a surviving hypothesis, never
on the first plausible story.

### Example 5: never end on a promise

A tightship session may not end with "Next, I'll wire up the config." Either the work is
**done and reported** — what shipped, what it cost, where the artifacts are — or it's
**blocked on a named question** only you can answer. And if the session is running out
of context, it writes a resume note precise enough that a brand-new session — even a
different model — picks up mid-stride. The conversation is disposable; the record isn't.

## What's inside

| § | Rule of the ship |
|---|---|
| 1 | The repo is the record — the model's memory is a hint, the written record wins |
| 2 | Real work runs as a wave: written plan → review → build → closeout with actuals |
| 3 | Plans get adversarially reviewed; objections need proof or a probe, never vibes |
| 4 | Delegate the labor, verify the result yourself — every time |
| 5 | Spending real money? Arm the budget first; recover with fresh commands, not resumed wreckage |
| 6 | Quality checks use the real code path and end in an artifact a human can inspect |
| 7 | Every piece of user feedback becomes a logged, root-caused finding |
| 8 | Cheap model + full procedure by default; save the frontier model for the hard 10% |
| 9 | Never end on a promise |
| 10 | When the model isn't brilliant enough: tournaments, checklists, and batching the hard stuff |

## Make it yours

Tightship is deliberately generic. Pair it with a thin **project skill** that fills in
your specifics — which docs to read at boot, your exact test commands, your known
failure playbooks. There's a fill-in-the-brackets starter in
[`PROJECT-SKILL-TEMPLATE.md`](PROJECT-SKILL-TEMPLATE.md). Rule of thumb: every line you
add should name the mistake it prevents.

## License

MIT
