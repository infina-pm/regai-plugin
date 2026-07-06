# Playbook: contract-review

**Goal shape:** review a contract and produce a risk/recommendation memo.

## Gather
- The contract text, the user's role (which side), and risk appetite from
  `.regai/context.md`.
- Relevant `[contract-review]` learnings.

## Check (ground via regai)
- Classify each material clause; flag deviations from the playbook / standing
  preferences with a severity (high / medium / low).
- For each legal concern, ground it: `check_in_force` the controlling document,
  `get_article` the Điều before quoting.

## Questions to answer for easy review
First confirm **which side the user is on** — their company vs. the counterparty
(the partner) — and use that throughout. Below, "you" = the user's company.
Answer each up front; best guess where the contract is silent, marked as such.

- **Type of contract** (distribution, BCC, …) + list the heading of every Appendix (phụ lục).
- **Duration** — what happens if you breach? if the partner breaches?
  Auto-renewal, or conditions for renewal?
- **Commercial terms** (anything involving money) — summarize; use a table if clearer.
- **Vs. prior** — if you've signed a similar contract before, what's different this time?
- **Deal breakers for the partner** — terms the partner won't sign without.
- **Deal breakers for you** — terms your company won't sign without.
- **Reconciliation & payment** — if money moves both ways, frequency of reconciliation
  and payment terms.
- **Restrictions on you** — exclusivity, right of first refusal, etc.

## Deliverable
- Markdown review memo in the mission folder: the answers above, then per-clause
  findings, severity, recommended edit, and the legal basis (số hiệu + Điều + tình trạng).

## Citation rules
- Every flagged legal risk names its source document and Điều. No memory-based law.
