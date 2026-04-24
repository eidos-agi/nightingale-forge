# CLAUDE.md — nightingale

> The failure study forge. Reads the wreckage, writes the warning on the door.

Named for Florence Nightingale — she didn't just nurse soldiers. She studied WHY they were dying, invented data visualization to prove it, and changed medicine because the evidence was undeniable.

## Skills

| Skill | What It Does |
|-------|-------------|
| `/nightingale study` | Read full session transcripts, extract structured incident reports |
| `/nightingale patterns` | Cluster incidents into cognitive-failure classes — the Florence Nightingale systemic view |
| `/nightingale report` | Gap analysis — cross-reference incidents against StepProof runbooks (when present) |
| `/nightingale consult <op>` | Pre-flight briefing — what went wrong last time someone did this? |
| `/nightingale brief` | Top 5 unprotected failure modes right now |

## Templates

| Template | What It Is |
|----------|-----------|
| `incident.yaml` | Structured incident report: severity, root cause, evidence, runbook gate mapping |

## How It Works

1. `/nightingale study` reads sessions via claude-resume, extracts incidents into `.nightingale/incidents/`
2. `/nightingale patterns` clusters the incident corpus into cognitive-failure classes — the systemic view
3. `/nightingale report` cross-references incidents against `.stepproof/runbooks/` to find gaps (only when StepProof is in the project)
4. `/nightingale consult` answers "what should I watch for?" before risky operations
5. `/nightingale brief` shows the project's top unprotected failure modes

The incident corpus grows with every study run. Whichever downstream encoder is in play — StepProof gates, forge-forge rules, skills updates, cockpit-mcp continuity artifacts — consumes the corpus. Compound interest.

## Guardrails

- **GUARD-001**: No software — skills and templates only
- **GUARD-002**: Document, don't judge — incidents are system failures, not human failures
- **GUARD-003**: Every incident must be actionable in at least one way — mapped to a gate (StepProof), classified to a pattern (`/nightingale patterns`), or routed to a consumer (forge-forge, skills, cockpit-mcp). An incident that fits none is a signal the corpus is undercooked, not that the incident is invalid.

## Related Forges

- **StepProof** — governance gates. Nightingale finds the gaps; StepProof fills them. (First and loudest consumer — nightingale logs from greenmark-cockpit are what motivated StepProof to exist.)
- **forge-forge** — forge standards. Nightingale incidents reveal patterns that harden forges themselves.
- **resume-resume** — session transcript index. Nightingale reads through resume-resume to study wreckage.
- **cockpit-mcp** — continuity substrate. Nightingale incidents are one of the artifacts surfaced at takeoff/land.
- **skills / runbooks generally** — any agent guardrail system can consume incidents as new rules.
