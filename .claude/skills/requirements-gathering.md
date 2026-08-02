# Skill: Requirements Gathering

## When to invoke

Invoke this skill when:
- The user pastes raw, unformatted discovery notes from a client session.
- The user says "structure these notes", "run discovery", "requirements gathering", or `/requirements-gathering`.
- You are at the start of a new engagement and have not yet reached Gate 1.

## What this skill does

1. Reads the raw notes the user provides.
2. Structures them against the discovery checklist below.
3. Produces a structured findings summary in plain language.
4. Identifies gaps and frames clarifying questions.
5. Presents the findings for user confirmation (Gate 1 stop).

---

## Discovery Checklist

Use this checklist to assess completeness of the discovery notes. Mark each item as: **found**, **partial**, or **missing**.

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

Use this template when presenting structured findings to the user.

```
## Discovery Findings — [Client Name / Engagement]

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

### Open Questions (missing or ambiguous)
1. [Question] — [Why it matters for architecture]
2. ...

---
**Gate 1 pause.** Please review the findings above and answer the open questions.
Confirm the scope is correct before I draft architecture options.
```

---

## Template file

See `templates/discovery-findings.md` for a blank copy of the findings output format.
