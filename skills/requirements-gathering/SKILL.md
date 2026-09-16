---
name: requirements-gathering
description: >
  This skill should be used when the user pastes raw, unformatted discovery
  notes from a client session, says "structure these notes", "run
  discovery", "requirements gathering", or "/requirements-gathering", or is
  at the start of a new architecture-lead engagement that has not yet
  reached Gate 1. It ingests notes/ (or pasted text) plus reference/
  compliance docs, distills a plain-language problem statement first,
  structures the rest against a discovery checklist, runs the
  skeptic/practical-planner negotiation, and produces a Gate 1 findings
  summary.
metadata:
  version: "0.3.0"
---

## When to invoke

Invoke this skill when:
- The user pastes raw, unformatted discovery notes from a client session.
- The user says "structure these notes", "run discovery", "requirements gathering", or `/requirements-gathering`.
- You are at the start of a new engagement and have not yet reached Gate 1.

## What this skill does

0. Check the project's `notes/` and `reference/` folders first. If `notes/` has files, read all of them as the raw notes — this is the primary path, not a fallback. Only ask the user to paste notes if `notes/` is empty or missing. If `reference/` has files (compliance policies, standards, approved-vendor lists, etc.), read those too — they aren't discovery notes, they're binding constraints. Never read `archive/` — superseded material there plays no role in findings.
1. **Before anything else, distill the problem.** State, in one to two sentences of plain business language, what problem is actually being solved. If you can't do this confidently from the raw notes, that's the first and highest-priority open question — ahead of any checklist gap below. Everything else in this skill is secondary until this is clear enough to state plainly.
2. Structure the rest of the notes against the discovery checklist below. Fold anything found in `reference/` into the Constraints section, marked `Hard` (compliance/policy constraints are not negotiable the way a soft preference might be).
3. Produce a structured findings summary in plain language.
4. Identify gaps and frame clarifying questions.
5. Dispatch to `skeptic` with a summary of what's known (problem statement, functional/NFR/constraints/stakeholders as structured so far). Dispatch the resulting gap list to `practical-planner`. Reconcile per the Lead's "Gatekeeping and Negotiation" rules into: a blocking Open Questions list, and a non-blocking, explicitly-stated Assumptions list.
6. Present the findings for user confirmation (Gate 1 stop) AND write them to `outputs/discovery-findings.md` (creating `outputs/` with `mkdir -p` if needed).

---

## Discovery Checklist

Use this checklist to assess completeness of the discovery notes. Mark each item as: **found**, **partial**, or **missing**.

### Problem Framing
- [ ] Can the problem be stated in one to two sentences, in plain business language, with no jargon?
- [ ] Is there anything in the notes suggesting different stakeholders would state the problem differently — and if so, has that been reconciled or flagged as unresolved?

### Functional Requirements
- [ ] What the system must *do* (core capabilities)
- [ ] Who the primary users are and what they are trying to accomplish
- [ ] Key workflows or user journeys that must be supported
- [ ] Integration points with existing systems

### Non-Functional Requirements
- [ ] Scale expectations (users, transactions, data volume — with numbers if possible)
- [ ] Latency or performance targets
- [ ] Availability and uptime expectations
- [ ] Security and compliance requirements
- [ ] Data residency or sovereignty constraints

### Constraints
- [ ] Technology constraints (mandated platforms, languages, clouds)
- [ ] Team constraints (skills available, team size)
- [ ] Budget constraints (rough order of magnitude)
- [ ] Timeline constraints (hard deadlines and why they are hard)
- [ ] Regulatory or contractual constraints

### Stakeholders
- [ ] Who is the economic buyer?
- [ ] Who is the technical decision-maker?
- [ ] Who are the end users?
- [ ] Who has veto power or sign-off authority?

### Success Criteria
- [ ] How will the client know this project succeeded?
- [ ] What metrics or outcomes matter most?
- [ ] What does failure look like?

### Unknowns and Risks (as stated by the client)
- [ ] What does the client say they don't know yet?
- [ ] What concerns did they raise unprompted?
- [ ] What assumptions are they making that may not be safe?

---

## Findings Output Format

Use this template when presenting structured findings to the user. The Problem Statement is the first thing in the document — before "What I Heard," before any table.

\`\`\`
## Discovery Findings — [Client Name / Engagement]

### The Problem We're Solving

[One to two sentences, plain business language, no jargon. If a reader reads nothing
else in this document, they should understand why this engagement exists. This must
be confirmed by the user separately from the scope confirmation below — do not treat
Gate 1 as complete on scope confirmation alone.]

### What I Heard

[1–3 paragraph plain-language summary of the client's situation and goals.
No architecture terms. Written as if explaining it to a non-technical colleague.]

### Functional Requirements
| Requirement | Detail | Confidence |
|-------------|--------|------------|
| ...         | ...    | High / Medium / Low |

### Non-Functional Requirements
| Requirement | Detail | Confidence |
|-------------|--------|------------|
| ...         | ...    | High / Medium / Low |

### Constraints
| Constraint | Detail | Hard / Soft |
|------------|--------|-------------|
| ...        | ...    | ...         |

### Stakeholders
| Role | Name / Description | Influence |
|------|--------------------|-----------|
| ...  | ...                | High / Medium / Low |

### Success Criteria
- ...

### Open Questions (missing or ambiguous — blocking)
1. [Question] — [Why it matters for architecture]
2. ...

### Assumptions (Practical Planner sign-off — non-blocking, tracked)
1. [Stated assumption] — [Why this gap has diminishing returns to chase right now; what would trigger revisiting it]
2. ...

---
**Gate 1 pause.** Please confirm the problem statement above is correctly framed,
confirm the scope is correct, and answer the open questions before I draft
architecture options.
\`\`\`

---

## Template file

See `templates/discovery-findings.md` for a blank copy of the findings output format.
