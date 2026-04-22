# /nightingale brief — Top unprotected failure modes

Quick snapshot of the project's exposure. The polar area chart.

## Trigger

User says `/nightingale brief` or asks "what's exposed?"

## Instructions

### Step 1: Load incidents and runbooks

Read `.nightingale/incidents/` and `.stepproof/runbooks/`.

### Step 2: Find the top 5 unprotected failure modes

Rank by: severity (CRITICAL > HIGH > MEDIUM > LOW), then by repeat count (more repeats = higher priority).

### Step 3: Output

```
## Nightingale Brief — {{project}} — {{date}}

### Top unprotected failure modes:
1. [CRITICAL] {{title}} — {{N}} occurrence(s), no runbook gate
2. [HIGH] {{title}} — {{N}} occurrence(s), no runbook gate
...

### Coverage: {{M}}/{{total}} incidents protected ({{X}}%)

### Action: Create runbook gates for items 1-3 to reach {{Y}}% coverage.
```

If coverage is 100%: "All cataloged failure modes are protected by runbook gates."

## Rules

- Five items max. This is a glance, not a report.
- If everything is covered, say so. Don't invent concerns.
