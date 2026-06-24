# Triad Working Session: Weekly Summary Prototype Review

**Attendees:** Raj (Eng), Lena (Design), me (PM)
**Duration:** 30 minutes
**Purpose:** Walk through the tested prototype and usability findings, and align on what goes into the next build.

## Agenda

### 1. Context recap (3 min)
- Quick restate of the problem: 30-day retention dropped 44%→37%, root cause is the app going static/un-guiding after week 1 (per `docs/decision-brief.md`).
- Hypothesis we're testing: personalized weekly summary screen → renewed week-over-week relevance → improved 30-day retention (`docs/hypothesis.md`).

### 2. Live walkthrough of the prototype (7 min)
- Demo `prototype/index.html`: top insight, scored nudge, goal progress (both goal-set and no-goal states).
- Show the fix already made: removed duplicate "Set a savings goal" CTA that confused the usability participant.

### 3. Usability findings review (8 min)
- Walk through `docs/iteration-log.md` and the ranked list in `docs/iteration1-feedback.md`.
- Flag clearly: this is n=1 — directional, not validated. Frame the discussion as "what's worth testing further" not "what's confirmed."

### 4. Open discussion (10 min)
**Questions to ask:**
- Raj: Of the unaddressed feedback (goal visualization, subscription checkboxes, category/merchant ranking), what's actually buildable with existing data and infra, and what would require new work?
- Raj: Does removing the duplicate-CTA nudge candidate change anything about the scoring engine's reliability we should sanity-check before testing wider?
- Lena: Does the goal-visualization request ("show me what this becomes — a car, a bike, a holiday") feel like a real design direction, or a one-off preference we shouldn't over-index on from a single participant?
- Lena: Is the "Why this nudge?" transparency panel worth keeping for consumers at all, or should it be cut/simplified before more testing?
- All: Are we comfortable testing the current build with more participants as-is, or do we want to make any of the open fixes first?

### 5. Decisions to walk out with (2 min)
- [ ] Which 1–2 items from the ranked feedback list (if any) get built before the next usability round
- [ ] Whether the next round is more 1:1 usability sessions or a slightly wider test
- [ ] Who owns the next build change (Raj for logic, Lena for any visual/interaction design)
- [ ] Rough timing for round 2 testing

---

# Post-Session Alignment Doc Template

*(To be filled in after the session and saved as a dated follow-up, e.g. `docs/triad-session-2026-06-XX-notes.md`)*

## Session: Weekly Summary Prototype Review — [date]
**Attendees:**

## What We Showed
-

## What We Heard
- Raj:
- Lena:

## Decisions Made
1.
2.
3.

## What's Going Into the Next Build
| Item | Owner | Notes |
|---|---|---|
| | | |

## What We're Explicitly Not Doing (and why)
-

## Next Testing Round
- **Format:**
- **Target participants:**
- **Target date:**

## Open Questions / Follow-ups
-
