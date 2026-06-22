---
name: weekly-status
description: Turn raw bullet-point notes into a formatted leadership status update with Shipped, In Progress, Blockers, and Next Week sections. Use when asked to write, draft, or format a weekly status update from notes.
---

# Weekly Status Update

Convert raw, informal bullet notes into a leadership-ready weekly status update.

## Input
Raw bullet-point notes — unordered, informal, may mix topics (shipped work, in-progress work, blockers, plans).

## Output format
Produce exactly four sections, in this order:

```
## Shipped
- ...

## In Progress
- ...

## Blockers
- ...

## Next Week
- ...
```

## Rules
- Maximum 3 bullets per section. If there are more than 3 relevant items for a section, keep the 3 most significant and drop the rest — do not add a 4th bullet or a "more" note.
- If a section has no items, write "None" under that heading rather than omitting the heading.
- Use plain, declarative language. No jargon, no buzzwords, no hedging ("potentially," "looking into," "synergy," etc.).
- Each bullet is one short sentence stating what happened or what's planned — not a question, not a fragment.
- Do not invent details that aren't in the input notes. If a note is ambiguous, place it in the section it most plausibly belongs to rather than guessing additional specifics.
- Preserve names, numbers, and dates from the input exactly as given.
