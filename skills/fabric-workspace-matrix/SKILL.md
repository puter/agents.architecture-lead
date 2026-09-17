---
name: fabric-workspace-matrix
description: >
  This skill should be used whenever an engagement involves Microsoft Fabric
  and workspace topology is part of what's being decided or communicated —
  the same trigger condition as the Cartographer's Fabric workspace diagram.
  Invoke it when the user says "workspace matrix", "map our Fabric
  workspaces", or "/fabric-workspace-matrix". It produces the tabular detail
  (capacity, items, RBAC, Git integration, deployment stage) that sits
  alongside the Cartographer's Fabric workspace diagram, and stores it at
  outputs/fabric-workspace-matrix.md.
metadata:
  version: "0.1.0"
---

## When to invoke

Invoke this skill when:
- The engagement involves Microsoft Fabric and workspace layout is part of what's being decided (new workspace-per-domain pattern, capacity sizing, environment promotion strategy) — not only when the user asks for it by name.
- The Cartographer is drawing or has drawn a Fabric workspace diagram — this matrix is the detail behind that picture, the same relationship the security matrix has to the Security Structure diagram.
- The user says "workspace matrix", "map our Fabric workspaces", "show me the capacity plan", or `/fabric-workspace-matrix`.
- Discovery surfaces an existing (brownfield) Fabric estate that needs to be inventoried before new architecture can be proposed against it.

---

## Before drafting

1. Confirm the workspace topology decided or proposed so far (chosen Gate 2 option, any ADR locking in a workspace pattern).
2. If this is a brownfield engagement — an existing Fabric tenant already has workspaces — consider consulting `fabric-skills:search-consumption-cli` to discover what actually exists in the tenant rather than relying only on what the client described in notes. This is read-only catalog discovery; it does not create or modify anything. See "Microsoft Fabric Specialist Skills (Reference Only)" in `architecture-lead`'s own instructions for the boundary on when Fabric skills may be consulted.
3. Cross-reference `outputs/security-matrix.md` if it exists for the identity/access model — this matrix should point at it for RBAC detail rather than re-deriving it.

---

## Matrix Format

See `templates/fabric-workspace-matrix-template.md` for the blank copy. It covers: workspace-by-workspace detail (domain, purpose, capacity SKU, environment, key items, access, Git integration, deployment stage), capacity notes, and cross-workspace dependencies.

---

## After drafting

1. Write to `outputs/fabric-workspace-matrix.md` (creating `outputs/` with `mkdir -p` if needed). Re-write on every revision.
2. Make sure the workspace names and boundaries here match the Cartographer's Fabric workspace diagram exactly — a mismatch is a Skeptic-worthy gap, not a cosmetic detail.
3. If the Project Manager's budget estimate includes Fabric capacity cost, make sure the capacity SKUs named here are the same ones that estimate is built against.

---

## Template file

See `templates/fabric-workspace-matrix-template.md` for a blank copy.
