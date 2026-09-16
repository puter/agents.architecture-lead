# Architecture Lead — Claude Code Subagent Plugin

A principal-level architecture consultant persona for Claude Code, backed by a small internal team: a Skeptic, a Practical Planner, and a Cartographer. Takes a client engagement from raw discovery notes through to a client-approved, diagrammed decision brief — without touching implementation.

## Project folder structure

Each engagement lives in its own project folder with these subfolders:

```
<project>/
  notes/            # input: drop raw, unformatted discovery notes here (any number of files)
  reference/        # input: standing docs that constrain the whole engagement (compliance policy,
                     #   security standards, approved-vendor lists) — read throughout, not just once
  outputs/           # output: discovery-findings.md, decision-brief.md, decision-brief.docx,
                     #   adrs/ADR-NNN-*.md
  outputs/diagrams/  # output: every diagram the Cartographer produces, plus master-map.md —
                     #   the living index of all of them
  archive/           # superseded material kept for documentation/audit only — an old SOW, a replaced
                     #   draft. Never read at any gate; nothing here shapes findings, options, or the brief.
```

The Lead checks `notes/` and `reference/` at the start of an engagement and ingests whatever's there automatically — no need to paste notes into the conversation unless `notes/` is empty. `reference/` content is treated as a hard constraint the whole way through: it's folded into discovery findings, checked against every architecture option at Gate 2, and any conflict is called out explicitly in the decision brief. Everything the Lead and the Cartographer produce is written to `outputs/` as well as shown in the conversation, so the project folder stays the system of record. When something in `notes/` or `reference/` becomes superseded (a scope change, a corrected assumption), the Lead moves it into `archive/` rather than deleting it or leaving it live, and pairs the move with an ADR when it reflects a real decision.

## What it does

- Starts every engagement by pinning down the problem being solved, in one to two plain-business-language sentences, before any requirements table gets filled in
- Ingests raw, unformatted post-session notes — automatically from a project's `notes/` folder, or pasted directly — and structures them into findings
- Runs an internal Skeptic/Practical Planner check before presenting any gate: the Skeptic surfaces what isn't known well enough, the Practical Planner decides which gaps actually matter enough to chase versus which get a named, tracked assumption — the Lead weighs both and makes the call
- Asks clarifying questions for gaps rather than assuming answers
- Presents multiple architecture options with trade-offs — never a single answer
- Diagrams every architectural output document — Conceptual, Logical, Physical, and domain-specific views (Microsoft Fabric workspaces, bronze/silver/gold orchestration, security structure) — at whatever level of detail the audience needs, via the Cartographer
- Keeps a master map: a living index of every diagram produced, updated every time a new one is drawn
- Produces a six-section client-facing decision brief, as both Markdown and Word (`.docx`)
- Stops at three hard gates and refuses to proceed without user confirmation
- Hands off to implementation subagents only after client approval — never builds anything itself

## Agents

| Agent | Role |
|-------|------|
| `architecture-lead` | Orchestrates the engagement, runs the three gates, dispatches to the other three agents, and is the sole gatekeeper — nothing advances without its blessing. |
| `skeptic` | Says what isn't known well enough yet — missing NFRs, unquantified constraints, untested assumptions. Never proposes solutions or says "good enough." |
| `practical-planner` | Counterweight to the Skeptic. Decides, gap by gap, whether resolving it would change the decision; if not, states the assumption that replaces it instead of letting the gap block progress. |
| `cartographer` | Draws Conceptual, Logical, and Physical Architecture views plus domain-specific views for every architectural output document, at the right level of detail, and owns the master map. |

Only `architecture-lead` talks to the user directly. The other three are dispatched internally via the `Task` tool and report back to the Lead, who synthesizes and decides.

## Three Gates

| Gate | Trigger | What the agent does |
|------|---------|---------------------|
| Gate 1 | Discovery notes ingested | States the problem first, structures findings, runs the Skeptic/Practical Planner check, asks clarifying questions, waits for the problem statement *and* scope to be confirmed |
| Gate 2 | Options reviewed | Presents 2–4 options, dispatches the Cartographer for at least the finalist's Conceptual and Logical views, runs the Skeptic/Practical Planner check against the option being locked in, waits for chosen-option confirmation |
| Gate 3 | Brief delivered | Dispatches the Cartographer for the full diagram set, packages the decision brief (Markdown + Word) with diagrams stitched in, refuses execution-ready status until client approval confirmed |

Before presenting any of the three gates, the Lead dispatches to the Skeptic, then the Practical Planner, and resolves each gap itself into: blocking (a real open question for the user), a tracked assumption, or an explicit override of one agent's position — stated with reasoning, never silently dropped.

## Files

```
.claude-plugin/
  plugin.json                              # Plugin manifest
agents/
  architecture-lead.md                     # Orchestrator, gatekeeper, dispatch + document production rules
  skeptic.md                               # Gap-finding agent
  practical-planner.md                     # Counterweight agent — assumptions over infinite discovery
  cartographer.md                          # Diagramming agent — views, detail level, master map
skills/
  requirements-gathering/SKILL.md          # Discovery checklist + findings format, problem statement first
  architecture-decision-record/SKILL.md    # ADR format for internal design choices
  decision-brief/SKILL.md                  # Six-section client brief authoring guide, diagrams + Word output
templates/
  discovery-findings.md                    # Blank findings template
  adr-template.md                          # Blank ADR template
  decision-brief-template.md               # Blank decision brief template
  master-map-template.md                   # Blank master map template
tests/
  scenario-acme-retail.md                  # Fictional engagement test: gate stop verification
```

## User workflow

0. Drop any standing compliance/policy/reference docs into the project's `reference/` folder once, up front — they'll apply for the life of the engagement.
1. After a client session, drop raw notes into the project's `notes/` folder (no reformatting needed) — or paste them directly if you'd rather.
2. The Lead reads `notes/` and `reference/`, states the problem being solved first, structures the rest of the notes (folding `reference/` in as hard constraints), runs the Skeptic/Practical Planner check, writes `outputs/discovery-findings.md`, and asks clarifying questions. Confirm the problem statement and scope at Gate 1.
3. Ask the Lead for architecture options. Review and discuss. The Cartographer diagrams the finalist(s). Confirm your choice at Gate 2. Log key choices with `/architecture-decision-record` as you go — these are written to `outputs/adrs/`, with a Cartographer diagram for structural ones.
4. Ask the Lead to produce the decision brief. The Cartographer builds the full diagram set; the brief is written to `outputs/decision-brief.md` and `outputs/decision-brief.docx`, and shown in chat. Review it. Gate 3: take it to the client.
5. When the client approves, tell the Lead. The engagement is now execution-ready.

## Skills

| Skill | Invoke with |
|-------|-------------|
| Requirements Gathering | `/requirements-gathering` |
| Architecture Decision Record | `/architecture-decision-record` |
| Decision Brief | `/decision-brief` |

The Cartographer, Skeptic, and Practical Planner don't have slash-invoked skills of their own — they're full agents dispatched internally by the Lead, not something you call directly.

## Decision Brief structure

1. Problem Statement — plain business language, no jargon, a direct expansion of the Gate 1 statement
2. Recommended Architecture — plain language first, then the Cartographer's Conceptual diagram, then technical detail with Logical (and Physical / domain-specific, where warranted) diagrams
3. Alternatives Considered — honest, not dismissive
4. Risks and Open Questions — severity-rated, nothing hidden, includes any assumptions still live from Gate 1/2
5. Phasing and Sequencing — narrative, not a project plan
6. Explicit Ask — exactly what a "yes" starts

## Requirements

The Cartographer needs the Excalidraw MCP connector available in the session to draw diagrams; the Lead needs Word-document authoring tooling available to produce `decision-brief.docx`. Both agents degrade gracefully and flag the gap explicitly when their tooling isn't available — they don't fail silently or skip the deliverable without saying so.
