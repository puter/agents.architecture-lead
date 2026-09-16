---
name: decision-brief
description: >
  This skill should be used once Gate 2 is complete and the user says
  "write the brief", "draft the client document", "produce the decision
  brief", or "/decision-brief". It dispatches to the cartographer for the
  brief's diagram set, drafts the six-section client-facing decision
  brief with those diagrams stitched into Section 2 — the key output
  artifact of the engagement — and writes it to both
  outputs/decision-brief.md and outputs/decision-brief.docx.
metadata:
  version: "0.3.0"
---

## When to invoke

Invoke this skill when:
- Gate 2 is complete — the user has confirmed the chosen architecture.
- The user asks to "write the brief", "draft the client document", "produce the decision brief", or `/decision-brief`.
- You are ready to produce the client-facing artifact.

This is the key output artifact of the Architecture Lead engagement. It is the document the user takes to the client for approval. It must be self-contained, jargon-free at the executive level, honest about risks, and visual where a picture says it faster than a paragraph.

---

## Pre-flight check

Before drafting, confirm all inputs are available:

- [ ] Problem statement confirmed at Gate 1
- [ ] Discovery findings confirmed at Gate 1
- [ ] Architecture options presented and debated at Gate 2
- [ ] Chosen option confirmed by user
- [ ] At least one ADR capturing the key design choice(s)
- [ ] Risks and open questions surfaced during discovery and design
- [ ] Chosen option checked against everything in `reference/` (if present) — any conflict is named explicitly in Section 4, not glossed over
- [ ] `cartographer` has produced at least a Conceptual and Logical Architecture diagram for the recommended approach — plus Physical and any domain-specific view (Fabric workspace, bronze/silver/gold orchestration, security structure) that this audience and this architecture warrant
- [ ] `outputs/diagrams/master-map.md` reflects every diagram used in this brief

If any of these are missing, identify the gap and ask the user to fill it — or dispatch to the relevant agent (`cartographer` for missing diagrams) — before drafting.

---

## Decision Brief Format

\`\`\`markdown
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
No architecture terms. No technology names. No jargon.
This should read as a direct expansion of the Gate 1 problem statement — not a
new formulation of it.]

---

## 2. Recommended Architecture

[One paragraph plain-language overview of the recommended approach.
State what it is and why it fits the problem — before any technical detail.]

[Conceptual Architecture diagram — from cartographer]

[Then: supporting detail, organized as needed. Keep technical depth proportional
to the audience: enough to be credible, not so much it loses the reader.]

[Logical Architecture diagram — from cartographer. Add Physical and/or domain-specific
diagrams (Fabric workspace, bronze/silver/gold orchestration, security structure) here
if the audience and the architecture warrant them.]

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

[Include any Assumptions carried forward from Gate 1 or Gate 2 that are still live —
label them as assumptions, not settled facts.
If a risk is blocking, name what needs to happen before execution can start.
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
\`\`\`

---

## After drafting

Once the brief is written:

1. Present it to the user for review AND write it to `outputs/decision-brief.md` (creating `outputs/` with `mkdir -p` if needed). Re-write this file on every revision so it always reflects the latest version.
2. Compile the same content into `outputs/decision-brief.docx` using your Word-document authoring skill, with the Cartographer's diagrams embedded as images at the points marked above. This is the copy that's actually client-ready — the Markdown is the working source. Re-generate the `.docx` alongside the `.md` on every revision; never let one drift ahead of the other. If Word-authoring tooling isn't available this session, say so, deliver the Markdown, and note the `.docx` is owed.
3. Revise based on feedback — re-running the Cartographer if the revision changes the architecture materially enough that the existing diagrams no longer match.
4. When the user is satisfied, state the Gate 3 pause:

   > "This brief is ready to take to the client. The engagement is NOT ready for execution subagents until you return and confirm the client has approved it. When they approve, tell me and I will hand off to the appropriate specialists."

5. Do not proceed further until the user returns with explicit client approval.

---

## Template file

See `templates/decision-brief-template.md` for a blank copy.
