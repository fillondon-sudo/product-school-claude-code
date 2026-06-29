# Metric Diagnosis: 30-Day Retention Decline & Weekly Summary Impact

**Builds on:** `data/metric-findings.md`. All numbers below are from the same dataset (`P5L1_Nudge_Dataset.xlsx`), with additional queries run to decompose retention into stages and to compare churned vs. retained users within the week-5 treatment group.

## 1. Metric Tree: Decomposing 30-Day Retention

```
30-Day Retention
└── Day-1 Activation Rate           (% of signups who return at all on day 1)
    └── Day-1 → Day-7 Retention     (% of day-1-active users still active on day 7)
        └── Day-7 → Day-30 Retention (% of day-7-active users still active on day 30)
            ├── driven by: Goal-setting in week 1 (Q2 finding — ~1.67x retention multiplier)
            ├── driven by: Nudge open/act rate (relevance signal)
            ├── driven by: Weekly summary open/act rate (the v2 feature)
            ├── driven by: Session frequency (engagement depth)
            └── driven by: Acquisition channel / platform mix (user quality at signup)
```

`day_30 retention = day_1_rate × (day7|day1 conditional rate) × (day30|day7 conditional rate)`

This decomposition matters because each stage has a different likely root cause and a different fix — a decline driven by Day-1 activation points to onboarding, while a decline in Day-7→Day-30 points to mid-funnel relevance (the problem this initiative targets).

## 2. What Caused the Decline, Weeks 1–4

```sql
SELECT cohort_week,
ROUND(AVG(day_1)*100,1) day1,
ROUND(AVG(CASE WHEN day_1=1 THEN day_7 END)*100,1) day7_given_day1,
ROUND(AVG(CASE WHEN day_7=1 THEN day_30 END)*100,1) day30_given_day7
FROM nudge_retention GROUP BY cohort_week ORDER BY 1;
```

| cohort | day-1 activation | day1→day7 conditional | day7→day30 conditional |
|---|---|---|---|
| 1 | 90.0% | 61.1% | 38.3% |
| 2 | 90.0% | 54.4% | 30.2% |
| 3 | 97.0% | 48.5% | 31.3% |
| 4 | 92.0% | 43.5% | 31.8% |

**Diagnosis:** Day-1 activation is flat-to-rising (90→97%) — onboarding isn't the problem. The decline is concentrated almost entirely in the **day-1→day-7 conditional rate**, which falls steadily from 61.1% to 43.5% across cohorts 1-4 (a ~18pp drop). The day-7→day-30 conditional rate is roughly flat (~30-38%) across the same cohorts. Two supporting signals point to the same stage:
- Average sessions per user fall from 5.74 (cohort 1) to 3.69 (cohort 4) — users are disengaging within the first week, not later.
- Nudge open rate dips in cohort 4 (9.4%, the lowest of all cohorts) alongside the lowest day1→day7 rate.

**Conclusion:** the decline is a **week-1 engagement collapse**, not an onboarding problem or a later-stage churn problem — exactly the "stops feeling relevant after week 1" hypothesis this initiative was built on, and it shows up as a measurable, isolatable stage in the funnel, not just a qualitative impression.

## 3. What the Week 5 Split Tells Us About What the Feature Fixed

| cohort | day1→day7 conditional | day7→day30 conditional | avg sessions/user |
|---|---|---|---|
| 4 (pre-feature) | 43.5% | 31.8% | 3.69 |
| 5 (summary_v1 introduced) | 60.0% | 31.1% | 4.48 |

**Diagnosis:** Cohort 5's day1→day7 conditional rate jumps back up to 60.0% — nearly back to cohort 1's level (61.1%) and a ~16.5pp recovery from cohort 4. Average sessions per user also recovers (3.69→4.48). **But the day7→day30 conditional rate does not move (31.8%→31.1%, essentially flat)** — it's been flat across every single cohort regardless of the feature.

**What this means:** the weekly summary fixes exactly the stage it was designed for — re-engaging users in the first week so they don't go passive — but it has **not** moved the later-stage (day 7-30) retention conditional at all. The feature is solving the "week 1 cliff," not the "sustained relevance through day 30" problem. This is consistent with Q3/Q4's findings (summary_v1 lifts day-7 retention by +30pp but day-30 by only +14pp — the day-30 lift is smaller because it's riding on the day-7 recovery, not adding an independent late-stage boost).

## 4. Four Ranked Hypotheses: Why Did Some Treatment Users Still Churn?

32 of the 50 `summary_v1` users in cohort 5 churned despite the feature. Ranked by how strongly the data already points to each:

### Hypothesis 1 (highest confidence): The feature re-engages week 1 but doesn't sustain engagement through day 30
Day7→day30 conditional retention is flat at 31.1% for cohort 5 — identical to every pre-feature cohort. The summary isn't failing to land; it's succeeding at the one stage it targets and simply isn't designed to address the next stage.
**Data to confirm/rule out:** Track per-user open trajectories from week 1 to week 4 for churned vs. retained summary_v1 users. Already partially checked: churned users actually had *higher* open rates by week 4 (62.5%) than retained users (44.4%) — meaning churners weren't disengaging from the email/notification, they were disengaging from the app despite still opening sends. This rules out "users stopped getting the nudge" and instead **confirms** that opening the summary doesn't reliably convert to an app session — a content/in-app-action gap, not a delivery gap.

### Hypothesis 2 (moderate confidence): Platform-specific friction in the post-open experience
Among week-5 `summary_v1` users, Android skews heavily toward the churned group (16 of 32 churned, 50%) vs. retained (6 of 18, 33%); iOS skews toward retained (11 of 18, 61%) vs. churned (12 of 32, 37.5%).
**Data to confirm/rule out:** Compare deep-link success/crash rates and load times for the in-app weekly summary screen by platform (not in this dataset — would need app telemetry). If Android shows higher screen-load failure or slower load times, this explains the gap; if platform mix is the same after controlling for acquisition channel, this hypothesis weakens.

### Hypothesis 3 (moderate confidence): Acquisition channel quality, not the feature itself, explains some churn
Among week-5 `summary_v1` churners, organic users are under-represented (53% of churned vs. 72% of retained) while paid+referral are over-represented (46.9% of churned vs. 27.8% of retained).
**Data to confirm/rule out:** Check whether this channel skew exists in the *control* group too (i.e., is it a pre-existing churn-by-channel pattern unrelated to the summary feature, or unique to summary_v1)? If control shows the same channel skew, this is a general acquisition-quality issue, not a feature-specific failure — would need a query splitting churn-by-channel within control as well to fully rule in/out.

### Hypothesis 4 (lower confidence, but actionable): Opening the summary isn't translating into the specific action that drives retention (goal-setting)
Within week-5 `summary_v1` users, the rate of having set a goal in week 1 is nearly identical between churned (43.8%) and retained (44.4%) — and `acted_on` rate for weekly summary sends is actually slightly *higher* among churners (19.5%) than retainers (13.9%). This suggests the summary's specific nudge content/CTA isn't reliably driving the one action (goal-setting) most strongly tied to retention in this dataset (Q2 finding).
**Data to confirm/rule out:** Break down `acted_on` by `nudge_type` to see whether "acted_on" actions are goal-related or something else (e.g., dismissing a subscription nudge). If acted-on actions are concentrated in non-goal nudge types, this would confirm the summary is driving the wrong action; if goal-related actions dominate "acted_on" but still don't move retention, this would point instead toward Hypothesis 1 (the goal action itself isn't sufficient without continued engagement).

## Sources
- `P5L1_Nudge_Dataset.xlsx`, queried via SQLite (same dataset as `data/metric-findings.md`)
- `data/metric-findings.md` — base findings this diagnosis builds on
- `CLAUDE.md` — original "stops feeling relevant after week 1" hypothesis, now confirmed as an isolatable day1→day7 funnel stage rather than a vague qualitative claim
