# nightingale

Failure study forge for AI agent teams. Reads session transcripts, extracts structured incident reports, and cross-references against governance runbooks to find unprotected failure modes.

Named for Florence Nightingale — she studied WHY soldiers were dying, proved it with data visualization, and changed medicine because the evidence was undeniable.

## Skills

```bash
/nightingale study              # Read sessions, extract incidents
/nightingale study <session_id> # Study a specific session
/nightingale study recent       # Study the 5 most recent sessions

/nightingale report             # Gap analysis: incidents vs StepProof runbooks
/nightingale consult <operation> # "What should I watch for?"
/nightingale brief              # Top 5 unprotected failure modes
```

## How It Works

Any project can have a `.nightingale/incidents/` directory. Nightingale populates it by reading full session transcripts (via claude-resume) and extracting structured incident reports.

Each incident maps to a StepProof runbook gate — either `COVERED` (a gate exists) or `GAP` (no gate, recommendation provided). The gap report is the polar area chart: it makes the exposure undeniable.

## Setup

Clone and symlink the skills:

```bash
git clone git@github.com:eidos-agi/nightingale.git ~/repos/nightingale
# Skills are available when Claude Code is launched from a directory with this repo accessible
```

## The Loop

```
study → incidents grow → report → gaps identified → runbook gates added → consult → safer operations
```

Every study run makes the corpus richer. Every gap report makes the runbooks tighter. The forge is the machine; the incidents are the compound interest.

## License

MIT
