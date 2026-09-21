# Mission workflow — detail

## 0. Bootstrap
Ensure `<workspace>/.regai/` exists (see SKILL.md). Idempotent — if it's already
there, skip silently.

## 1. Intake
- Read `.regai/context.md` and `.regai/memory/learnings.md` in full.
- Restate the goal in one sentence and confirm it.
- Ask ONLY for context that is missing AND material to the goal. Do not interrogate.
- Surface relevant past learnings (match on the `[tag]`) so the user sees what you're carrying forward.

## 2. Resolve playbook
- Look for a matching playbook in `.regai/playbooks/` first, then this skill's
  `playbooks/`. Same filename → personal wins.
- No match → use the **generic structure** below.

### Generic structure (no playbook)
1. Clarify the deliverable shape and acceptance criteria.
2. List the facts/sources to gather.
3. Identify which legal points need grounding via regai.
4. Produce the deliverable.
5. Self-check against the acceptance criteria.

## 3. Plan
Write `.regai/missions/<YYYY-MM-DD>-<slug>/plan.md` containing:
- **Goal** (one sentence)
- **Steps** (ordered, each independently checkable)
- **Sources to verify** (which số hiệu / Điều, which regai tool)
- **Deliverable** (format + where it lands)

Then STOP. Present the plan and wait for approval or edits. Do not execute before approval.

## 4. Execute with checkpoints
Work the approved steps. Grounding discipline (from regai-live-check):
- `check_in_force` before citing any document; cite `in_force_slug` if superseded.
- `get_article` before quoting any Điều — never quote a search-hit title.
- Cite as **số hiệu + Điều + tình trạng**.

Mandatory checkpoints — pause and show progress, invite correction:
- **After the research phase** (before drafting/analysis).
- **Before producing the final deliverable.**

## 5. Deliver
- Default: write Markdown into the mission folder.
- Word/PDF: only on request — hand off to the `docx` / `pdf` skill.
- Save supporting notes to `notes.md` in the mission folder.

## 6. Learn
Append durable, REUSABLE lessons to `.regai/memory/learnings.md`. One bullet per
lesson, newest at the bottom, dated, with a loose `[tag]`:

```
- 2026-06-21 [contract-review] Client X requires an unlimited-liability carve-out for IP indemnity.
- 2026-06-21 [securities] Offerings → verify NĐ 155/2020/NĐ-CP Điều 15 is in force.
```

Record only what generalizes beyond this one mission. Mission-specific facts stay in
the mission folder, not in learnings. If `learnings.md` ever grows unwieldy, split it
into per-tag files — not before.
