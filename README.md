# nightingale

Failure study forge for AI agent teams. Reads session transcripts and extracts structured incident reports. Incidents feed whatever encodes "don't do this again" — governance runbooks (StepProof), forge improvements (forge-forge), skill/runbook updates generally, or continuity artifacts (cockpit-mcp). Nightingale is upstream of all of them.

Named for Florence Nightingale — she didn't just tend soldiers, she studied WHY they were dying at systemic scale, proved it with data visualization, and changed medicine because the evidence was undeniable. The job here is the same: systemic pattern extraction from the wreckage, in a form downstream encoders can act on.

## Skills

```bash
/nightingale study              # Read sessions, extract incidents
/nightingale study <session_id> # Study a specific session
/nightingale study recent       # Study the 5 most recent sessions

/nightingale patterns           # Cluster incidents into cognitive-failure classes
/nightingale report             # Gap analysis: incidents vs StepProof runbooks (if present)
/nightingale consult <operation> # "What should I watch for?"
/nightingale brief              # Top 5 unprotected failure modes
```

## How It Works

Any project can have a `.nightingale/incidents/` directory. Nightingale populates it by reading full session transcripts (via claude-resume) and extracting structured incident reports.

Each incident is actionable in at least one of three ways:
- **Gate-mappable** — if StepProof runbooks exist, the incident maps to a gate (`COVERED` or `GAP`). `/nightingale report` produces the gap report — the polar area chart.
- **Pattern-classable** — even without runbooks, an incident belongs to a cognitive-failure class (e.g. "trust-before-verify on unfamiliar APIs", "shell-through-shell quoting optimism"). `/nightingale patterns` clusters incidents into classes; the class distribution is the systemic view Florence Nightingale was famous for.
- **Consumer-routable** — incidents flow out to forge-forge, skills, cockpit-mcp continuity layers, or any other encoder of "don't do this again."

An incident with no mapping in any of these three ways is the only kind of orphan — and that's a signal the incident corpus itself is undercooked, not that the incident is invalid.

## Setup

Clone and symlink the skills:

```bash
git clone git@github.com:eidos-agi/nightingale.git ~/repos/nightingale
# Skills are available when Claude Code is launched from a directory with this repo accessible
```

## The Loops

Nightingale has more than one downstream. Pick the loop that matches what's in the project today:

```
study → incidents grow → report   → gaps identified   → StepProof gates added
                      → patterns  → classes identified → forge-forge rules / skills updated
                      → consult   → pre-flight briefing for risky operations
                      → brief     → top exposed failure modes right now
```

Every study run makes the corpus richer. Every report tightens a different fence. The forge is the machine; the incidents are the compound interest.

## License

MIT
