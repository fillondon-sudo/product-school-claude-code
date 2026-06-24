# Prototype: Personalized Weekly Money Summary

Interactive prototype for the Engage v2 weekly summary screen. Open `index.html` directly in a browser — no build step, no dependencies.

## What it demonstrates
- **Top spending insight**: computed from a fabricated 2-week transaction history for a fictional 28-year-old user (Jordan), comparing this week's spend by category against last week's to find the biggest increase.
- **Contextual nudge**: a small set of candidate nudges (dining spike, no-goal-set, subscription audit, goal boost) are scored against four signals (magnitude, relevance, actionability, recency) and weighted; the highest-scoring nudge is surfaced. Expand "Why this nudge?" to see all candidates and their scores.
- **Savings goal progress**: toggle "Goal: Active / None" to demo both states — an active goal with a progress bar, and the no-goal state with a prompt to set one, reflecting the research finding that churn nearly doubles for users without a week-1 goal.
- Clicking the nudge button simulates the user taking the suggested action.

## Data
All transaction and goal data is fabricated and hardcoded in `index.html` (`transactions` array and `goalFixture` object) — no real accounts, no API calls, no integrations. This matches the brief's constraint to use only data Nudge already has (transactions, account connection, goal status).

## Known limitations (prototype only)
- Nudge scoring weights are illustrative, not validated against real user behavior.
- No persistence — refreshing resets state (nudge-acted flag, goal toggle).
- Only one fabricated user/dataset; not built to handle arbitrary real accounts.
- "This week vs last week" comparison is hardcoded to the two weeks of fabricated data.

## Related docs
- `docs/pm-brief.md` — brief this prototype was built from
- `docs/decision-brief.md` — research synthesis supporting the Engage v2 direction
