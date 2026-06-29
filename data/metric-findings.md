# Metric Findings: Weekly Summary Feature

**Data source:** `P5L1_Nudge_Dataset.xlsx` (500 users, 2,347 sessions, 500 retention records, 1,740 nudges, 400 weekly summary sends), loaded into a local SQLite database and queried directly. All numbers below are computed from the actual dataset, not estimated.

## 1. 30-Day Retention by Cohort Week

```sql
SELECT cohort_week,
       COUNT(*) AS n_users,
       ROUND(AVG(day_30)*100,1) AS day30_retention_pct
FROM nudge_retention
GROUP BY cohort_week
ORDER BY cohort_week;
```

**Plain English:** For each weekly signup cohort, take the fraction of users still active on day 30 (the `day_30` flag), and average it across all users in that cohort.

**Result:**
| cohort_week | n_users | day_30 retention |
|---|---|---|
| 1 | 100 | 32.0% |
| 2 | 100 | 25.0% |
| 3 | 100 | 25.0% |
| 4 | 100 | 22.0% |
| 5 | 100 | 29.0% |

**What it means for scaling the weekly summary:** Retention declined from 32% (cohort 1) to a low of 22% (cohort 4) — consistent with the documented 44%→37% retention drop this initiative is responding to. Cohort 5 (29%) breaks the decline and is notable because it's also the first cohort with a `summary_v1` variant present in the data (see Q3) — this is the first signal in the raw cohort trend that the intervention may be working, but it's confounded with whatever else changed between cohort 4 and 5. This alone isn't sufficient evidence to scale — it motivates looking at the treatment/control split directly (Q3).

## 2. Savings Goal Set in Week 1 vs. Not — Retention Comparison

```sql
SELECT
  CASE WHEN u.goal_set_date <> ''
       AND julianday(u.goal_set_date) - julianday(u.signup_date) <= 7
       THEN 'goal_set_week1' ELSE 'no_goal_week1' END AS goal_group,
  COUNT(*) AS n_users,
  ROUND(AVG(r.day_7)*100,1) AS day7_retention_pct,
  ROUND(AVG(r.day_30)*100,1) AS day30_retention_pct
FROM nudge_users u
JOIN nudge_retention r ON r.user_id = u.user_id
GROUP BY goal_group;
```

**Plain English:** Split users into two groups based on whether `goal_set_date` falls within 7 days of `signup_date` (or is blank, meaning no goal was ever set). Compare day-7 and day-30 retention between the groups.

**Result:**
| group | n_users | day_7 retention | day_30 retention |
|---|---|---|---|
| goal_set_week1 | 165 | 67.3% | 36.4% |
| no_goal_week1 | 335 | 46.3% | 21.8% |

**What it means for scaling the weekly summary:** This strongly confirms the churn signal already named in `CLAUDE.md` ("churn is nearly double for users who don't set a savings goal in week 1") — day-30 retention is 36.4% vs. 21.8%, a ~1.67x difference. This is correlational, not causal (users motivated enough to set a goal in week 1 may simply be more engaged generally), but it directly supports prioritizing the goal-progress card and the goal-visibility fix flagged in `docs/design-review.md` as the highest-impact change.

## 3. Week 5 Cohort Only: Day-7 and Day-30 Retention, Treatment vs. Control

```sql
SELECT u.variant,
       COUNT(*) AS n_users,
       ROUND(AVG(r.day_7)*100,1) AS day7_retention_pct,
       ROUND(AVG(r.day_30)*100,1) AS day30_retention_pct
FROM nudge_users u
JOIN nudge_retention r ON r.user_id = u.user_id
WHERE u.cohort_week = 5
GROUP BY u.variant;
```

**Plain English:** Restrict to users who signed up in cohort week 5 (the only cohort with both `control` and `summary_v1` variants represented), then compare day-7 and day-30 retention between the two groups.

**Result:**
| variant | n_users | day_7 retention | day_30 retention |
|---|---|---|---|
| control | 50 | 46.0% | 22.0% |
| summary_v1 | 50 | 76.0% | 36.0% |

**What it means for scaling the weekly summary:** This is the clearest causal-style evidence in the dataset — within the same cohort week (controlling for signup-time effects), the weekly summary (`summary_v1`) group retained at 76% on day 7 vs. 46% for control (+30pp), and 36% vs. 22% on day 30 (+14pp). Sample size is modest (n=50/arm), so treat the exact magnitude with caution, but the direction and size of the effect are a strong positive signal for scaling.

## 4. Weekly Summary Open Rate Across the 4 Sends vs. Control

```sql
SELECT week_number, variant,
       COUNT(*) AS n_sends,
       ROUND(AVG(opened)*100,1) AS open_rate_pct
FROM nudge_weekly_summary_sends
GROUP BY week_number, variant
ORDER BY week_number, variant;
```

**Plain English:** For each of the 4 weekly summary sends, compare the open rate (`opened` flag) between users who received the real weekly summary (`summary_v1`) and the control group.

**Result:**
| week_number | variant | n_sends | open_rate |
|---|---|---|---|
| 1 | control | 50 | 4.0% |
| 1 | summary_v1 | 50 | 28.0% |
| 2 | control | 50 | 4.0% |
| 2 | summary_v1 | 50 | 52.0% |
| 3 | control | 50 | 4.0% |
| 3 | summary_v1 | 50 | 52.0% |
| 4 | control | 50 | 6.0% |
| 4 | summary_v1 | 50 | 56.0% |

**What it means for scaling the weekly summary:** Control's open rate stays flat (~4-6%) across all 4 sends, as expected for a non-personalized baseline. `summary_v1`'s open rate nearly doubles from week 1 (28%) to week 2 (52%) and continues climbing to 56% by week 4 — the opposite of the "static experience" fatigue pattern this initiative is trying to fix. This is a second independent positive signal (alongside Q3's retention lift): not only does the summary retain users better, engagement with it compounds rather than decays over the first month.

## Overall Recommendation for the Scale Decision

All three quantitative signals point the same direction:
1. Goal-setting in week 1 correlates with ~1.67x day-30 retention (Q2) — supports prioritizing goal visibility, independent of the summary feature.
2. Within the same cohort, `summary_v1` shows a +14pp day-30 retention lift over control (Q3).
3. `summary_v1` open rates *increase* across 4 weeks rather than decay (Q4), suggesting sustained relevance rather than novelty wearing off.

**Caveat:** the treatment/control comparison is only directly observable in cohort week 5 (n=50/arm) — before recommending a full-scale rollout, confirm with Raj/data whether a larger or multi-cohort A/B test is feasible, since a single-cohort, 50-user-per-arm sample is a reasonable signal but not yet a high-confidence basis for a permanent product decision on its own.

## Sources
- `P5L1_Nudge_Dataset.xlsx` (user-provided dataset, queried directly via SQLite)
- `CLAUDE.md` (week-1 goal-setting churn correlation, prior qualitative claim — now also confirmed quantitatively here)
- `docs/design-review.md` (goal-visibility recommendation, now reinforced by Q2)
