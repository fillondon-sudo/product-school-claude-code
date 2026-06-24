# Learning Synthesis & Hypothesis: Engage v2

**Sources:** `docs/decision-brief.md` (research synthesis: interviews, NPS, competitive scan), `docs/iteration-log.md` (usability session 1 on the weekly summary prototype)

## What We Know
- 30-day retention dropped from 44% to 37% over the last two quarters, and the drop-off happens *after* the initial spending-breakdown "aha moment," not during onboarding.
- The static/repetitive experience is the top-ranked theme across independent sources (interviews and NPS) — users explicitly say "nothing has changed since the first day."
- Personalization breaks down quickly in the current product: a stated savings goal was never referenced again, and at least one user found nudges random enough to disable notifications entirely.
- In a usability test of the weekly summary prototype, the core value proposition was legible without explanation, and it produced a real stated behavior-change intent ("it made me realise I spent too much on subscriptions").
- A duplicated, ambiguous "Set a savings goal" CTA was a genuine usability defect that actively confused the one participant tested — this was a logic problem, not a one-off reaction, and has been fixed in the prototype.
- No competitor (Monarch Money, Cleo, Rocket Money) proactively delivers one ranked, personalized weekly insight, and none solve the "notification leads to continued in-app story" problem — this is open white space, not a catch-up play.

## What We Assume
- That the personalized weekly summary screen (top insight + one actionable nudge + goal progress) is the right mechanism to address the static-experience problem — this is the team's working hypothesis, not yet confirmed at scale.
- That the goal-visualization request from usability session 1 ("show me what I could do with the money — a car, a bike, a holiday") would meaningfully increase motivation, rather than being a one-off preference.
- That fixing the duplicate-CTA defect removes a real blocker to comprehension for users broadly, based on usability heuristics — not yet confirmed with more than one participant.
- That the ~3-month time-to-value reported by our power user is broadly representative of how long personalization takes to "click," and that this is the main driver of early churn (e.g., the 5-week churned user) — plausible given the data, but inferred from a single account, not measured directly.
- That solving in-app static-content and lack-of-guidance issues is sufficient to move 30-day retention, independent of the email/notification delivery layer (push notifications were requested by the usability participant but are a distribution decision outside the prototype's current scope).

## What We Still Don't Know
- Whether the duplicate-CTA confusion and the goal-visibility issue recur across a broader sample — usability session 1 had n=1.
- Whether the nudge-scoring approach (multi-signal ranking) actually produces nudges users find relevant at scale, versus the specific cases tested so far.
- Whether users understand or trust the "Why this nudge?" transparency panel, or whether it should be simplified, relabeled, or removed for a consumer audience.
- What the actual lift on 30-day retention would be if this feature shipped — no quantitative test (e.g., A/B) has been run yet; everything to date is qualitative/directional.
- Whether goal-visualization and subscription-cancellation checkboxes (both flagged as larger builds) are worth the investment relative to simpler fixes, since we only have one data point requesting them.

## Hypothesis Statement
We believe that **the personalized weekly summary screen (top spending insight, one ranked actionable nudge, and visible savings goal progress)** will deliver **renewed week-over-week relevance that gives passive users a reason to return and act** for Nudge users in their first 30 days, as measured by **30-day retention rate**.
