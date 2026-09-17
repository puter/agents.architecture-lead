---
name: workspace-maintenance
description: >
  This skill should be used periodically as an engagement matures — most
  naturally as part of the Project Manager's weekly close-out, but also
  standalone when the user says "clean up the workspace", "archive old
  notes", "tidy up the project", or "/workspace-maintenance". It scans
  notes/, reference/, and outputs/ for material that's gone stale — raw
  notes fully subsumed by discovery findings, draft diagrams or documents
  marked superseded but never moved, conceptual patterns abandoned at
  Gate 2 — and proposes moving it to archive/. It never deletes anything
  and never moves anything without the user confirming the list first.
metadata:
  version: "0.1.0"
---

## When to invoke

Invoke this skill when:
- The Project Manager is closing out a week — run this as the last step, after the status report, so the workspace stays tidy on the same cadence the client sees progress.
- The user says "clean up the workspace", "archive old notes", "tidy up the project", "trash collection", or `/workspace-maintenance`.
- An engagement has crossed a gate (especially Gate 2, where discarded options and draft diagrams tend to accumulate) and nobody has done a pass since.

This is a housekeeping skill, not a document-producing one. It doesn't ask deep judgment calls — it looks for concrete staleness signals and lets the user make the call on anything ambiguous.

---

## What Counts as Stale

Check each project folder for these specific signals — don't guess at staleness, look for evidence:

- **`notes/`** — a raw notes file whose entire content is now reflected in `outputs/discovery-findings.md` and isn't referenced by any open question, tracked assumption, or later engagement material. Fully subsumed, nothing left hanging off it.
- **`reference/`** — a standing document explicitly superseded by a newer one (the client sent an updated policy, a corrected standard) where both versions are still sitting side by side.
- **`outputs/`** — an ADR whose Status field already reads "Superseded by ADR-NNN" but the file is still being read as if current; a draft decision-brief revision that's been fully replaced by a later `.md`/`.docx` pair (only relevant if revisions were saved as separate files rather than overwritten in place — check the Lead's convention of overwriting first).
- **`outputs/diagrams/`** — any diagram `master-map.md` already marks "Superseded" where the superseded file itself is still sitting in the active diagrams folder rather than flagged or moved.
- **Abandoned Gate 2 options** — working notes or draft diagrams for an architecture option that was seriously explored and then explicitly set aside (should already be captured in Section 3 of the decision brief, but the scratch material behind it often lingers).

Do not flag anything as stale on a guess. If something looks old but you can't point to the specific evidence above, leave it — ask the user instead of proposing a move you can't justify.

---

## What You Do

1. Scan the folders above and build a candidate list: file path, why it's flagged (one line, citing the specific signal), and where it would move to (`archive/`, preserving relative structure).
2. Present the candidate list to the user before moving anything. Group by folder. Nothing moves without explicit confirmation — this mirrors the same non-destructive posture `architecture-lead` already uses for superseded material.
3. On confirmation, move (not copy, not delete) each file into `archive/` with `mkdir -p` as needed, preserving enough path structure that where it came from is still obvious.
4. If a move reflects a real decision (an option was dropped, a document was formally superseded) rather than pure tidying, and no ADR already captures that decision, flag it — don't write the ADR yourself, tell the user or `architecture-lead` one is owed.
5. Report what moved and what was left in place (and why), so the user has a clean record of the pass.

---

## What You Never Do

- You never delete anything. `archive/` is the only destination — this is about keeping the working folders navigable, not about disposal.
- You never move a file without it being confirmed by the user first, even when the staleness signal seems obvious.
- You never touch `archive/` itself — once something's there, this skill doesn't manage it further.
- You never flag something as stale without a concrete signal from the list above — "this looks old" is not a reason.
