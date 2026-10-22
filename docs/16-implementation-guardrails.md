# Implementation Guardrails

This document lists implementation rules that protect financial correctness, explainability, and Clean Architecture boundaries.

## Guardrail 1: Domain Logic Belongs In The Domain

Do not calculate financial results in:

- Controllers
- API handlers
- UI components
- Report renderers
- Database queries

Those layers may call use cases or read persisted results. They must not own formulas.

## Guardrail 2: Every Formula Needs A Policy

If a formula depends on interpretation, create or use a policy object.

Examples:

- CAC Policy
- Gross Margin Policy
- Cost of Revenue Policy
- Revenue Recognition Policy
- Collection Policy
- Supplier Payment Policy
- Runway Policy
- Funding Requirement Policy

Do not hard-code hidden assumptions.

## Guardrail 3: Undefined Is A First-Class Result

Never coerce undefined to zero.

Examples:

- CAC is undefined when new customers are zero.
- Gross margin is undefined when revenue is zero.
- LTV is undefined when churn is zero under the default formula.
- CAC payback is undefined when gross profit per new customer is not positive.

Return:

```text
value: undefined
reason: "new paying customers are zero"
```

## Guardrail 4: Reconciliation Is Non-Negotiable

Every valid Forecast Run must satisfy:

```text
Assets = Liabilities + Equity
```

Do not add a plug account. If the model does not reconcile, the Forecast Run is invalid.

## Guardrail 5: Cash And Profit Are Different

Profit can be positive while cash declines.

Cash can increase while revenue is not fully earned.

Examples:

- Annual billing creates cash and deferred revenue.
- CAPEX uses cash but does not fully reduce profit immediately.
- Depreciation reduces profit but does not use current-period cash.

## Guardrail 6: Expense Classification Is Multidimensional

Do not create one `category` field and expect it to solve all cost logic.

An expense needs separate dimensions:

- Accounting treatment
- Business function
- Cost behaviour
- Payment policy
- Economic-purpose allocation, when relevant
- Metric allocation policy, when relevant

## Guardrail 7: CAC Must Be Policy-Driven

Never calculate CAC as:

```text
all_expenses / new_customers
```

CAC must use:

- A named CAC policy
- Eligible acquisition costs
- Eligible new customer denominator
- Timing policy
- Disclosure of inclusions and exclusions

## Guardrail 8: Scenario Overrides Must Be Isolated

A scenario override affects only:

- The selected scenario
- Its descendants, if applicable
- The target assumption and effective period

It must not mutate the Base Scenario or approved Model Version.

## Guardrail 9: Conditional Actions Affect Forecast State

A conditional action should not rewrite approved assumptions.

It schedules an effect inside a Forecast Run.

Example:

```text
Trigger: ARR reaches 100000 USD in October
Action: Increase price for new customers
Effective: November
```

Lineage must identify the trigger and action.

## Guardrail 10: Display Rounding Is Not Calculation Rounding

Internal calculations may use decimal customer quantities and precise money values.

Display may round values, but rounded display values must never feed later calculations.

## Guardrail 11: Keep Reports Boring

Reports should present stored domain outputs and lineage.

Reports should not:

- Recalculate metrics
- Change policy interpretation
- Hide warnings
- Hide undefined metrics
- Present invalid forecasts as ready

## Guardrail 12: Ambiguity Becomes An Open Decision

If the correct treatment is unclear, do not guess.

Create an open decision or warning for topics such as:

- Internally developed software capitalisation
- Support as cost of revenue
- Brand spend in CAC
- Founder salary in CAC
- Multi-segment CAC denominator
- Milestone-target funding semantics

## Pull Request Checklist

Before merging a domain change, confirm:

- The relevant spec document is updated.
- Positive and negative tests exist.
- Undefined cases are covered.
- Calculation lineage is preserved.
- Forecast reconciliation still passes.
- No formulas were added to adapters.
- No rounded display values feed calculations.
- Metrics disclose selected policies.
- Golden model impact is reviewed when statements or metrics change.

