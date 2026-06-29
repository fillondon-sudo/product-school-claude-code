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
