# Playbook: legal-memo

**Goal shape:** answer a legal question as a structured memo.

## Gather
- The precise question and the factual scenario.
- Relevant learnings by tag.

## Check (ground via regai)
- Run the regai-checking-current-law research loop: `deep_research` →
  `check_in_force` → `get_document` → `get_article` → `list_related`.
- Present the regulatory stack in tier order: Luật → Nghị định → Thông tư.

## Deliverable
- Markdown memo in the mission folder: Question → Short answer → Analysis by tier →
  Caveats. Every assertion cited.

## Citation rules
- số hiệu + Điều + tình trạng throughout; the Nghị định layer carries the primary
  actionable detail.
