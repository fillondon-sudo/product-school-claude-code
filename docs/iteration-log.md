# Iteration Log: Weekly Summary Prototype — Usability Session 1

**Date:** 2026-06-26
**Method:** 1 participant, moderated walkthrough of `prototype/index.html`
**Questions asked:** What do you think this does? / How would you arrive at this experience? / Would you change your spending based on this? / If you had a magic wand, what would you change?

**Sample size caveat:** This is a single-participant session. Findings below are flagged as directional, not validated — patterns are noted as "likely to recur" only where there's a clear usability/heuristic reason to expect other users to hit the same issue, not because we've confirmed it with more people.

## What's Working
- **Core value proposition lands:** the participant correctly identified the screen's purpose unprompted ("review spendings and review my savings... helps me achieve my saving goal") — the job-to-be-done is legible without explanation.
- **It drove a real behavior-change signal:** "absolutely yes... it made me realise I spent too much on subscriptions" — this is the strongest possible usability outcome for this prototype: a stated intent to act, triggered directly by the top insight (the Equinox/subscription spike).
- **Discovery channel preference is clear:** participant wants this delivered via push notification, not solely an in-app surface — consistent with the existing research finding that the weekly email already pulls people in better than the static home feed.

## Top 2 Friction Points

### 1. Duplicate, ambiguous call-to-action when no goal is set
When toggled to "Goal: None," the participant saw two buttons that appeared to do the same thing and couldn't tell which to click to set a goal: *"I see two CTA buttons and they both seem to do exactly the same thing. I'm confused which button to click to set a saving goal."* This was caused by the nudge engine independently surfacing a "Set a savings goal" nudge alongside the goal card's own "Set a savings goal" button — two visually distinct CTAs claiming the same action, but only one (the goal card's) actually worked as expected. This is a real usability defect, not just a one-off reaction — duplicate/competing CTAs for the same action are a well-established source of choice confusion, so this is likely to recur with other users, not specific to this participant.

### 2. Goal target isn't prominent or motivating
The participant didn't register the $500 goal amount until "after a while," and separately asked for the goal to feel "dynamic, moving... showing me what I could do with all the money I saved (e.g. car, bicycle, holiday)." The current goal card shows the number but doesn't make it salient or emotionally resonant — it states a fact rather than visualizing a reward. This is a lower-severity issue than #1 (it didn't block the participant from understanding the screen), but it directly undercuts the "actionable and motivating" goal of the feature.

## Other Feedback Captured (not prioritized for this round)
- Wants checkboxes on subscriptions to multi-select and cancel in one click, with the savings goal updating live as selections change.
- Doesn't understand the "Why this nudge?" scoring detail panel — likely too technical for a consumer-facing surface; may need to be hidden or rewritten in plain language for non-PM users.
- Wants a personalized spending-pattern analysis section, kept concise.
- Wants other categories beyond subscriptions shown, ranked by size, with top merchants highlighted (top 3–10).

## Highest-Priority Change for Next Round
**Eliminate the duplicate "Set a savings goal" CTA.** This is the single most severe issue: it's an active point of confusion on the core action of the screen (setting a goal), it's a logic problem (not just a copy/design tweak), and it stands in the way of measuring whether the rest of the screen works, since a confused user may not get past it. The goal-visualization request (Friction Point #2) and the other "magic wand" asks (checkboxes, merchant ranking, pattern analysis) are valuable but additive — they don't block comprehension the way the duplicate CTA does.

## Change Made
Removed the `no_goal_setup` nudge candidate from the nudge-scoring engine in `prototype/index.html`. The goal card is now the single, unambiguous place to set a savings goal; the nudge slot always surfaces the next-best-ranked spending nudge instead (currently `subscription_audit`, which independently matches the participant's stated interest in reviewing subscriptions). Verified the scoring logic still runs correctly post-change.

## Next Round Suggestions
- Re-test with 4–5 participants minimum before drawing conclusions on frequency of the CTA confusion or validating the goal-visualization request.
- Consider mocking the goal-visualization ("materializing reward") concept as a separate design exploration before building it, given it's a bigger lift than the CTA fix.
- Capture whether participants notice/understand the "Why this nudge?" panel at all, to decide whether to simplify, relabel, or hide it by default.
