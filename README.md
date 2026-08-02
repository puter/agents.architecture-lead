# Architecture Lead — Claude Code Subagent Plugin

A principal-level architecture consultant persona for Claude Code. Takes a client engagement from raw discovery notes through to a client-approved decision brief, without touching implementation.

## What it does

- Ingests raw, unformatted post-session notes and structures them into findings
- Asks clarifying questions for gaps rather than assuming answers
- Presents multiple architecture options with trade-offs — never a single answer
- Produces a six-section client-facing decision brief
- Stops at three hard gates and refuses to proceed without user confirmation
- Hands off to implementation subagents only after client approval — never builds anything itself

## Three Gates

| Gate | Trigger | What the agent does |
|------|---------|---------------------|
| Gate 1 | Discovery notes ingested | Structures findings, asks clarifying questions, waits for scope confirmation |
| Gate 2 | Options reviewed | Presents 2–4 options, engages in discussion, waits for chosen option confirmation |
| Gate 3 | Brief delivered | Packages the decision brief, refuses execution-ready status until client approval confirmed |

## Files

```
.claude/
  agents/
    architecture-lead.md        # Subagent system prompt + behavioral rules
  skills/
    requirements-gathering.md   # Discovery checklist + findings format
    architecture-decision-record.md  # ADR format for internal design choices
    decision-brief.md           # Six-section client brief authoring guide
templates/
  discovery-findings.md         # Blank findings template
  adr-template.md               # Blank ADR template
  decision-brief-template.md    # Blank decision brief template
tests/
  scenario-acme-retail.md       # Fictional engagement test: gate stop verification
plugin.json                     # Plugin manifest for marketplace publishing
```

## User workflow

1. After a client session, paste raw notes (no reformatting needed).
2. The agent structures them and asks clarifying questions. Confirm scope at Gate 1.
3. Ask the agent for architecture options. Review and discuss. Confirm your choice at Gate 2.
4. Ask the agent to produce the decision brief. Review it. Gate 3: take it to the client.
5. When the client approves, tell the agent. The engagement is now execution-ready.

## Skills

| Skill | Invoke with |
|-------|-------------|
| Requirements Gathering | `/requirements-gathering` |
| Architecture Decision Record | `/architecture-decision-record` |
| Decision Brief | `/decision-brief` |

## Decision Brief structure

1. Problem Statement — plain business language, no jargon
2. Recommended Architecture — plain language first, then technical detail
3. Alternatives Considered — honest, not dismissive
4. Risks and Open Questions — severity-rated, nothing hidden
5. Phasing and Sequencing — narrative, not a project plan
6. Explicit Ask — exactly what a "yes" starts
