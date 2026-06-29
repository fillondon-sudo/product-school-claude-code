# Change Log: Engage v2

## Day 1 — 2026-06-22 — Discovery phase kickoff
- Confirmed the retention drop (44% → 37% over two quarters) as the core problem to address.
- Aligned with Raj and Lena on the passivity hypothesis: the app stops feeling relevant after week 1.
- Surfaced supporting signals: savings-goal correlation with churn, static home feed, email/in-app discontinuity.
- Drafted a proposed direction (personalized weekly summary screen) — not yet agreed, pending Thursday's problem-alignment meeting.
- Set up project.md (overview), strategy.md (hypothesis), and change_log.md (this file) to track Engage v2 work.
- Built a reusable skill (`skills/weekly-status.md`) for turning raw notes into a formatted leadership status update.

## Day 2 — 2026-06-26 — Research synthesis and prototype
- Synthesized 3 user interviews (`research/interview-synthesis.md`) — top theme: repetitive/static experience and a ~3-month time-to-value risk.
- Analyzed NPS feedback (`research/nps-analysis.md`) — independently confirmed the same top theme (static/repetitive experience).
- Researched 3 competitors — Monarch Money, Cleo, Rocket Money (`research/competitive-matrix.md`) — identified two white-space gaps: proactive ranked weekly guidance, and email/notification-to-in-app continuity. Neither gap is owned well by competitors, both align with internal research findings.
- Synthesized all research into a 1-page decision brief for Marcus (`docs/decision-brief.md`) recommending the personalized weekly summary screen as the Engage v2 direction.
- Built an interactive prototype of the personalized weekly summary screen (`prototype/index.html`, `prototype/README.md`) with scored/ranked nudge selection and a toggleable goal-set/no-goal state, using fabricated transaction data per `docs/pm-brief.md`.
- Ran usability session 1 on the prototype (`docs/iteration-log.md`) — top friction: duplicate/ambiguous "Set a savings goal" CTA when no goal is set. Fixed by removing the duplicate nudge candidate; goal card is now the single CTA for setting a goal.
- Structured and ranked all session 1 feedback into a single reviewable list (`docs/iteration1-feedback.md`) for deciding scope of the next prototype iteration.
- Wrote a learning synthesis (known/assumed/unknown) and formal hypothesis statement (`docs/hypothesis.md`) for the personalized weekly summary screen, tying it to the 30-day retention metric.
- Prepared the triad working session agenda and post-session alignment doc template (`docs/triad-session.md`) ahead of reviewing the prototype with Raj and Lena.
- Reviewed the maybe-finance/maybe open-source codebase as a PM-level reference (`docs/codebase-summary.md`) — confirmed it has no notification/insight-ranking system, only email and on-demand AI chat, informing scope considerations for a notification ranking ticket.
- Ran a spec readiness review on the weekly summary brief (`docs/spec-readiness.md`) — identified 3 gaps (production compute model, nudge edge-case rules, sprint scope cut line) before sprint kickoff with Raj.
- Wrote a one-page evidence-grounded design review of the prototype (`docs/design-review.md`) — identified goal visibility/motivation as the highest-impact change for week-1 retention given its direct link to the week-1 goal-setting churn signal, and split remaining open items into Lena's design ownership vs. product-level scope decisions.
- Produced a QA artifact for the notification ranking feature ahead of QA handoff (`docs/qa-checklist.md`) — edge case list grouped by empty states/edge data/multi-account/permissions, a 10-item PM sign-off checklist, and a PR comment template tied to Sprint 1 scope and unresolved tiebreak/repeat-nudge rules from `docs/spec-readiness.md`.
- Ran SQL analysis against the actual Nudge dataset (`P5L1_Nudge_Dataset.xlsx`) and saved findings to `data/metric-findings.md` — confirmed 30-day retention decline by cohort, quantified the week-1 goal-setting retention lift (36.4% vs 21.8%), found a +14pp day-30 retention lift for the weekly summary vs. control in cohort week 5, and found summary open rates climbing (28%→56%) rather than decaying across 4 sends — overall a positive signal for scaling, with a caveat on sample size (n=50/arm, single cohort).
- Extended the goal-setting retention comparison to break out by cohort week 1-5 (`data/metric-findings.md`) — found the churn gap between goal-setters and non-setters widens over time (8.1pp in cohort 1 vs 25.6pp in cohort 4), reinforcing goal visibility as a durable retention lever independent of cohort-level decline.
- Built a retention metric tree and diagnosis (`data/metric-diagnosis.md`) — isolated the weeks 1-4 decline to a collapsing day1→day7 conditional retention stage (61.1%→43.5%), confirmed the week-5 weekly summary feature fixes exactly that stage (back up to 60.0%) without moving the flat ~31% day7→day30 stage, and produced 4 ranked, data-grounded hypotheses for why treatment users still churn (sustained engagement gap, platform friction, acquisition channel quality, action-content mismatch) each with a confirm/rule-out data plan.
- Wrote a results memo for Marcus (`docs/recommendation-memo.md`) recommending scaling the weekly summary while flagging the unsolved day7→day30 stage, plus a skeptical-VP-of-Engineering pressure test on sample size, confounding, and projected steady-state impact.
