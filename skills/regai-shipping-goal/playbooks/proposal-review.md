# Playbook: proposal-review

**Goal shape:** Product sends a feature / business-model proposal; produce a legal
verdict and a path to ship it.

## Gather
- The feature, user type (cá nhân / tổ chức), and the money & data flows. Ask if
  any of these are unclear before analysing.
- `.regai/context.md` for org posture and red lines (e.g. the BCC 3-condition check).
- Any uploaded precedent files — index and select per `reference/precedent-files.md`.
- Relevant `[proposal-review]` learnings.

## Check (ground via regai)
1. **Identify issues** — list the legal issues as a table: vấn đề | lĩnh vực | ưu tiên
   (🔴/🟡/🟢). Derive the core questions that decide feasibility.
2. **Ground each issue** — `search` → `check_in_force` → `get_article`; escalate to
   `deep_research` only for a broad, multi-tier issue. Never state law from memory;
   flag any issue the corpus doesn't cover rather than guessing.
3. **Market & precedent** — web search for how comparable VN/regional fintechs handled
   it, plus the selected case-library precedents. Cite comparables as market practice,
   not legal authority.

## Deliverable
- Markdown legal report in the mission folder using `templates/legal-memo-formal.md`:
  exec summary, per-issue analysis, risk matrix, ranked options (khuyến nghị first),
  then a compliance checklist + SOP for Product (pre-dev / dev / pre-launch / ongoing).
- Templates or contracts only if the user confirms they want them (see other playbooks).
- Hand off to the `docx` skill for the Infina-headered Word file only on request.

## Citation rules
- số hiệu + Điều + tình trạng on every legal claim. Contested points get a risk level
  and a mitigation, never a flat conclusion. Append the internal-use disclaimer from
  `.regai/context.md`.
