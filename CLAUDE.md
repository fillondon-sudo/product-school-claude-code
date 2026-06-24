# Context: Nudge — Engage Squad

## Product
Nudge is a consumer personal finance app that helps people build better money habits. Users connect bank accounts, see where their money is going, set savings goals, and get nudges to stay on track.

- Launched 4 years ago
- Series B funded ($42M)
- 2.1M registered users, 340K monthly active users
- Growing 28% YoY on MAU

## My role
PM owning the Engage squad — everything related to keeping users active after they connect their first account: home feed, weekly money summaries, savings nudges, push notifications.

Triad: Raj (Senior Engineer), Lena (Product Designer), me. Report to Marcus (Head of Product).

## Current situation
Acquisition funnel works: users sign up, connect a bank account, see their spending breakdown. The problem is what happens next — a large portion of users go passive after the initial aha moment, open the app less, stop responding to nudges, and eventually churn.

30-day retention (active 30 days after connecting first account) dropped from 44% to 37% over the last two quarters.

**Core metric:** 30-day retention (% of users still active 30 days after connecting their first account). Currently 37%, down from 44%.

**Key tension:** Pressure to move toward a solution (personalized weekly summary screen) vs. the need to confirm the team is aligned on the actual problem before committing to that design. Marcus explicitly wants problem alignment first.

**Open decision:** Whether the personalized weekly summary screen is the right Engage v2 direction, and what its success metrics/non-goals should be — to be resolved in Thursday's meeting.

## Active initiative: Engage v2
Hypothesis: users go passive because the app stops feeling relevant after the first week. The initial spending breakdown is compelling, but after that Nudge hasn't given users a reason to come back that feels personal, timely, or actionable.

Supporting signals from the team:
- Churn is nearly double for users who don't set a savings goal in week 1
- Home feed looks identical regardless of how long it's been since last open
- Weekly summary email has ~22% open rate, but the in-app experience after clicking through doesn't continue that story
- Proposed direction: personalized in-app weekly summary screen (top insight, one actionable nudge based on real usage, savings goal progress)

## Ways of working
- Ground recommendations in actual data/quotes from the team — don't invent details
- Clearly label speculative/proposed content as such (e.g., "proposed — confirm with team")
- Default to clarifying the problem before proposing solutions

## Tracking docs
- `project.md` — what Nudge is, squad, current phase, key stakeholders
- `strategy.md` — hypothesis/strategy for recovering the retention drop
- `change_log.md` — dated log of significant decisions/milestones

During conversations, proactively prompt when something discussed seems worth saving to one of these files (or a new one we create later) — don't wait to be asked. Ask before writing to `project.md` or `strategy.md`; don't auto-save those without confirmation.

`change_log.md` is the exception: log significant milestones (new docs created, research completed, decisions made) automatically as they happen, without asking first.
