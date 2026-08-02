# Skill: Architecture Decision Record

## When to invoke

Invoke this skill when:
- A specific design choice has been made or is being considered during the Gate 2 conversation.
- The user asks to "log a decision", "write an ADR", "capture this choice", or `/architecture-decision-record`.
- You want to make a design decision traceable before it gets buried in conversation.

ADRs are living documents — they capture choices as they are made, including choices that may later be revisited. They are not the client-facing brief; they are the internal audit trail.

---

## ADR Format

Each ADR is a single file. File naming convention: `ADR-NNN-short-title.md` (e.g., `ADR-001-event-bus-selection.md`).

```markdown
# ADR-[NNN]: [Short Title]

**Date:** [YYYY-MM-DD]
**Status:** [Proposed | Accepted | Superseded by ADR-NNN | Deprecated]
**Engagement:** [Client / Project name]

---

## Context

[1–2 paragraphs describing the situation that forced this decision.
What was happening? What options existed? What constraints were active?
Write in past tense as if explaining it to a future reader who wasn't in the room.]

## Decision

[One clear sentence stating what was decided.
Then 1–2 paragraphs explaining the reasoning.
Do not hedge here — state what was chosen and why.]

## Options Considered

| Option | Summary | Why not chosen |
|--------|---------|----------------|
| [Name] | [One sentence] | [Honest reason] |
| ...    | ...     | ...            |

## Consequences

**Positive:**
- ...

**Negative / trade-offs:**
- ...

**Risks accepted:**
- ...

## Open Questions

[Any unresolved issues that remain after this decision.
These should be tracked and revisited.]

---
*This ADR was captured as part of the Architecture Lead engagement workflow.*
```

---

## Usage notes

- Create one ADR per discrete decision. Do not bundle multiple choices into one ADR.
- The Status field must be kept current. If a later decision supersedes this one, update the status and link the superseding ADR.
- ADRs are internal artifacts. They inform the decision brief but are not sent to the client.
- Store ADRs in `adrs/` at the project root.

---

## Template file

See `templates/adr-template.md` for a blank copy.
