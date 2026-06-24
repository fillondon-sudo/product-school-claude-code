# Codebase Tour: maybe-finance/maybe — PM-Level Summary

**Repo:** https://github.com/maybe-finance/maybe
**Status:** Archived/no longer actively maintained as of v0.6.0 (July 2025), open-source under AGPLv3.

## 1. What This Product Does (One Sentence)
Maybe is a self-hosted personal finance app that lets individuals and families connect accounts (via Plaid), track balances, transactions, and investments, and manage budgets — comparable in product surface to Nudge, but self-hosted rather than cloud-managed.

## 2. Codebase Organization

Standard Rails app structure. The interesting product logic lives almost entirely in `app/`.

| Folder | What it does |
|---|---|
| `app/models` | Core data entities — accounts, transactions, categories, rules, users/families. This is where to look first to understand what the product actually models. |
| `app/controllers` | Request handling/routing — maps to user-facing actions (viewing accounts, running imports, managing budgets). |
| `app/jobs` | Background work — account sync, auto-categorization, auto-merchant-detection, rule processing, imports. This is the closest thing to "engagement machinery" in the codebase. |
| `app/services` | Business logic and external integrations (e.g., Plaid sync logic) that's too complex to live directly in models. |
| `app/components` | Reusable UI building blocks. |
| `app/views` | HTML templates (Hotwire/Turbo + Stimulus, not a SPA framework). |
| `app/mailers` | Email notifications — this is the *only* notification-adjacent system present (see Finding below). |
| `app/javascript` | Frontend JS for Hotwire/Stimulus behavior. |
| `db/` | Migrations — useful for tracing how the data model evolved over time. |
| `lib/` | Shared library code outside the Rails app conventions. |

**Notable absence:** there is no `app/models` or `app/jobs` entity for push notifications, in-app alerts, or any kind of ranking/scoring engine for surfacing insights to users. The only outbound user-facing trigger is email (`app/mailers`) and the AI chat assistant (`Chat`/`Message` models). This is directly relevant to finding #5 below.

## 3. The 3 Most Important Files to Know

1. **`app/models/account.rb`** — the central entity of the whole product. Implements a state machine (`draft` → `active` → `disabled` → `pending_deletion`), categorizes balances as cash/non-cash/investment, and uses polymorphic associations to support multiple account subtypes (Depository, CreditCard, Investment, Loan, etc.). Understanding this model is close to understanding the whole data model.
2. **`app/models/rule.rb`** (+ `app/jobs/rule_job.rb`) — the closest thing this codebase has to a "conditional logic engine." Rules have conditions and actions, currently scoped to the `transaction` resource type, and can run synchronously (`apply`) or async (`apply_later`). This is the natural analog to think about if/when this product ever built a nudge or notification-ranking system — it doesn't have one, but this is the pattern it would likely extend.
3. **`app/jobs/sync_job.rb`** — handles syncing account data from external sources (Plaid). Any notification or insight feature would need to hook into (or run after) this, since insights are only as fresh as the last sync.

## 4. Key Data Models & What They Tell Us About Product Decisions

- **Account is polymorphic across subtypes** (Depository, CreditCard, Investment, Loan, OtherAsset/Liability) rather than one flat "account" table — this tells us the product was built to support a comprehensive net-worth view across very different account types, not just a single checking-account-centric experience.
- **Entry is a base abstraction over Transaction, Valuation, and Trade** — the product treats "things that happened to your money" as a unified ledger concept, which is a more general model than Nudge likely needs but is worth noting as a design pattern.
- **Rule + Category + Merchant models exist together** — categorization is rule-driven and merchant-aware, suggesting the product invests in getting transaction metadata right as a foundation, rather than just displaying raw transaction strings.
- **Family (not just User) is a first-class model**, with Users belonging to a Family — this is a multi-user household design decision baked in from the data layer up, not bolted on.
- **Chat/Message/AssistantMessage/ToolCall models** — there's a full AI chat assistant built into the data model, which tells us the product's answer to "how do we surface insight to the user" was conversational/on-demand (user asks a question), not proactive. There's no model for proactively ranking and pushing a single insight to a passive user — confirming the "no notification ranking system exists" finding above.

## 5. What I'd Need to Understand to Write a Good Ticket for a Notification Ranking Feature

Based on this tour, a well-scoped ticket would need to nail down:

- **Where the insight-worthy data already lives** — likely sourced from `Entry`/`Transaction` plus `Category`/`Merchant`, synced via `SyncJob`. The ticket needs to specify what counts as "notification-worthy" (e.g., a category spike, a goal milestone) in terms of these existing models, not invent new data sources.
- **Whether to extend the `Rule` engine or build a separate ranking service** — `Rule` already has a conditions/actions structure scoped to `transaction` resources. The ticket should explicitly decide: extend Rule's resource-type registry to support a "notification" action type, or build a standalone ranking job (more like `auto_categorize_job.rb`) that doesn't try to reuse Rule's conditional engine. This is the single biggest architectural fork in scope.
- **Delivery mechanism gap** — there's no push notification infrastructure today, only email (`app/mailers`). The ticket needs to either include or explicitly exclude building a push/in-app notification delivery layer — it's not something to assume already exists.
- **Where ranking would run and how often** — needs to plug into the existing job/sync cadence (`SyncJob`, likely triggering a new ranking job after each sync) rather than being a separate always-on service, to stay consistent with how the rest of the background processing is architected.
- **How "one ranked insight" interacts with the existing on-demand AI chat** — since the product already lets users ask questions conversationally, the ticket should clarify whether the proactive notification is meant to replace, complement, or eventually feed into that chat surface, to avoid two disconnected "intelligence" systems.

## Sources
- [maybe-finance/maybe](https://github.com/maybe-finance/maybe)
- [app/ folder](https://github.com/maybe-finance/maybe/tree/main/app)
- [app/models folder](https://github.com/maybe-finance/maybe/tree/main/app/models)
- [app/jobs folder](https://github.com/maybe-finance/maybe/tree/main/app/jobs)
- [app/models/rule.rb](https://github.com/maybe-finance/maybe/blob/main/app/models/rule.rb)
- [app/models/account.rb](https://github.com/maybe-finance/maybe/blob/main/app/models/account.rb)
- [README.md](https://raw.githubusercontent.com/maybe-finance/maybe/main/README.md)
