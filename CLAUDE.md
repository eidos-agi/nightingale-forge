# CLAUDE.md — nightingale

> The failure study forge. Reads the wreckage, writes the warning on the door.

Named for Florence Nightingale — she didn't just nurse soldiers. She studied WHY they were dying, invented data visualization to prove it, and changed medicine because the evidence was undeniable.

## Skills

| Skill | What It Does |
|-------|-------------|
| `/nightingale study` | Read full session transcripts, extract structured incident reports |
| `/nightingale report` | Gap analysis — cross-reference incidents against StepProof runbooks |
| `/nightingale consult <op>` | Pre-flight briefing — what went wrong last time someone did this? |
| `/nightingale brief` | Top 5 unprotected failure modes right now |

## Templates

| Template | What It Is |
|----------|-----------|
| `incident.yaml` | Structured incident report: severity, root cause, evidence, runbook gate mapping |

## How It Works

1. `/nightingale study` reads sessions via claude-resume, extracts incidents into `.nightingale/incidents/`
2. `/nightingale report` cross-references incidents against `.stepproof/runbooks/` to find gaps
3. `/nightingale consult` answers "what should I watch for?" before risky operations
4. `/nightingale brief` shows the project's top unprotected failure modes

The incident corpus grows with every study run. The gap report gets more accurate. The consult briefings get richer. Compound interest.

## Guardrails

- **GUARD-001**: No software — skills and templates only
- **GUARD-002**: Document, don't judge — incidents are system failures, not human failures
- **GUARD-003**: Every incident must trace to a gate — covered or gap, no orphan incidents

## Related Forges

- **StepProof** — governance gates. Nightingale finds the gaps; StepProof fills them.
- **forge-forge** — the forge standard. Nightingale follows it.
