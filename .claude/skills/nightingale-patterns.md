# /nightingale patterns — Cluster incidents into cognitive-failure classes

The Florence Nightingale move. Individual incidents tell you what broke once. Pattern clusters tell you what's broken **systemically**.

## Trigger

User says `/nightingale patterns`, or asks "what classes of failure are we making?", or "why do I keep getting bitten by the same kind of thing?"

## Why this exists separate from `/report`

`/nightingale report` compares incidents to StepProof runbooks — it's gate-coupled. Useful when runbooks exist.

`/nightingale patterns` is upstream of any specific gate system. It answers: "regardless of whether we have rules for this, what classes of thinking are repeatedly producing wreckage?" A project with zero runbooks can still run `patterns` productively.

## Instructions

### Step 1: Load incidents

Read every `.nightingale/incidents/*.yaml` in the project. If fewer than 3 exist, output: "Not enough incidents for pattern analysis. Run `/nightingale study` first." and stop.

### Step 2: Cluster by cognitive-failure class

For each incident, look at the `root_cause` and `what_would_catch_it` fields. Group incidents whose root causes share a common cognitive pattern, not just a shared tool or domain.

Example classes seen in agent work:
- **Trust-before-verify** — invoking an unfamiliar tool/API without checking its exact behavior; assuming defaults are safe
- **Narrow surface mapping** — performing a migration/rename by memory instead of enumerating the full surface first
- **Shell-through-shell quoting optimism** — treating nested shell invocations as if they were local; ignoring layer-specific quoting rules
- **Declare-done-before-verify** — assertions that pass for the wrong reason; success reported before downstream verification
- **Cross-boundary metadata loss** — writing across filesystems (UNC, symlinks, container mounts) and forgetting that POSIX permissions/ownership don't survive

The classes are not a fixed list. Invent new ones as the corpus demands. Every class is a *pattern of thinking*, not a *tool-specific bug*.

### Step 3: Output the polar-area-chart equivalent

```
## Nightingale Patterns — {{project}} — {{date}}

Incidents analyzed: {{N}}
Cognitive-failure classes identified: {{M}}

### Classes (ranked by incident count × severity)

1. {{CLASS_NAME}} — {{N_incidents}} incident(s)
   Evidence: {{INC-0001, INC-0004, INC-0007}}
   Pattern: {{one-sentence description of the thinking failure}}
   Suggested gate-type: {{runbook ceremony? | skill update? | forge rule? | pre-flight check?}}

2. ...
```

End with one honest sentence naming the class that ate the most wall-time in this corpus, and one sentence naming whichever existing encoder (StepProof, forge-forge, skills) is the natural home for fixing it.

## Rules

- Cluster on cognitive patterns, NOT tools. "All the `gh repo` mistakes" is a shallow cluster; "trust-before-verify on unfamiliar CLIs" is the real one.
- If an incident doesn't fit any class cleanly, invent a new class. Don't force-fit.
- An incident can belong to more than one class; say so.
- No single incident is a "class of one" — if you find one orphan incident, it's a data point, not a pattern. Name it but don't promote it to a class.
- Never judge. Classes describe systems that failed, not people who failed.

## Output is actionable

The closing lines should answer: **which encoder should act on which class, first?** A class without a downstream owner is just a list — nightingale's job is to hand each class to the forge/skill/runbook that should own it.
