# PM Brief: Personalized Weekly Money Summary — Prototype

## User
28-year-old who connected their Chase account 2 weeks ago and has not opened the app since.

## Job to be done
Understand where their money went this week and take one action.

## Feature
Personalized weekly money summary:
- Top spending insight
- One contextual nudge
- Savings goal progress

## Constraint
Use data Nudge already has — no new integrations.

## Prototype scope
- Interactive logic, not a static mockup: insight detection and nudge selection run against fabricated transaction data.
- Fabricated a realistic ~2-week set of Chase-style transactions for a fictional 28-year-old user.
- Nudge selection: a small set of candidate nudges, each scored against multiple signals (spend pattern, goal status, recency), prototype picks and surfaces the best-fit nudge.
- Savings goal progress: demoes both states — active goal (progress shown) and no goal set (prompt to set one) — to reflect the research finding that churn nearly doubles for users without a week-1 goal.
- Self-contained build: single `prototype/index.html` (HTML/CSS/JS, fabricated data and scoring logic embedded, no backend/build step).

## Out of scope
- Real Chase/Plaid integration
- Production-grade ranking model
- Persisted user state across sessions
