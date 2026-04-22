# /nightingale consult — What should I watch for?

Before a risky operation, consult the incident corpus for relevant past failures.

## Trigger

User says `/nightingale consult <operation>` or agent is about to perform a risky operation.

## Arguments

- `<operation>` — what's about to happen. E.g., "deploy data-daemon", "apply migration", "merge to main", "onboard fleetio"

## Instructions

### Step 1: Load incidents

Read all `.nightingale/incidents/INC-*.yaml` files.

### Step 2: Match

Find incidents where `operation` matches or is related to the requested operation. Also check `tools_involved` for overlap.

### Step 3: Briefing

Output a concise briefing:

```
## Nightingale Briefing: {{operation}}

{{N}} past incidents related to this operation.

### Watch for:
- {{root_cause from INC-X}} — last seen {{date}}
- {{root_cause from INC-Y}} — happened {{N}} times

### Runbook:
Use `stepproof_run_start(template_id="rb-{{name}}")` to bind yourself to the ceremony.

### Unprotected risks:
- {{gap from INC-Z}} — no runbook gate exists for this yet
```

## Rules

- Keep it short. This is a pre-flight briefing, not a history lesson.
- Prioritize repeat offenders — failures that happened more than once are the most important warnings.
- If no incidents match, say so honestly. "No past incidents for this operation" is useful information.
- Always recommend the relevant StepProof runbook if one exists.
