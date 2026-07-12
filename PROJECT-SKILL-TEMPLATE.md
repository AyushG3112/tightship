# Project-skill template — the domain layer over /tightship

Tightship is deliberately generic. Each serious project pairs it with a THIN project
skill that binds the rules to your domain. Copy this into your repo as
`.claude/skills/operate/SKILL.md`, fill the brackets, delete what doesn't apply, and
keep it under ~100 lines forever — a binding layer that needs scrolling won't be followed.

```markdown
---
name: operate
description: Run a work session on <PROJECT> at the tightship bar. Invoke at the START of any substantive session, or when resuming mid-wave.
---

# Operate: <PROJECT> session procedure

**This skill specializes the user-level `/tightship` skill with THIS repo's bindings.
FIRST ACTION on invocation: also invoke the `tightship` skill so its rules are actually
in context — a reference is not an invocation.** Everything below is the
project-specific layer.

## 0. Boot (every session, every post-compaction continuation)
1. Read <YOUR STATE DOCS, IN ORDER — e.g. the always-loaded project brief's "current
   position" section → the state handoff doc → the decision log → the active plan>.
2. Trust only the repo. If a doc contradicts your memory, the doc wins.
3. Identify: what is IN FLIGHT, what is FENCED (needs explicit fresh authorization),
   what is yours to do.

## 1. The wave shape here
- Contracts live in <docs/plans-or-milestones-dir>; a contract has: STATUS block, slice
  table, sources, budget, stop conditions, and a review-findings section.
- Spend/scope ceilings come from <the owner> up front, with priced options.

## 2. Gates (every slice, no exceptions)
<THE EXACT COMMAND LIST — build, format check, linters, unit tests, integration tests,
plus any run-twice or flaky-class rules, plus environment vars a cold session needs.>

## 3. Delegation bounds
- Files/areas agents must not touch concurrently: <list the collision-prone spots —
  e.g. sequential migration numbers, generated files, shared fixtures>.
- Verification the session must do ITSELF: <e.g. run the integration battery, read the
  diff hunks carrying invariants, count executed tests>.

## 4. Live operations
- Ritual before anything real: <credentials source, env exports, binary rebuilds —
  name every binary that embeds config/seeds/code and when it goes stale>.
- Budget/limit mechanism: <how to arm it, what the gate prices, what "rejected but
  actuals are healthy" means here>.
- Failure playbooks (by name/number so they're citeable):
  - <SYMPTOM> → <EXACT RECOVERY VERB> (never <THE TEMPTING WRONG VERB>)
  - <SYMPTOM> → <RECOVERY>
- External-vendor failure classes that are NOT this system's fault: <e.g. vendor billing
  caps, rate limits — where to look, who fixes them>.

## 5. Probes
- Probe inputs come from <the production code path that builds real requests>.
- Perceptual/quality acceptance closes on artifacts delivered to <where the owner
  reviews — folder/location>, inspected by the session first.

## 6. Findings
- Owner feedback = findings, logged in <the findings/implementation log> with mechanism,
  fix direction, and cost of the lesson. Queue to <the matching planned-work docs>
  unless it poisons in-flight work.

## 7. Compact-proof
- State docs that must move in the SAME commit as any state change: <list them>.
- In-flight banners live in <location> and are zero-context resume procedures.

## 8. Model economy
- Default model: <X>. Reserve <frontier model> for: new-contract review, weird-defect
  root-causing, taste calls. Stalled diagnosis = compact-proof + STOP.

## 9. Session end
- Closeout = <log entry + decision record + state docs + push + shut down <services>>.
```

**Writing tips:** every bracket you fill should answer "what would a cold, capable model
get wrong here on day one?" If you can't name the failure a line prevents, cut the line.
The failure playbooks are the highest-value section — write them as SYMPTOM → VERB pairs
the moment each incident happens, not from memory later.
