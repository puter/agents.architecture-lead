---
name: security-matrix
description: >
  This skill should be used when the engagement's architecture includes a
  security architecture view — the Cartographer has drawn or is about to draw
  a Security Structure diagram, the reference/ folder contains compliance or
  security standards that must be mapped to controls, or the user says
  "security matrix", "build the security matrix", "map our controls", or
  "/security-matrix". It produces the asset/threat/control mapping that
  backs up the Security Structure diagram with specifics a security
  reviewer can act on, and stores it at outputs/security-matrix.md.
metadata:
  version: "0.1.0"
---

## When to invoke

Invoke this skill when:
- The Cartographer has produced (or is about to produce) a Security Structure diagram — the matrix is the detail behind that picture.
- `reference/` contains compliance standards, security policy, or an approved-control list that needs to be mapped against the proposed architecture.
- The user asks to "build the security matrix", "map our controls", "show me the security posture", or `/security-matrix`.
- A Gate 3 decision brief covers an architecture with meaningful security surface (handles regulated data, crosses trust boundaries, introduces new identity/access patterns).

This is an internal-audience artifact by default — it names specific controls, owners, and residual risk in a level of detail a client document usually shouldn't carry. Adapt or summarize it for client consumption; don't hand the raw matrix over as-is unless asked.

---

## Before drafting

1. Read `reference/` for any binding compliance or security standards — these define required columns (e.g. a HIPAA engagement needs a compliance mapping column that names HIPAA safeguards specifically, not a generic "compliance" checkbox).
2. Confirm the trust boundaries with whatever's been decided so far (discovery findings, the chosen Gate 2 option, any ADRs already logged for security-relevant structural choices).
3. If the engagement is on Microsoft Fabric, cross-reference `outputs/fabric-workspace-matrix.md` if it exists — workspace-level RBAC belongs there; don't duplicate it here, reference it.

---

## Matrix Format

See `templates/security-matrix-template.md` for the blank copy. It covers: trust boundaries, a control matrix (asset, data classification, threat, control, owner, residual risk, compliance mapping), and an identity/access table.

Tailor the columns to the actual platform and compliance regime — the template is a generic baseline, not a fixed schema. State explicitly what you added or renamed and why, so the next person working the file understands it was adapted deliberately.

---

## After drafting

1. Write to `outputs/security-matrix.md` (creating `outputs/` with `mkdir -p` if needed). Re-write on every revision.
2. If a Security Structure diagram exists or is being produced, note where it lives and make sure the trust boundaries named in both documents actually match — a mismatch here is a Skeptic-worthy gap.
3. Flag any residual risk rated High in the same place you'd flag a blocking gap at a gate — don't let a High residual risk sit quietly in a table nobody's asked about.

---

## Template file

See `templates/security-matrix-template.md` for a blank copy.
