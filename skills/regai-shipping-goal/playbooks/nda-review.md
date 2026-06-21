# Playbook: nda-review

**Goal shape:** fast triage of an NDA — verdict + the clauses to fix.

## Gather
- The NDA text and which side the user is on.
- Standing preferences from `.regai/context.md` and `[nda*]` / `[contract*]` learnings.

## Check (against the standard NDA checklist)
- Mutuality (one-way vs mutual — matches the user's position?).
- Definition & scope of Confidential Information (over-broad? missing carve-outs?).
- Term and survival period (reasonable? open-ended?).
- Permitted disclosures / exclusions (public, independently developed, legally compelled).
- Return/destruction, governing law & forum, assignment, remedies.
- Ground any statutory point (e.g. mandatory governing-law constraints) via the regai
  tools; `check_in_force` before relying on a document.

## Deliverable
- Short triage in the mission folder with an overall verdict:
  - **GREEN** — acceptable as-is.
  - **YELLOW** — sign after the listed fixes.
  - **RED** — do not sign; material problems.
  - Then a bullet list of specific clauses to change and why.

## Citation rules
- Any legal basis cited as số hiệu + Điều + tình trạng. No memory-based law.
