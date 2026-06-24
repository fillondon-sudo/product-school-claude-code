# Usability Session 1 — Structured & Ranked Feedback

Ranked by severity × frequency-likelihood (single participant — "frequency-likelihood" reflects how probable an issue is to recur based on usability heuristics, not confirmed volume across multiple users).

| Rank | Item | Type | Severity | Status |
|---|---|---|---|---|
| 1 | Duplicate/ambiguous "Set a savings goal" CTA — two buttons appeared to do the same thing, only one worked | Bug / friction | High — blocks core action | Already fixed |
| 2 | Goal target ($500) not prominent — noticed late, not salient | Friction | Medium — undercuts motivation, doesn't block use | Not yet addressed |
| 3 | Goal visualization — "dynamic, moving... what I could do with the money (car, bike, holiday)," materializing as savings grow | Suggestion (new feature) | Medium-High impact, but high build effort | Not yet addressed |
| 4 | Subscription checkboxes — multi-select to cancel in one click, savings pot updates live per selection | Suggestion (new feature) | High impact, high effort | Not yet addressed |
| 5 | "Why this nudge?" scoring panel is confusing — liked the idea, didn't understand it | Friction | Low-Medium — not core path, but undermines trust in the nudge | Not yet addressed |
| 6 | Personalized spending-pattern analysis section, concise | Suggestion (new feature) | Medium impact, medium effort | Not yet addressed |
| 7 | Show other categories beyond subscriptions, ranked by size, highlight top 3–10 merchants | Suggestion (enhancement) | Medium impact, low-medium effort | Not yet addressed |
| 8 | Delivery via push notification rather than relying on in-app discovery | Positive signal / suggestion | High strategic relevance (matches existing email-engagement research), but it's a distribution decision, not a prototype change | Not yet addressed — flag for product, not prototype |

## Working Well (no action needed, but worth protecting in future iterations)
- Core value prop is legible unprompted ("review spendings... achieve my saving goal")
- Drove a real stated behavior-change intent (subscription spend)

## Sequencing Notes
- **#2 and #3 are related** — could be tackled together as "make the goal feel real and visible" rather than as two separate efforts.
- **#5 is cheap to fix** (simplify or hide the panel) relative to its impact on trust — possibly worth doing even though it's not top-ranked by severity.
- **#4 is the biggest lift** here and probably deserves its own design pass before being built into this prototype.
- **#8 isn't a prototype change at all** — it's a distribution/notification strategy question for the broader Engage v2 plan, separate from what we iterate on in `index.html`.

## Source
Full session detail (questions asked, verbatim participant responses, what's working, change made) is in `docs/iteration-log.md`.
