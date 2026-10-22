# Testing Guide

This guide explains how to turn the V1 specs into a practical test suite.

The formal testing strategy lives in [TDD and Acceptance Specifications](11-tdd-and-acceptance-specs.md).

## Test From The Domain Inward

Start with pure domain tests before application or adapter tests.

Preferred order:

1. Value objects
2. Aggregates
3. Domain services
4. Application use cases
5. Adapters
6. Reports

The forecast engine should be testable without a database, API, browser, or report renderer.

## Value Object Tests

Cover:

- Money amount and currency
- Currency mismatch rejection
- Percentage valid ranges
- Monthly versus annual rate distinction
- Period ordering
- Period range boundaries
- Decimal customer quantities
- Policy identifiers

Example cases:

```text
Percentage accepts 0 percent.
Percentage accepts 100 percent.
Percentage rejects 120 percent where a bounded percentage is required.
Annual rate is not silently treated as monthly rate.
```

## Aggregate Tests

Startup Model:

- Creates exactly one Base Scenario.
- Belongs to exactly one startup.
- Cannot approve while validation errors exist.
- Approved Model Version is immutable.

Expense Plan:

- Requires accounting treatment.
- Requires business function.
- Rejects allocation above 100%.
- Rejects variable cost without driver.
- Separates CAPEX from OPEX.

Scenario:

- Inherits from exactly one parent when derived.
- Rejects cyclic inheritance.
- Applies overrides only to selected assumptions.

Conditional Plan:

- Rejects unavailable metrics.
- Records trigger and effective periods.
- Rejects equal-priority incompatible actions.

Forecast Run:

- Is immutable.
- Reconciles every period.
- Records lineage.

## Domain Service Tests

Revenue Recognition Service:

- Monthly subscription revenue.
- Annual billing and deferred revenue.
- Delayed collections and accounts receivable.
- Price change for new customers only.

Expense Recognition Service:

- OPEX recognition.
- CAPEX purchase and depreciation.
- Cost of revenue classification.
- Supplier payment delay and accounts payable.

Metric Calculation Service:

- Paid CAC.
- Blended CAC.
- Fully loaded CAC.
- LTV undefined when churn is zero.
- Runway beyond horizon.
- Funding by period target.
- Funding by milestone target.

Scenario Resolution Service:

- Base assumptions.
- Segment-specific overrides.
- Effective-dated overrides.
- Inherited assumptions.

Conditional Plan Evaluation Service:

- Hiring trigger.
- Pricing trigger.
- Spending suspension.
- Financing action.
- Same-period circularity prevention.

## Property-Based Tests

Use property tests for rules that must hold across many inputs.

Examples:

- Valid Forecast Runs always reconcile.
- Economic-purpose allocations never exceed 100%.
- Customer quantities never become negative.
- Scenario inheritance never cycles.
- Display rounding never changes internal results.
- Undefined values are never converted to zero.
- Equal deterministic inputs produce equal Forecast Runs.

## Golden Models

Golden models are reviewed reference examples with known outputs.

Each golden model should include:

- Input assumptions
- Selected policies
- Expected monthly statements
- Expected metrics
- Expected warnings
- Expected lineage examples

Required golden models:

1. Simple monthly subscription model.
2. Annual billing with deferred revenue.
3. Delayed collections with accounts receivable.
4. Supplier payment delay with accounts payable.
5. Variable cost and cost of revenue.
6. CAPEX and depreciation.
7. Paid CAC and fully loaded CAC.
8. Multiple customer segments.
9. Conditional hiring.
10. Conditional price change.
11. Funding requirement by selected period.
12. Funding requirement by selected milestone.
13. Scenario comparison.

## Report Tests

Reports should be tested as renderers of domain output.

They should verify:

- Metric policy names are shown.
- Undefined reasons are shown.
- Warnings are shown.
- Invalid forecasts are not presented as ready.
- Values match persisted Forecast Run outputs.
- The report does not recalculate formulas.

## Testing Smells

These are warning signs:

- A test asserts a number but cannot explain where it came from.
- A test passes only because of rounded values.
- A metric test does not specify policy.
- A CAC test does not specify eligible expenses.
- A report test calculates a fresh metric.
- A Forecast Run test ignores statement reconciliation.
- A scenario test mutates the base model.
- A conditional action has no lineage assertion.

## Done Means Explained

A test is stronger when it proves both:

- The number is correct.
- The reason for the number is traceable.

For example, a CAC test should assert:

- CAC value
- Included expenses
- Excluded expenses
- Customer denominator
- Policy used
- Undefined reason, when applicable

