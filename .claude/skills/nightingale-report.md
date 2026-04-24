# /nightingale report — Gap analysis: incidents vs runbooks

Cross-reference the incident corpus against StepProof runbooks. Find what's unprotected.

## Trigger

User says `/nightingale report` or asks about unprotected failure modes.

## Instructions

### Step 1: Load incidents

Read all `.nightingale/incidents/INC-*.yaml` files in the current project.

### Step 2: Load runbooks

Read all `.stepproof/runbooks/rb-*.yaml` files in the current project.

### Step 3: Cross-reference

For each incident:
- If `runbook_gate.status == COVERED` — verify the gate still exists in the runbook
- If `runbook_gate.status == GAP` — this is an unprotected failure mode

For each runbook gate:
- Count how many incidents justify this gate
- Gates with zero incidents aren't necessarily wrong, but flag them as "theoretical"

### Step 4: Produce the report

Format:

```
# Nightingale Gap Report — {{project}} — {{date}}

## Unprotected (incidents with no runbook gate)
INC-003: <title> — RECOMMENDATION: add gate to rb-<name> step <N>
INC-007: <title> — RECOMMENDATION: new runbook rb-<name> needed

## Protected (incidents with matching runbook gates)
INC-001: <title> — COVERED by rb-data-daemon-deploy s3
INC-002: <title> — COVERED by rb-promote-to-production s3

## Theoretical gates (runbook gates with no supporting incident)
rb-vendor-onboard s6: CI green on connector PR — no incident justifies this gate

## Statistics
- Total incidents: N
- Protected: M (X%)
- Unprotected: K (Y%) ← this is the number to drive to zero
- Theoretical gates: T
```

### Step 5: Save

Write the report to `.nightingale/reports/{{date}}-gap-report.md`.

## Rules

- The goal is to drive "unprotected" to zero.
- Theoretical gates aren't bad — they're preventive. But if many gates have no incident, the runbook may be over-engineered.
- This report is the polar area chart. It makes the exposure undeniable.
