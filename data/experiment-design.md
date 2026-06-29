# Experiment Design: Weekly Summary Full Test

**Builds on:** `data/metric-findings.md`, `data/metric-diagnosis.md`, `docs/recommendation-memo.md`.
**New parameters:** MDE = 5pp, Power = 80%, Significance = 95%, Available WAU = 85,000, Max test duration = 8 weeks.

## 1. Is the Pilot Result Statistically Significant at n=50?

Two-proportion z-test, pilot cohort week 5 (n=50/arm):

```
Day-30 retention:  control 22% (11/50)  vs.  summary_v1 36% (18/50)
pooled p = 0.29
SE = sqrt(0.29 × 0.71 × (1/50 + 1/50)) = 0.0908
z = (0.36 − 0.22) / 0.0908 = 1.54
two-sided p-value ≈ 0.123
```

**Result: NOT statistically significant at the 95% confidence level** (p=0.123, need p<0.05 / |z|>1.96). This confirms the VP's skepticism in `docs/recommendation-memo.md` — the +14pp day-30 lift is directionally promising but the pilot sample (n=50/arm) is too small to rule out chance.

**Worth noting:** the day-7 retention gap (76% vs. 46%) is significant — z=3.08, p≈0.002. The earlier, larger effect on day-7 retention is real; the smaller day-30 effect is the one that isn't yet statistically distinguishable from noise.

## 2. Required Sample Size Per Variant for the Full Test

Standard two-proportion sample size formula, using control's observed day-30 baseline (22%) and a 5pp MDE (22%→27%):

```
n per arm = (z_α/2 + z_β)² × (p1(1−p1) + p2(1−p2)) / (p2−p1)²
          = (1.96 + 0.8416)² × (0.22×0.78 + 0.27×0.73) / 0.05²
          = 7.849 × 0.3687 / 0.0025
          ≈ 1,158 per arm
```

**Required: ~1,158 users per variant (~2,316 total).**

## 3. How Many Weeks Does the Full Test Need to Run?

Two components: time to enroll the required sample, plus the 30-day observation window needed to measure day-30 retention for the *last* enrolled user.

- **Enrollment:** 2,316 total users is small relative to 85,000 available WAU — enrollment isn't the bottleneck. Spreading enrollment over **2 weeks** (rather than one large batch) gives more stable day-to-day randomization and a buffer against any single-week anomaly (e.g., a marketing push skewing one week's signups, the confounding risk flagged by the VP).
- **Observation:** the last user enrolled in week 2 needs 30 days (≈5 weeks) to reach their day-30 retention measurement.

**Total: 2 weeks enrollment + 5 weeks observation = 7 weeks**, within the 8-week cap (1-week buffer for any enrollment slippage).

## 4. Wait for the Full Test, or Recommend Scaling Now?

**Recommendation: Wait for the full test.** The pilot's day-30 lift — the metric that matters for the actual scale decision — is not statistically significant (p=0.123), and the required sample size to detect a 5pp effect with 80% power (1,158/arm) is achievable in 7 weeks, inside the 8-week budget. Scaling now on an unproven day-30 result risks committing engineering and design resources, plus the goal-visibility and content-conversion gaps already identified in `data/metric-diagnosis.md`, before confirming the feature reliably moves the metric Marcus owns. The day-7 result is already significant and directionally strong enough to justify running the full test rather than abandoning the feature — this isn't a "kill it" situation, it's a "prove it before scaling" situation.

## 5. Leading Indicators to Monitor While the Full Test Runs

- **Weekly day-7 retention by arm** — matures fast (already significant in the pilot) and gives an early read on whether the effect is holding before day-30 data is available.
- **Weekly summary open and acted-on rate by arm, tracked weekly per cohort** — should keep climbing or holding steady, not decaying; a drop here would be an early warning sign before it shows up in retention.
- **Day1→day7 conditional retention rate, weekly** — this is the specific funnel stage the feature is designed to fix (per `data/metric-diagnosis.md`); if it doesn't recover toward the ~60% level seen in the pilot, the full test is unlikely to show a day-30 effect either.
- **Randomization balance check (acquisition channel, platform mix, signup volume) between arms each week** — directly addresses the VP's confounding concern; if the arms drift apart on these dimensions, the test's internal validity is compromised regardless of the outcome.
- **Guardrail/harm metrics** — notification opt-out rate and any app-rating/support-ticket signal, to catch a negative side effect early rather than only at the 7-week readout.

## Sources
- `data/metric-findings.md`, `data/metric-diagnosis.md` — pilot data this design is calibrated against
- `docs/recommendation-memo.md` — the VP pressure-test questions this experiment design directly answers
