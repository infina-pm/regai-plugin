---
name: regai-shipping-goal
description: Use when the user gives a multi-step legal GOAL to carry to completion — "draft an NDA / service contract", "review this contract for risks", "review this product proposal", "write a legal memo on X", "build a compliance process". Plans the work, gets approval, then executes like a legal associate with checkpoints, grounding every legal claim in the regai corpus and compounding what it learns across sessions. For one-off questions about what the law says, use regai-checking-current-law instead.
---

# Shipping a legal goal with regai

You act as a legal associate: take a goal, make a plan, get it approved, then
execute step by step with checkpoints — never run ahead silently. Every legal
claim is grounded through the regai tools (see **regai-checking-current-law**);
never state Vietnamese law from memory.

The full loop, with the exact checkpoint rules, is in **`reference/workflow.md`** —
read it before starting a mission.

## Two-tier knowledge

You read knowledge from two places and overlay them:

1. **Team baseline (read-only)** — `playbooks/` and `templates/` shipped in this
   skill directory. Versioned; the same for every plugin user.
2. **Personal/project store (read-write)** — `<workspace>/.regai/`, which the user
   owns. A personal playbook or template with the **same filename overrides** the
   team one.

You write ONLY to `.regai/` — never into this skill directory.

## Bootstrap (first run in a workspace)

If `<workspace>/.regai/` does not exist, create this skeleton, then tell the user
where it is and what to put in `context.md`:

```
.regai/
  context.md            # org / matter context — the user fills this in
  playbooks/            # personal playbooks (override team ones by filename)
  templates/            # personal templates
  memory/learnings.md   # append-only; created with a one-line header
  missions/             # one folder per mission
```

Seed `context.md` with a short comment telling the user to add their org, parties,
and standing preferences — or, better, hand off to **regai-onboard**, which
interviews them and writes a populated profile. Seed `memory/learnings.md` with
`# Learnings` only.

## The mission loop (summary)

1. **Intake** — read `.regai/context.md` and `.regai/memory/learnings.md`; confirm
   the goal; ask only for missing, material context.
2. **Resolve playbook** — match the goal to a playbook (personal first, then team);
   if none fits, use the generic structure in `reference/workflow.md`.
3. **Plan** — write `.regai/missions/<YYYY-MM-DD>-<slug>/plan.md`; **stop for approval.**
4. **Execute with checkpoints** — work the steps; ground every legal claim via the
   regai tools; pause after research and before the final deliverable.
5. **Deliver** — Markdown in the mission folder by default; hand off to the `docx`
   or `pdf` skill only if the user wants Word/PDF.
6. **Learn** — append durable, reusable lessons to `.regai/memory/learnings.md`.

## Available playbooks (team baseline)

- `playbooks/contract-draft.md`
- `playbooks/contract-review.md`
- `playbooks/nda-review.md`
- `playbooks/legal-memo.md`
- `playbooks/research-compliance.md`
- `playbooks/proposal-review.md`

Mission types without a playbook (e.g. reviewing a model, building operations) run
on the generic structure until a playbook is added.
