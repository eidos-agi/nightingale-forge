# /nightingale brief — Top unprotected failure modes

Quick snapshot of the project's exposure. The polar area chart.

## Trigger

User says `/nightingale brief` or asks "what's exposed?"

## Instructions

### Step 1: Load incidents and any available protective layers

Read `.nightingale/incidents/`. Then opportunistically read whichever protective layers exist in this project:
- `.stepproof/runbooks/` (if StepProof is in use)
- `.claude/skills/` (skill-based guardrails)
- any forge-specific rule files

An incident is "protected" if *any* of those layers covers it, not only runbooks.

### Step 2: Find the top 5 unprotected failure modes

Rank by: severity (CRITICAL > HIGH > MEDIUM > LOW), then by repeat count (more repeats = higher priority).

### Step 3: Output

```
## Nightingale Brief — {{project}} — {{date}}

### Top unprotected failure modes:
1. [CRITICAL] {{title}} — {{N}} occurrence(s), no protective gate
2. [HIGH] {{title}} — {{N}} occurrence(s), no protective gate
...

### Coverage: {{M}}/{{total}} incidents protected ({{X}}%)

### Action: Add a protective gate for items 1-3. Natural home: {{runbook | skill | forge rule | pre-flight check}}.
```

If coverage is 100%: "All cataloged failure modes are protected."

## Rules

- Five items max. This is a glance, not a report.
- If everything is covered, say so. Don't invent concerns.
