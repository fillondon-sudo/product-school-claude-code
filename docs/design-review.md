# Design Review: Weekly Summary Prototype

## 1. User Needs Addressed Well
- **Surfacing a concrete spending insight users didn't have visibility into.** Amara: "The spending breakdown was shocking in a useful way — I had no idea I was spending that much on subscriptions." The prototype's top-insight card (currently surfacing the subscription spike) hits exactly this need.
- **Driving stated behavior-change intent, not just awareness.** Usability session 1 participant: "Would you change your spending based on this? Absolutely yes. It made me realise I spent too much on subscriptions." This is direct evidence the nudge mechanism (not just the insight) is doing its job.
- **Closing the "what should I do next" gap.** Tom: "I kept waiting for it to give me something to act on and it never really did." Priya: "It took about three months to get there." The single ranked, actionable nudge is designed to give a next step immediately rather than after months of passive use — addressing the exact complaint Tom and Priya raised about the current product.
- **A single, unambiguous CTA for goal-setting.** Usability session 1 originally found a duplicate "Set a savings goal" CTA that confused the participant; this has been fixed (one nudge candidate removed) and re-verified — there's only one working CTA now.

## 2. User Needs Not Yet Addressed
- **Goal visibility/motivation.** Usability participant: goal target ($500) wasn't "immediately visible, only after a while," and the wand-ask was for a visualization of "what I could do with the money I saved (a car, a bike, a holiday)." Nothing in the current prototype visualizes the goal's real-world payoff — it's a static progress bar.
- **Static dashboard feel beyond week 1.** Tom: "The app just showed me the same dashboard every time." Amara: "Nothing has changed since the first day." The prototype currently only computes one insight + one nudge per week; it doesn't yet address whether the *experience itself* (layout, framing) varies meaningfully week over week, only the underlying data.
- **Action-density compared to competitors.** Tom: "I switched to YNAB because at least that feels like it is asking something of me." The subscription-audit nudge tells the user to act, but doesn't yet let them act in-product (no subscription multi-select/cancel flow) — the gap Tom is describing.
- **Transparency panel comprehension.** Usability participant: "scoring details sounds cool and I like it but I don't understand." The "Why this nudge?" panel is currently aimed at a technical/internal audience, not validated for consumer use.

## 3. Highest-Impact Change for Week-1 Retention
**Make the savings goal target and progress visually prominent and tied to a real-world payoff, immediately on screen load.**
This is the single highest-leverage fix because it's the only friction point with *both* explicit usability evidence ("not immediately visible") *and* a direct line to the retention mechanism CLAUDE.md names explicitly: "churn is nearly double for users who don't set a savings goal in week 1." If the goal isn't visible or motivating, users have no reason to set or keep one — which is the single signal most tightly correlated with churn we have. The dining/subscription insight changes week to week regardless, but goal visibility is a fixable, structural gap with a known retention link.

## 4. Lena's Ownership vs. Product Decision
**Lena (Design) owns:**
- Visual prominence/placement of the goal progress card (making the $500 target legible without scrolling or delay)
- Visualization treatment for "what this becomes" (car/bike/holiday framing) — once scope is approved
- Simplifying or relabeling the "Why this nudge?" panel for a consumer audience

**Requires a product decision (not Lena's call alone):**
- Whether to build subscription multi-select/cancel-in-app (flagged in `docs/spec-readiness.md` as a larger build, currently deferred from Sprint 1) — this is a scope/investment tradeoff, not a visual design problem
- Whether goal-visualization is validated enough to commit engineering time to, given it's currently n=1 evidence (per `docs/hypothesis.md`, "What We Assume")
- Whether to expand beyond one nudge/one insight per week (multi-category ranking, top merchants) — this changes the nudge-scoring engine's scope, owned jointly with Raj

## Sources
- `research/interview-synthesis.md` (Priya, Tom, Amara quotes)
- `docs/iteration-log.md` and `docs/iteration1-feedback.md` (usability session 1 findings)
- `prototype/index.html` (current prototype behavior)
- `docs/hypothesis.md`, `docs/spec-readiness.md` (validation status and scope framing)
- `CLAUDE.md` (week-1 goal-setting churn correlation)
