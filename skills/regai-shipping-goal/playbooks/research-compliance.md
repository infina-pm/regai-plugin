# Playbook: research-compliance

**Goal shape:** given a topic/activity, map the compliance obligations.

## Gather
- The activity/topic and the actor (who must comply), plus any jurisdiction nuance.
- Relevant `[compliance]` / topic-tagged learnings.

## Check (ground via regai)
- `deep_research` the topic to surface the regulatory stack; for each relevant
  document `check_in_force`, then `get_article` the obligation-bearing Điều.
- `list_related` to pull implementing Nghị định / Thông tư that carry the operational
  requirements, deadlines, and thresholds.

## Deliverable
- An obligations checklist in the mission folder: each row = obligation, who it binds,
  threshold/deadline, and the source (số hiệu + Điều + tình trạng). Note gaps where
  the corpus has no in-force coverage (say so plainly — do not guess).

## Citation rules
- Every obligation cites its in-force source. If a topic isn't covered, report it as
  outside the current corpus rather than answering from memory.
