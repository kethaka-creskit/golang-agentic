# Engineer Onboarding Guide

This guide helps an implementation team approach V1 without needing prior startup-finance experience.

The formal source of truth remains the spec pack:

- [Product Scope](01-product-scope.md)
- [Domain Map](02-domain-map.md)
- [Domain Invariants](04-domain-invariants.md)
- [Expense and Allocation Model](05-expense-and-allocation-model.md)
- [Metric Policies](06-metric-policies.md)
- [Forecast Engine](07-forecast-engine.md)
- [TDD and Acceptance Specifications](11-tdd-and-acceptance-specs.md)

Use this guide as a practical companion.

## Mental Model

The product turns assumptions into financial consequences.

```text
Startup
  -> Model Version
  -> Scenario
  -> Monthly Forecast Run
  -> Statements
  -> Metrics
  -> Readiness and reports
```

The founder enters assumptions. The domain model applies policies. The forecast engine produces results. Reports explain the results.

Do not think of V1 as a dashboard first. Think of it as a deterministic calculation engine with a product interface later attached to it.

## What To Read First

1. Read [Product Scope](01-product-scope.md) to understand V1 boundaries.
2. Read [Ubiquitous Language](03-ubiquitous-language.md) so naming is consistent.
3. Read [Startup Finance Primer](14-startup-finance-primer.md) for the minimum finance concepts.
4. Read [Worked Forecast Example](15-worked-forecast-example.md) to see linked statements.
5. Read [Expense and Allocation Model](05-expense-and-allocation-model.md) carefully. This is one of the highest-risk areas.
6. Read [Metric Policies](06-metric-policies.md) before implementing CAC, LTV, runway, or funding requirement.
7. Read [Clean Architecture](10-clean-architecture.md) before choosing module boundaries.

## Core Implementation Sequence

Build in thin, testable slices:

1. Value objects: Money, Percentage, Period, Period Range, Customer Quantity, Currency Code.
2. Startup boundary and Startup Model creation.
3. Model Version approval and immutability.
4. Customer segments and customer movement.
5. Pricing, MRR, ARR, and basic recurring revenue.
6. Expense classification, OPEX, CAPEX, cost of revenue, and payment timing.
7. Basic linked statements with reconciliation.
8. Runway and cash-out.
9. CAC, LTV, gross margin, and payback using explicit policies.
10. Scenarios and effective-dated overrides.
11. Conditional plans and action scheduling.
12. Readiness assessment and calculation lineage.

Do not start with reports. Reports are outputs of the domain model.

## The Most Important Rule

Never hide uncertainty inside code.

If the domain is ambiguous, model it explicitly:

- Create a policy.
- Add an open decision.
- Add a warning.
- Require user or adviser selection.
- Record lineage.

Examples:

- Do not guess whether software development is CAPEX.
- Do not guess whether support is cost of revenue.
- Do not guess whether brand marketing belongs in CAC.
- Do not guess whether a monthly rate is actually annual.

## Common Finance Mistakes

Avoid these mistakes:

- Treating revenue as cash.
- Treating billing as revenue.
- Treating deferred revenue as revenue already earned.
- Treating CAPEX as OPEX.
- Treating depreciation as cash leaving the bank.
- Treating all marketing spend as CAC.
- Treating undefined metrics as zero.
- Calculating runway as current cash divided by current burn.
- Using rounded display values in later calculations.
- Fixing statement imbalance with a plug value.

## Domain Ownership

Financial rules belong in the domain.

Application use cases orchestrate:

- Load model
- Validate command
- Call aggregate or domain service
- Save result
- Return outcome

Adapters translate:

- API request to command
- Database row to persisted representation
- Domain output to report format

Adapters do not calculate financial metrics.

## When To Stop And Clarify

Stop and create an open decision when:

- A calculation can be interpreted in multiple reasonable ways.
- A rule depends on accounting judgement.
- A metric numerator or denominator is unclear.
- Two domain docs appear to conflict.
- A test needs a magic number that cannot be explained.
- A report would present a number without policy disclosure.

## Definition Of Ready For Implementation

A feature is ready when:

1. The domain terms are defined.
2. The aggregate or service ownership is clear.
3. The policy is explicit.
4. Undefined states are known.
5. At least one positive and one negative acceptance example exists.
6. Statement impact is understood.
7. Required lineage is known.

