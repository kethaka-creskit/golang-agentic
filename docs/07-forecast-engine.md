# Forecast Engine

This document specifies how V1 turns assumptions, policies, scenarios, and conditional plans into deterministic Forecast Runs.

Domain invariants live in [Domain Invariants](04-domain-invariants.md). Scenario and trigger rules live in [Scenarios and Conditional Planning](08-scenarios-and-conditional-planning.md).

## Forecast Run

A Forecast Run is immutable output from:

- One Startup Model
- One approved Model Version
- One Scenario
- One set of selected policies
- One monthly forecast horizon

Rules:

1. Forecast Runs are deterministic.
2. Equal inputs produce equal results.
3. Forecast Runs are not edited after creation.
4. Every material output records calculation lineage.
5. Invalid Forecast Runs cannot be used as investor-ready outputs.

## Monthly Evaluation Order

For each forecast period:

1. Bring forward opening balances.
2. Apply approved assumption changes effective in the period.
3. Apply previously triggered actions effective in the period.
4. Resolve scenario overrides effective in the period.
5. Calculate customer acquisition and conversion by segment.
6. Calculate churn, contraction, and retention by segment.
7. Calculate ending customer quantities by segment and in total.
8. Calculate earned revenue.
9. Calculate billing, collections, receivables, and deferred revenue.
10. Calculate variable costs.
11. Calculate workforce costs.
12. Calculate fixed operating costs.
13. Calculate capital purchases and depreciation.
14. Calculate supplier payments, prepayments, and accounts payable.
15. Calculate financing cash flows and debt balances.
16. Calculate simplified tax.
17. Produce income statement results.
18. Produce cash flow results.
19. Produce balance sheet results.
20. Validate statement reconciliation.
21. Calculate period metrics.
22. Evaluate conditional-plan conditions.
23. Resolve conflicting actions.
24. Schedule valid actions.
25. Record warnings, triggers, and lineage.
26. Carry closing balances into the next period.

Default timing:

Conditions evaluated in one period take effect from the following period unless a later effective delay is configured. Same-period action effects are outside the default because they create circularity risk.

## Financial Statements

### Income Statement

Supports:

- Subscription revenue
- Usage revenue
- One-time revenue
- Service revenue
- Refunds and credits
- Cost of revenue
- Gross profit
- Sales and marketing expense
- Research and development expense
- General and administrative expense
- Depreciation or amortisation expense
- Interest expense
- Tax expense
- Net income

### Cash Flow Statement

Supports:

- Customer collections
- Supplier payments
- Workforce payments
- Other operating payments
- Taxes paid
- Capital expenditure
- Equity contributions
- Debt proceeds
- Debt repayments
- Interest payments
- Net cash movement
- Opening cash
- Closing cash

### Balance Sheet

Assets:

- Cash
- Accounts receivable
- Prepayments, when modelled
- Fixed assets at cost
- Accumulated depreciation

Liabilities:

- Accounts payable
- Deferred revenue
- Debt

Equity:

- Contributed capital
- Retained earnings

## Accounting Equation

```text
Assets = Liabilities + Equity
```

Rules:

1. Every forecast period must reconcile within tolerance.
2. A non-zero unexplained imbalance invalidates the Forecast Run.
3. Imbalance cannot be hidden in a plug account.
4. Cash cannot be silently introduced or removed.

## Revenue Timing

V1 separates:

- Earned revenue
- Billed revenue
- Collected cash
- Accounts receivable
- Deferred revenue

Rules:

1. Revenue is recognised when earned under the recognition policy.
2. Cash collection follows collection policy.
3. Annual billing creates deferred revenue when service remains undelivered.
4. Delayed collection creates accounts receivable.

## Expense Timing

V1 separates:

- Expense recognition
- Supplier payment
- Accounts payable
- Prepayment
- Capital purchase
- Depreciation or amortisation

Rules:

1. Payment delay affects cash and accounts payable.
2. CAPEX affects cash and fixed assets at purchase.
3. Depreciation affects profit but not current-period cash.
4. Expense timing must follow the selected expense policy.

## Calculation Lineage

Lineage must identify:

- Model Version
- Scenario
- Forecast period
- Input assumptions
- Scenario overrides
- Applied policies
- Conditional triggers and actions
- Formula used
- Prior period values
- Warnings or undefined reasons

Required lineage examples:

- MRR by customer cohort and pricing plan
- Segment revenue by segment assumptions and pricing plan
- Closing cash by opening cash, collections, payments, CAPEX, and financing
- CAC by included acquisition expenses and customer denominator
- Runway by monthly closing cash sequence
- Conditional hire by trigger metric and effective period
