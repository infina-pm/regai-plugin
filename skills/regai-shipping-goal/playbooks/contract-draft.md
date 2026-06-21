# Playbook: contract-draft

**Goal shape:** produce a redline-ready draft contract.

## Gather
- Parties (full legal names, roles), governing law, purpose.
- Key commercial terms (price, term, termination, liability, IP, confidentiality).
- Any template to start from (`templates/`, personal first); else draft from scratch.
- Standing preferences from `.regai/context.md` and `[contract*]` learnings.

## Check (ground via regai)
- For each obligation with a statutory basis, verify the controlling Luật/Nghị
  định/Thông tư with the regai tools; `check_in_force` before relying on it.
- Confirm mandatory clauses for the contract type are present.

## Deliverable
- Markdown draft in the mission folder, clauses numbered, with a short cover note
  listing assumptions and any clause grounded in a specific số hiệu + Điều.

## Citation rules
- Cite số hiệu + Điều + tình trạng for every legal point. Never cite an expired doc.
