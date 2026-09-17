# Architecture Lead — Claude Code Subagent Plugin

A principal-level architecture consultant persona for Claude Code, backed by a small internal team: a Skeptic, a Practical Planner, a Cartographer, and a Project Manager. Takes a client engagement from raw discovery notes through to a client-approved, diagrammed decision brief — without touching implementation.

## Project folder structure

Each engagement lives in its own project folder with these subfolders:

```
<project>/
  notes/                      # input: drop raw, unformatted discovery notes here (any number of files)
  reference/                  # input: standing docs that constrain the whole engagement (compliance
                               #   policy, security standards, approved-vendor lists) — read throughout
  outputs/                    # output: discovery-findings.md, decision-brief.md, decision-brief.docx,
                               #   estimate.md, security-matrix.md, fabric-workspace-matrix.md,
                               #   adrs/ADR-NNN-*.md
  outputs/diagrams/           # output: every diagram the Cartographer produces, plus master-map.md —
                               #   the living index of all of them
  outputs/status/             # output: the Project Manager's weekly close-outs, status-YYYY-MM-DD.md
                               #   plus log.md, a running rollup
  archive/                    # superseded material kept for documentation/audit only — an old SOW, a
                               #   replaced draft. Never read at any gate; the workspace-maintenance
                               #   skill proposes what moves here, never moves anything unconfirmed.
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
- Budgets hours by role, sequences work against deadlines, and closes out each week with a status report — via the Project Manager
- Produces a security matrix and, for Microsoft Fabric engagements, a workspace matrix, alongside the Cartographer's corresponding diagrams
- Keeps the project folder tidy as the engagement matures — the workspace-maintenance skill proposes archiving stale notes and superseded drafts, never deleting, never without confirmation
- Stops at three hard gates and refuses to proceed without user confirmation
- Hands off to implementation subagents only after client approval — never builds anything itself

## Agents

| Agent | Role |
|-------|------|
| `architecture-lead` | Orchestrates the engagement, runs the three gates, dispatches to the other agents, and is the sole gatekeeper — nothing advances without its blessing. |
| `skeptic` | Says what isn't known well enough yet — missing NFRs, unquantified constraints, untested assumptions. Never proposes solutions or says "good enough." |
| `practical-planner` | Counterweight to the Skeptic. Decides, gap by gap, whether resolving it would change the decision; if not, states the assumption that replaces it instead of letting the gap block progress. |
| `cartographer` | Draws Conceptual, Logical, and Physical Architecture views plus domain-specific views for every architectural output document, at the right level of detail, and owns the master map. |
| `project-manager` | Budgets hours by role (as ranges with stated confidence, never false precision), sequences work against deadlines, and produces weekly close-out status reports. Also runs `workspace-maintenance` at each close-out. |

Only `architecture-lead` talks to the user directly by default. `skeptic`, `practical-planner`, and `cartographer` are dispatched internally via the `Task` tool and report back to the Lead, who synthesizes and decides. `project-manager` is the exception — it's also directly invocable by the user for anything project-management shaped, independent of the gate cycle.

## Three Gates

| Gate | Trigger | What the agent does |
|------|---------|---------------------|
| Gate 1 | Discovery notes ingested | States the problem first, structures findings, runs the Skeptic/Practical Planner check, asks clarifying questions, waits for the problem statement *and* scope to be confirmed |
| Gate 2 | Options reviewed | Presents 2–4 options, dispatches the Cartographer for at least the finalist's Conceptual and Logical views, dispatches the Project Manager for a rough-order estimate and sequencing check, runs the Skeptic/Practical Planner check against the option being locked in, waits for chosen-option confirmation |
| Gate 3 | Brief delivered | Dispatches the Cartographer for the full diagram set, dispatches the Project Manager for the full budget estimate, produces the security matrix (and Fabric workspace matrix, if applicable), packages the decision brief (Markdown + Word) with diagrams stitched in, refuses execution-ready status until client approval confirmed |

Before presenting any of the three gates, the Lead dispatches to the Skeptic, then the Practical Planner, and resolves each gap itself into: blocking (a real open question for the user), a tracked assumption, or an explicit override of one agent's position — stated with reasoning, never silently dropped.

## Files

```
.claude-plugin/
  plugin.json                              # Plugin manifest
.mcp.json                                  # Excalidraw MCP server (hosted API — see Requirements)
agents/
  architecture-lead.md                     # Orchestrator, gatekeeper, dispatch + document production rules
  skeptic.md                               # Gap-finding agent
  practical-planner.md                     # Counterweight agent — assumptions over infinite discovery
  cartographer.md                          # Diagramming agent — views, detail level, master map
  project-manager.md                       # Budgeting, sequencing, weekly close-out agent
skills/
  requirements-gathering/SKILL.md          # Discovery checklist + findings format, problem statement first
  architecture-decision-record/SKILL.md    # ADR format for internal design choices
  decision-brief/SKILL.md                  # Six-section client brief authoring guide, diagrams + Word output
  security-matrix/SKILL.md                 # Asset/threat/control mapping behind a Security diagram
  fabric-workspace-matrix/SKILL.md         # Workspace/capacity/RBAC detail behind a Fabric diagram
  workspace-maintenance/SKILL.md           # Non-destructive archiving of stale notes and drafts
templates/
  discovery-findings.md                    # Blank findings template
  adr-template.md                          # Blank ADR template
  decision-brief-template.md               # Blank decision brief template
  master-map-template.md                   # Blank master map template
  budget-estimate-template.md              # Blank hours-by-role budget template
  weekly-status-template.md                # Blank weekly close-out template
  security-matrix-template.md              # Blank security matrix template
  fabric-workspace-matrix-template.md      # Blank Fabric workspace matrix template
tests/
  scenario-acme-retail.md                  # Fictional engagement test: gate stop verification
```

## User workflow

0. Drop any standing compliance/policy/reference docs into the project's `reference/` folder once, up front — they'll apply for the life of the engagement.
1. After a client session, drop raw notes into the project's `notes/` folder (no reformatting needed) — or paste them directly if you'd rather.
2. The Lead reads `notes/` and `reference/`, states the problem being solved first, structures the rest of the notes (folding `reference/` in as hard constraints), runs the Skeptic/Practical Planner check, writes `outputs/discovery-findings.md`, and asks clarifying questions. Confirm the problem statement and scope at Gate 1.
3. Ask the Lead for architecture options. Review and discuss. The Cartographer diagrams the finalist(s). Confirm your choice at Gate 2. Log key choices with `/architecture-decision-record` as you go — these are written to `outputs/adrs/`, with a Cartographer diagram for structural ones.
4. Ask the Lead to produce the decision brief. The Cartographer builds the full diagram set, the Project Manager builds the budget estimate, the security matrix (and Fabric workspace matrix, if applicable) are produced; the brief is written to `outputs/decision-brief.md` and `outputs/decision-brief.docx`, and shown in chat. Review it. Gate 3: take it to the client.
5. When the client approves, tell the Lead. The engagement is now execution-ready.
6. Any time during the engagement — not gated — ask the Project Manager to "close out the week." It writes `outputs/status/status-YYYY-MM-DD.md`, updates the rollup log, and runs a workspace-maintenance pass to keep `notes/`, `outputs/`, and `outputs/diagrams/` from accumulating stale material.

## Skills

| Skill | Invoke with |
|-------|-------------|
| Requirements Gathering | `/requirements-gathering` |
| Architecture Decision Record | `/architecture-decision-record` |
| Decision Brief | `/decision-brief` |
| Security Matrix | `/security-matrix` |
| Fabric Workspace Matrix | `/fabric-workspace-matrix` |
| Workspace Maintenance | `/workspace-maintenance` |

The Cartographer, Skeptic, Practical Planner, and Project Manager don't have slash-invoked skills of their own (except workspace-maintenance, which the Project Manager also runs automatically) — they're full agents dispatched internally by the Lead, not something you call directly. `project-manager` is the one agent you can also address directly, outside the `Task` dispatch pattern, for close-outs and ad hoc estimates.

## Decision Brief structure

1. Problem Statement — plain business language, no jargon, a direct expansion of the Gate 1 statement
2. Recommended Architecture — plain language first, then the Cartographer's Conceptual diagram, then technical detail with Logical (and Physical / domain-specific, where warranted) diagrams
3. Alternatives Considered — honest, not dismissive
4. Risks and Open Questions — severity-rated, nothing hidden, includes any assumptions still live from Gate 1/2
5. Phasing and Sequencing — narrative, not a project plan
6. Explicit Ask — exactly what a "yes" starts

## Requirements

### Excalidraw (diagramming)

The Cartographer draws through the hosted Excalidraw API's MCP server, declared in this plugin's `.mcp.json`:

```json
{
  "mcpServers": {
    "excalidraw": {
      "type": "http",
      "url": "https://api.excalidraw.com/api/v1/mcp",
      "headers": {
        "Authorization": "Bearer ${EXCALIDRAW_API_KEY}"
      }
    }
  }
}
```

Get a personal API key from Excalidraw and set it as an environment variable — **never commit the real key**; `.mcp.json` only ever holds the `${EXCALIDRAW_API_KEY}` placeholder, which Claude Code expands from your shell environment at connect time:

```sh
export EXCALIDRAW_API_KEY="your-key-here"   # e.g. in ~/.zshrc, or your shell's env config
```

This hosted server supports importing icon libraries, including Azure service icons — the Cartographer uses them for Physical, Fabric workspace, and other diagrams depicting real Azure/Fabric services, once the product choice is actually settled (never for a component that's still undecided).

If the connector's actual tool names ever drift from what `cartographer.md` expects (`mcp__excalidraw__read_me`, `mcp__excalidraw__create_view`), the Cartographer is instructed to use whatever `mcp__excalidraw__*` tools are actually available and flag the mismatch — update the agent's `tools:` frontmatter to match once you see the real names.

### Word authoring

The Lead needs Word-document authoring tooling available to produce `decision-brief.docx`.

### Microsoft Fabric specialist skills

If this environment also has the `fabric-skills:*` plugin skills installed, `architecture-lead` and `project-manager` can consult a narrow, read-only subset of them (`fabric-skills:search-consumption-cli`, `fabric-skills:e2e-fabric-cost-estimation`, `fabric-skills:fabriciq`/`fabriciq-ontology-cli`) for tenant discovery and cost estimation — see "Microsoft Fabric Specialist Skills (Reference Only)" in `architecture-lead.md`. They never invoke a Fabric skill capable of creating, modifying, or deploying anything; that stays off-limits the same way all implementation work does.

### Graceful degradation

All of the above degrade gracefully — the Cartographer, the Lead's Word authoring, and the Fabric skill consultations all flag the gap explicitly when their tooling isn't available. None of them fail silently or skip a deliverable without saying so.
