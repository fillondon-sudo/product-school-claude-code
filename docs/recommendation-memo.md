# Results Memo: Weekly Summary (Engage v2) — For Marcus

## Situation
We shipped a prototype-validated personalized weekly summary screen (top spending insight, one ranked nudge, savings goal progress) and A/B tested it (`summary_v1` vs. control) against the hypothesis that the app going passive after week 1 is driving the 30-day retention drop (44%→37%). The test ran on cohort week 5 (n=50/arm), the first cohort with the feature live.

## Evidence
- **Day-7 retention lift:** `summary_v1` users retained at 76% on day 7 vs. 46% for control (+30pp), within the same signup cohort.
- **Day-30 retention lift:** `summary_v1` retained at 36% on day 30 vs. 22% for control (+14pp) — smaller than the day-7 lift because the feature fixes the week-1 engagement collapse specifically, not the flat ~31% day7→day30 stage that's unchanged across every cohort.
- **Sustained (not decaying) engagement:** `summary_v1` open rates climbed from 28% (week 1) to 56% (week 4), while control stayed flat at 4-6% — the opposite of the novelty-decay pattern we were worried about.

## Recommendation
Scale the weekly summary to all users, while treating the day7→day30 stage as a separate, unsolved problem that the current feature does not address.

## Ask
Sign-off to move from a single-cohort test (n=50/arm) to a larger, multi-cohort rollout — and a decision on whether that rollout is a full launch or a second, bigger A/B test, given the sample size caveat below.

## Risk if We Wait
Every additional cohort that goes without the feature continues to lose ~18pp of day1→day7 retention relative to what we've shown is recoverable, compounding the existing 44%→37% decline.

---

# Skeptical VP of Engineering: 3 Hardest Questions

**1. Is n=50 per variant actually enough to trust this, or are we reacting to noise?**
With 50 users per arm, a 14-30pp swing can look dramatic but still fall within a wide confidence interval — we haven't run a significance test (e.g., a proportions z-test) on either the day-7 or day-30 gap, and we only have one cohort's worth of treatment data. Before calling this "proven," I'd want the actual p-value or confidence interval, and ideally a second cohort showing the same direction before committing engineering resources to a full rollout.

**2. You're attributing the lift to the feature, but cohort 5 also differs from cohort 4 in other ways — how do you know it's not a seasonality, marketing-mix, or selection effect?**
Cohort 5 also showed a different acquisition channel mix and the treatment/control split itself isn't randomized across the *whole* user base — it's one week's signups split into two groups. If anything else changed that week (a marketing push, a different signup flow, an app update), it could be confounded with the variant assignment. What's the actual randomization mechanism, and can you rule out a concurrent change that week?

**3. The data shows day7→day30 retention is flat regardless of the feature — so what exactly are we scaling, and what's the actual expected impact on the metric Marcus cares about (30-day retention), at full scale, after the day-7 boost decays into the unchanged day-30 conditional?**
A +14pp day-30 lift in a 50-user cohort sounds good, but if the underlying mechanism is "more people reach day 7, then churn at the same conditional rate as everyone else," the long-run effect at scale could regress toward something smaller than 14pp once cohort-level noise washes out. What's the projected steady-state 30-day retention number you're actually committing to, and how confident are you in that projection versus the cohort-5 snapshot?
