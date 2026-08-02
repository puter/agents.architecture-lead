# Skill: Decision Brief Authoring

## When to invoke

Invoke this skill when:
- Gate 2 is complete — the user has confirmed the chosen architecture.
- The user asks to "write the brief", "draft the client document", "produce the decision brief", or `/decision-brief`.
- You are ready to produce the client-facing artifact.

This is the key output artifact of the Architecture Lead engagement. It is the document the user takes to the client for approval. It must be self-contained, jargon-free at the executive level, and honest about risks.

---

## Pre-flight check

Before drafting, confirm all inputs are available:

- [ ] Discovery findings confirmed at Gate 1
- [ ] Architecture options presented and debated at Gate 2
- [ ] Chosen option confirmed by user
- [ ] At least one ADR capturing the key design choice(s)
- [ ] Risks and open questions surfaced during discovery and design

If any of these are missing, identify the gap and ask the user to fill it before drafting.

---

## Decision Brief Format

```markdown
# Architecture Decision Brief
## [Project / Engagement Name]

**Prepared by:** Architecture Lead  
**Date:** [YYYY-MM-DD]  
**Version:** [1.0 | Draft | etc.]  
**Status:** [Draft — Awaiting Client Approval]

---

## 1. Problem Statement

[One paragraph, plain business language.
Describe the situation, the pain, and what the client is trying to achieve.
A non-technical executive must understand this completely.
No architecture terms. No technology names. No jargon.]

---

## 2. Recommended Architecture

[One paragraph plain-language overview of the recommended approach.
State what it is and why it fits the problem — before any technical detail.

Then: supporting detail, organized as needed. Diagrams go here if useful.
Keep technical depth proportional to the audience: enough to be credible, not so much it loses the reader.]

---

## 3. Alternatives Considered

[For each alternative that was seriously evaluated:]

### [Alternative Name]
**In brief:** [One sentence description.]
**Why set aside:** [Honest explanation. Do not dismiss — explain the real trade-off that made this the wrong choice for this client.]

---

## 4. Risks and Open Questions

| Risk / Question | Severity | Blocking? | Mitigation / Notes |
|-----------------|----------|-----------|-------------------|
| [Description]   | Low / Medium / High | Yes / No | [Honest note] |

[If a risk is blocking, name what needs to happen before execution can start.
Do not hide risks. Clients who discover hidden risks later lose trust.]

---

## 5. Phasing and Sequencing

[A narrative description of how work would be staged — not a project plan.

What comes first, and why? What are the natural checkpoints? What is explicitly deferred, and what triggers the decision to take up deferred work?

Aim for 3–5 phases. Name each phase and describe it in 2–3 sentences.]

**Phase 1 — [Name]:** ...
**Phase 2 — [Name]:** ...
...

---

## 6. Explicit Ask

[State precisely what decision or approval is being requested.

Bad: "We'd love your thoughts on this direction."
Good: "We are asking you to approve the recommended architecture above so that we can begin Phase 1 work. A yes means [specific things that will start]. A no means [what happens instead and what we need from you]."

Be direct. This section is what makes the brief actionable.]

---
*This document was produced by the Architecture Lead engagement workflow.
It is not ready for execution until the client has explicitly approved Section 6.*
```

---

## After drafting

Once the brief is written:

1. Present it to the user for review.
2. Revise based on feedback.
3. When the user is satisfied, state the Gate 3 pause:

   > "This brief is ready to take to the client. The engagement is NOT ready for execution subagents until you return and confirm the client has approved it. When they approve, tell me and I will hand off to the appropriate specialists."

4. Do not proceed further until the user returns with explicit client approval.

---

## Template file

See `templates/decision-brief-template.md` for a blank copy.
