# Spec Readiness Review: Weekly Summary Feature

**Spec reviewed:** `docs/pm-brief.md` (prototype brief — being treated here as the working spec ahead of production sprint planning)
**Reviewer persona:** Raj (Tech Lead), playing skeptical

## Top 3 Questions This Spec Doesn't Answer

### 1. Where does insight/nudge computation actually run in production, and how often?
The spec says nudge selection "scores candidates against multiple signals" and the prototype runs this client-side against hardcoded data. It says nothing about production: is this computed on-demand when the user opens the app, or pre-computed on a schedule (e.g., nightly, weekly)? Does it run as a new background job, or extend an existing sync/categorization pipeline? Without this, there's no way to estimate engineering effort, infra cost, or data freshness guarantees.

**What would resolve it:** A decision on compute model — e.g., "runs as a scheduled job after the nightly account sync, writes the week's insight + nudge + goal snapshot to a new table, read by the app at request time" — plus which existing job/service it hooks into.

### 2. What are the exact rules and edge cases for nudge selection, not just the scoring concept?
The spec describes "a small set of candidate nudges, each scored against multiple signals" but doesn't enumerate the candidates, the exact signal definitions, the weights, or — critically — what happens in edge cases: no transactions this week, a tie between two candidates, a user with no spending history at all (new account), or a user who's already acted on the same nudge multiple weeks running (does it repeat?). Engineering can't estimate or build against "a small set of candidate nudges" — that's a placeholder, not a spec.

**What would resolve it:** An explicit list of nudge candidates with their trigger conditions, the scoring formula/weights, and defined fallback behavior for each edge case (no data, ties, repeats).

### 3. What's actually in scope for the first production sprint vs. deferred?
The spec's "Out of scope" section only addresses prototype-level concerns (real Plaid integration, persisted state) — it doesn't address the newer signals from usability testing (goal visualization, subscription-cancel checkboxes, category/merchant ranking, simplifying the "Why this nudge?" panel). Without an explicit cut line, scope creep is likely the moment design or research raises any of these in sprint planning.

**What would resolve it:** An explicit "Sprint 1 scope" list naming the 3 core elements only (top insight, one nudge, goal progress) and a deferred list naming everything else, with a stated reason for the cut (e.g., "not yet validated beyond n=1").

## Rewritten Sections With Gaps Filled

### Computation & Architecture *(new section)*
- Insight and nudge computation run as a scheduled background job, triggered after the existing account sync completes (not on-demand at request time), producing one insight + one ranked nudge + goal snapshot per user per week.
- Results are persisted (new table/record) and read by the app at render time — the weekly summary screen does not compute anything live.
- *(Owner: Raj to confirm which existing sync/job pipeline this should hook into, and propose the persistence model.)*

### Nudge Candidates & Edge Cases *(replaces vague "small set of candidate nudges")*
- Enumerate the exact nudge candidates to ship in Sprint 1 (e.g., category spend spike, no-goal-set prompt, subscription review) with explicit trigger conditions and scoring weights — not left as "a small set," but a finalized, numbered list before sprint kickoff.
- Defined fallback behavior required for: no transactions in the week, a scoring tie, a new user with no prior week to compare against, and repeat presentation of the same nudge across multiple weeks.
- *(Owner: PM to finalize the candidate list and edge-case rules with Raj before sprint kickoff — this is the single biggest blocker to estimation.)*

### Sprint 1 Scope *(replaces the prototype-only "Out of scope" section)*
**In scope for Sprint 1:**
- Top spending insight (one per week, computed from real transaction data)
- One ranked nudge (from a finalized candidate list, see above)
- Savings goal progress (both goal-set and no-goal states)

**Explicitly deferred (not Sprint 1):**
- Goal visualization / "what this becomes" reward framing — raised by 1 usability participant, not yet validated
- Subscription multi-select/cancel-in-one-click — flagged as a larger build, needs its own design pass
- Category/merchant ranking beyond the single top insight
- Simplifying or removing the "Why this nudge?" transparency panel
- Push notification delivery — no notification infrastructure exists today (confirmed via `docs/codebase-summary.md` reference review); this is a separate scoping decision, not assumed in-scope here

## Source Documents
- `docs/pm-brief.md` — spec under review
- `docs/hypothesis.md` — known/assumed/unknown framing, used to justify the deferred-scope cuts (most deferred items are "what we assume" or single-data-point requests, not yet validated)
- `docs/iteration1-feedback.md` — source of the deferred feedback items
