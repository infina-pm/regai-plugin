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

## Deliverable
- Markdown review memo in the mission folder: per-clause findings, severity,
  recommended edit, and the legal basis (số hiệu + Điều + tình trạng).

## Citation rules
- Every flagged legal risk names its source document and Điều. No memory-based law.
