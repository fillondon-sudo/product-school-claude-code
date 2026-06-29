# QA Artifact: Weekly Summary Notification Ranking Feature

**Feature reviewed:** Top insight + ranked nudge + savings goal progress (per `docs/pm-brief.md`, `docs/spec-readiness.md`)
**Status:** Heading into QA ahead of Sprint 1

## 1. Edge Case List

### Empty States
- No transactions at all this week (new user or zero activity) — what renders for the insight card?
- No transactions *last* week but transactions this week — week-over-week delta is undefined/divide-by-zero risk
- No savings goal set — confirm only one CTA renders (the duplicate-CTA defect found in usability session 1 — see `docs/iteration-log.md`)
- No nudge candidate scores above a usable threshold — does the screen show a blank nudge slot, or a fallback message?
- User has connected an account but sync hasn't completed yet — insight/nudge computed on stale or partial data?

### Edge Data Conditions
- Exactly one transaction in the week — insight computation shouldn't break on a sample size of 1
- Two or more candidate nudges tie on score — confirm a deterministic tiebreak rule exists (per `docs/spec-readiness.md`, this was flagged as unresolved)
- Duplicate or near-duplicate categories (e.g. "Dining" vs "Restaurants" from different merchant feeds) inflating or splitting a category's totals
- Negative amounts / refunds appearing as transactions — do they get counted as spend, reducing spend, or excluded?
- A single transaction so large it dominates every category (e.g. annual insurance payment) — does it falsely trigger a spending-spike nudge?
- Same nudge candidate winning multiple weeks in a row — repeat-nudge fatigue, no rule defined yet per spec-readiness review
- Savings goal already met or overshot (saved > target) — progress bar/percentage math at >100%
- Goal target set to $0 or a negative number

### Multi-Account Scenarios
- User has multiple connected accounts (e.g. checking + credit card) — is spend aggregated across accounts or scoped to one?
- One account sync succeeds, another fails for the week — partial data silently understates spend without flagging it
- User disconnects an account mid-week — does the insight recompute, or reference a now-missing account?
- Shared/joint account scenario — whose "savings goal" and "spend" is being summarized if multiple users see the same account?

### Permission States
- Push notifications disabled — does the weekly summary still generate and wait in-app, or is computation skipped entirely?
- Notification permission revoked *after* a nudge was already scheduled — stale notification referencing data that's since changed
- Location permission off (if any nudge logic depends on location, e.g. merchant-proximity nudges) — confirm graceful fallback, not a silent failure
- Account-sync permission/consent revoked (Plaid-style) — insight/nudge should not run on an account the user has disconnected access to
- Marketing/communication opt-out vs. core in-app notification — confirm these are handled as distinct permission states, not conflated

## 2. PM QA Checklist — Top 10 Before Sign-Off

1. **Empty state correctness** — no transactions, no goal, and no qualifying nudge each render a defined fallback (not a blank screen or error).
2. **Single, unambiguous CTA when no goal is set** — re-verify the duplicate-CTA defect from usability session 1 has not regressed.
3. **Tie-break and repeat-nudge rules exist and are testable** — confirm engineering implemented the rules from `docs/spec-readiness.md`, not just the happy path.
4. **Insight math holds at the edges** — zero-spend weeks, one-transaction weeks, and refunds/negative amounts don't produce nonsensical deltas or percentages.
5. **Goal progress renders correctly above 100% and at exactly the target** — no broken progress bar or divide-by-zero.
6. **Multi-account aggregation behaves as specified** — confirm with engineering whether spend is meant to be account-scoped or household-wide, and that the build matches the decision.
7. **Partial sync failure doesn't silently produce a misleading insight** — either the screen flags incomplete data, or computation is deferred until sync completes.
8. **Notification-permission-off path is non-breaking** — in-app weekly summary still computes/renders even if push delivery is disabled or revoked.
9. **Disconnected/revoked account is excluded from computation** — no insight or nudge ever references data the user has disconnected.
10. **Sprint 1 scope boundary is respected in the build** — confirm nothing from the explicitly deferred list (goal visualization, subscription multi-select, category/merchant ranking, push delivery) has been partially or accidentally shipped, per `docs/spec-readiness.md`.

## 3. PR Comment Template (First Meaningful Code Review)

```
Thanks for getting this up — reviewing against the Sprint 1 scope and edge cases from docs/qa-checklist.md.

**Scope check**
- [ ] Confirms to the Sprint 1 in-scope list (top insight, one nudge, goal progress) with nothing from the deferred list included
- [ ] Matches the compute model agreed in docs/spec-readiness.md (scheduled job after sync, not on-demand)

**Edge cases I tested / want confirmed**
- [ ] No transactions this week
- [ ] No transactions last week (delta calc)
- [ ] Exactly one transaction
- [ ] Two nudge candidates tied on score — what's the tiebreak?
- [ ] Same nudge winning multiple weeks in a row — is repeat suppression intentional or accidental?
- [ ] Goal saved > target (overshoot)
- [ ] Notifications permission off — does in-app summary still compute?
- [ ] Multi-account user — confirming aggregation scope matches the intended design

**Questions**
- [Specific question about an implementation choice, tied to a line/file]
- [Anything that diverges from the agreed spec — ask rather than assume intent]

**Nit / non-blocking**
- [Optional style/naming notes, clearly marked as non-blocking]

Flagging rather than blocking on [X] since it's a smaller fix — happy to pair if useful.
```

## Sources
- `docs/pm-brief.md`, `docs/spec-readiness.md` — feature spec and unresolved edge-case/scope questions
- `docs/iteration-log.md`, `docs/iteration1-feedback.md` — usability-discovered defects (duplicate CTA, goal visibility)
- `prototype/index.html` — current scoring/insight logic referenced for edge-case derivation
