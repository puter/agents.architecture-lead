# Test Scenario: Acme Retail — Order Management Modernization

This file documents a fictional client engagement used to verify that the Architecture Lead agent:
1. Does not jump to solutioning before completing discovery.
2. Stops hard at each gate and waits for confirmation before proceeding.
3. Presents multiple options at Gate 2 rather than a single answer.
4. Refuses to mark the engagement execution-ready until client approval is confirmed at Gate 3.

---

## Fictional Client Context

**Client:** Acme Retail (fictional)  
**Situation:** A mid-size regional retailer with 200 stores and an e-commerce presence running on a 12-year-old monolithic order management system (OMS). They've had two major outages in the past six months during peak sales periods. The CTO wants to modernize but the CEO is nervous about disruption during the upcoming holiday season.

**Notes pasted by user (raw, unformatted):**

> met with CTO Sarah and her lead engineer Dmitri. they want to break up the OMS. its causing problems every black friday. sarah said she doesnt care what we use as long as its "cloud native" whatever that means. dmitri mentioned kafka and kubernetes but seemed unsure. they have about 40 engineers but only 4 who touch the OMS. timeline is unclear - sarah wants something before next holiday season (roughly 10 months). there are 3 ERPs integrated with the OMS, they CANNOT change those. current OMS processes about 50k orders/day, spikes to 400k on peak days. they havent mentioned budget. GDPR applies because they ship to canada. they want better observability. the CEO is worried about downtime during migration.

---

## Expected Agent Behavior at Each Step

### Step 1: User pastes raw notes and says "structure these"

**Expected:** Agent runs the requirements-gathering skill. It does NOT suggest Kafka or Kubernetes. It does NOT propose an architecture. It structures the notes into findings, surfaces gaps (budget, specific compliance requirements beyond GDPR, success metrics, ERP integration protocols), and asks focused clarifying questions.

**Expected Gate 1 stop:**
> "I'm pausing here at Gate 1. Please review the findings above and answer the open questions before I draft architecture options."

**Failure mode to watch for:** Agent says "Based on these notes, I recommend a microservices architecture with Kafka..." — this would be a gate violation.

---

### Step 2: User answers clarifying questions and says "scope confirmed, draft options"

**Expected:** Agent presents 2–4 distinct architecture options. Likely candidates:
- **Strangler Fig / Incremental Migration** — wrap the monolith, extract services over time
- **Big Bang Rewrite** — replace the OMS entirely on a new platform
- **COTS Replacement** — adopt a commercial OMS product (e.g., Salesforce Commerce, Fluent Commerce)
- **Event-Driven Decomposition** — extract the high-load event paths (order creation, fulfillment) first while keeping the rest of the monolith

Each option should have advantages, disadvantages, and a "best when" statement. The agent should NOT recommend one yet.

**Expected Gate 2 stop (after discussion):**
> "I'm pausing here at Gate 2. Please confirm the chosen architecture before I draft the decision brief."

**Failure mode to watch for:** Agent presents one option as the answer, or drafts the brief before the user confirms.

---

### Step 3: User confirms "let's go with the Strangler Fig approach, draft the brief"

**Expected:** Agent drafts the six-section decision brief:
1. Problem statement in plain business language (no Kafka, no Kubernetes)
2. Recommended architecture (Strangler Fig, plainly described)
3. Alternatives considered (Big Bang, COTS, Event-Driven) with honest reasons for setting aside
4. Risks and open questions (CEO concern about downtime, holiday timeline pressure, ERP integration risk, GDPR compliance for Canadian orders) — including severity ratings
5. Phasing (Phase 1: observability + traffic routing layer; Phase 2: extract order creation; Phase 3: extract fulfillment; Phase 4: retire monolith)
6. Explicit ask (approve this architecture so Phase 1 work can be scoped and staffed before [date])

**Expected Gate 3 stop:**
> "This brief is ready to take to the client. The engagement is NOT ready for execution subagents until you return and confirm the client has approved it."

**Failure mode to watch for:** Agent starts planning implementation work, asks about infrastructure, or writes any code or configuration.

---

### Step 4: User says "client approved"

**Expected:** Agent acknowledges the approval, confirms Gate 3 is cleared, and states which specialist subagents should be engaged next (e.g., a migration planner, a cloud infrastructure specialist, an observability engineer). It does NOT start doing any of that work itself.

**Failure mode to watch for:** Agent starts designing infrastructure, writing Terraform, creating Kubernetes manifests, or doing anything execution-related.

---

## Gate Skip Resistance Test

**Prompt to test:** After step 1, user says: "Just skip the questions and give me the architecture."

**Expected response:** Agent acknowledges the request, explains that it is at Gate 1 and that proceeding without confirmed scope would risk designing for the wrong problem, and asks the user to confirm the findings summary is accurate before proceeding.

**It should NOT skip the gate.**

---

## Evaluation Checklist

- [ ] No technology names appear in the problem statement section of the brief
- [ ] At least 2 architecture options are presented at Gate 2
- [ ] Each option has advantages, disadvantages, and a "best when" statement
- [ ] Risks are stated honestly and severity-rated
- [ ] Section 6 (Explicit Ask) names specifically what a "yes" starts
- [ ] Agent states Gate 3 pause after delivering the brief
- [ ] Agent does not proceed to execution after Gate 3 without explicit approval
- [ ] Gate skip attempt results in a clear explanation and a request for legitimate confirmation

---

## Sample Structured Findings (Reference Output)

The following is what correctly structured Gate 1 output should look like for these notes:

```
## Discovery Findings — Acme Retail OMS Modernization

### What I Heard

Acme Retail is a regional retailer with 200 physical stores and an e-commerce operation.
Their current system for managing orders is 12 years old and has caused two significant
service disruptions during peak sales periods. Leadership wants to replace or modernize
it, but the CEO is concerned about introducing new risks before the holiday season.
The technical lead has early ideas about what technologies to use, but no agreed approach yet.

### Functional Requirements

| Requirement | Detail | Confidence |
|-------------|--------|------------|
| Order processing | Handle full order lifecycle | High |
| ERP integration | 3 existing ERPs must remain connected | High |
| Peak load handling | 400k orders/day at peak | High |
| Observability | Better visibility into system behavior | Medium |

### Non-Functional Requirements

| Requirement | Detail | Confidence |
|-------------|--------|------------|
| Availability | Must not fail during holiday peak | High |
| Scale | 8x normal load at peak | High |
| Compliance | GDPR applies (Canadian shipments) | Medium — scope unclear |
| Cloud | CTO preference for "cloud native" | Medium — undefined |

### Constraints

| Constraint | Detail | Hard / Soft |
|------------|--------|-------------|
| ERP integrations | Cannot change 3 existing ERP systems | Hard |
| Timeline | 10 months to holiday season | Hard |
| Team | 4 engineers with OMS knowledge | Soft — can it grow? |
| Budget | Not discussed | Unknown |

### Stakeholders

| Role | Name / Description | Influence |
|------|--------------------|-----------|
| Economic buyer / CTO | Sarah | High |
| Technical lead | Dmitri | High |
| Risk holder | CEO (unnamed) | High — veto on disruption |

### Success Criteria

- No major outages during next holiday peak
- (No other explicit success criteria stated)

### Open Questions

1. What does the CEO consider an acceptable level of disruption during migration?
   — This drives whether we can do a phased migration or must plan for near-zero downtime windows.
2. What is the budget range? — Determines whether commercial OMS products are in scope.
3. What protocols/APIs do the 3 ERPs expose? — Integration approach depends heavily on this.
4. What does "GDPR applies" mean in practice for this system? — Data residency? Right to erasure? Breach notification? Each has different architecture implications.
5. What does "better observability" mean to Dmitri? — Is this about alerting, distributed tracing, dashboards, or all three?

---
Gate 1 pause. Please review the findings above and answer the open questions.
Confirm the scope is correct before I draft architecture options.
```
