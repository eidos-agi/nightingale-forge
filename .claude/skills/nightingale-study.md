# /nightingale study — Read sessions, extract incidents

Study one or more sessions for failures. Not keyword search — full investigative reading.

## Trigger

User says `/nightingale study`, `/nightingale study <session_id>`, or `/nightingale study recent`.

## Arguments

- `<session_id>` — study a specific session
- `recent` — study the 5 most recent sessions
- `all` — study all unstudied sessions (check `.nightingale/studied.txt` for already-processed)

## Instructions

### Step 1: Load the session

Use `mcp__claude-resume__read_session` to get the full transcript. If studying `recent`, use `mcp__claude-resume__recent_sessions` to find them.

### Step 2: Read like an incident investigator

Read the FULL session. Do not keyword search. Look for:

1. **Explicit failures** — errors, exceptions, "that broke", rollbacks
2. **Silent failures** — agent said "done" but it wasn't, something looked right but was wrong
3. **Ceremony bypasses** — agent skipped a step it should have taken, used raw psql instead of migration tool, shipped without testing
4. **Discovery gaps** — a problem from session N wasn't found until session N+3
5. **Repeat offenders** — same failure happening again that happened before
6. **Near misses** — something almost went wrong, caught at the last moment

### Step 3: Write incident reports

For each failure found, create a file in the project's `.nightingale/incidents/` directory using the template at `~/repos/nightingale/templates/incident.yaml`.

Key fields:
- **severity**: CRITICAL = production data at risk. HIGH = deploy/data failure. MEDIUM = process failure. LOW = friction/annoyance.
- **root_cause**: The system-level reason, not "the agent made a mistake." WHY did the system allow the mistake?
- **runbook_gate**: Check the project's `.stepproof/runbooks/` directory. Does a gate exist that would catch this? If yes: status=COVERED. If no: status=GAP with a recommendation.
- **evidence**: Include actual quotes from the session transcript.

### Step 4: Update studied list

Append the session_id to `.nightingale/studied.txt` so it's not re-studied.

### Step 5: Report

Print a summary: N incidents found, M gaps identified, K already covered by runbook gates.

## Rules

- Read the FULL session. Not summaries. Not keywords. The full transcript.
- Document, don't judge. Incidents are system failures, not human failures (GUARD-002).
- Every incident must trace to a gate (GUARD-003). No incident report is complete without a runbook mapping.
- If you find a repeat of a previously cataloged incident, link them. Repeats are the highest-signal finding.
- Be honest about severity. Don't inflate to seem thorough. Don't deflate to seem positive.
