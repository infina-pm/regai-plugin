---
name: regai-onboard
description: Use the first time a user works on a legal goal in a workspace, when `.regai/context.md` is missing or still empty (only the seed comment), or when the user says "set up regai for my company", "regai doesn't know my org", "onboard me", "tell regai about us", or runs /regai-onboard. Interviews the user briefly and writes their org/house-style profile so every later mission is grounded in their context instead of guessing.
---

# Onboarding a regai workspace

Goal: turn an empty `.regai/context.md` into a short, useful profile of the
user's organization and house style. Every `regai-shipping-goal` mission reads
this at intake, so a populated profile is what stops you from guessing at the
user's sector, risk posture, and drafting conventions on every run.

**This is the only skill that interviews to *populate* the profile.** Other
skills read `.regai/context.md`; they do not fill it in.

## When NOT to run

If `.regai/context.md` already has real content (more than the seed comment),
don't re-interview. Offer `/regai-onboard --redo` only if the user explicitly
wants to start over, or point them at editing the file directly for small
changes — it's plain Markdown.

## Steps

1. **Bootstrap the workspace.** If `<workspace>/.regai/` doesn't exist, create
   the skeleton (same as `regai-shipping-goal`):
   ```
   .regai/
     context.md
     playbooks/
     templates/
     memory/learnings.md   # seed with "# Learnings"
     missions/
   ```
   Idempotent — if a path exists, leave it.

2. **Interview — briefly.** Ask the questions below, grouped into one or two
   batches. Do not interrogate: accept "skip" / "not sure" for anything, and
   fill those slots with `[TBD]` rather than pressing. The whole thing should
   feel like two messages, not a form.

   - **Organization** — name, what the company does, sector/industry.
   - **Your role** — in-house counsel, business/ops, founder, external advisor?
     (Shapes how much legal framing to include in deliverables.)
   - **Typical matters** — what recurs? (contracts, NDAs, compliance, securities
     filings, memos…) — this is what tells you which playbooks will matter.
   - **Counterparties** — who you usually deal with (vendors, customers,
     investors, regulators).
   - **Risk posture** — conservative / balanced / aggressive.
   - **House style** — working language (Vietnamese / English / bilingual),
     default governing law, default dispute-resolution forum, and any tone or
     formatting conventions.
   - **Standing preferences / red lines** — clauses you always require or refuse.

3. **Write `context.md`.** Use the template below. Put real answers in; leave
   `[TBD]` for anything skipped. Keep it prose, not a schema — it's read in full
   by humans and by the mission loop.

4. **Confirm and hand off.** Show the user where the file is, tell them they can
   edit it any time, and that durable lessons accumulate separately in
   `.regai/memory/learnings.md` as they run missions. If they came in with an
   actual goal, hand straight to **regai-shipping-goal**.

## context.md template

```markdown
# regai context

_Maintained by /regai-onboard. Edit freely — every mission reads this at intake._

## Organization
- **Name:** …
- **Sector / what we do:** …
- **Your role:** …

## Typical matters
- …

## Counterparties
- …

## Risk posture
[conservative / balanced / aggressive] — [one line on what that means here, e.g. "standard market terms, escalate anything unusual"]

## House style
- **Working language:** Vietnamese / English / bilingual
- **Default governing law:** Vietnamese law
- **Default dispute resolution:** [VIAC arbitration / People's Court of …]
- **Tone & formatting:** …

## Standing preferences / red lines
- …
```

Anything the user skipped stays as `[TBD]` — a later mission can ask when it
actually needs that slot, exactly as `regai-shipping-goal` intake does.
