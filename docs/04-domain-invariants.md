# Domain Invariants

This document defines rules that must hold across V1. Context ownership is defined in [Domain Map](02-domain-map.md). Acceptance examples live in [TDD and Acceptance Specifications](11-tdd-and-acceptance-specs.md).

## Startup Boundary

1. A user may manage multiple startups.
2. One Startup Model belongs to exactly one startup.
3. A Forecast Run belongs to exactly one Startup Model, one Model Version, and one Scenario.
4. V1 does not consolidate across startups.
5. A scenario cannot inherit from a scenario in another Startup Model.
6. A conditional action cannot modify another startup.

## Model Versioning

1. A draft may change until approved.
2. Approval creates an immutable Model Version.
3. An approved Model Version cannot be modified in place.
4. Equal deterministic inputs and policies produce equal Forecast Runs.
5. Opening facts cannot be changed by future conditional actions.
6. Model comparison must identify changed assumptions and policies.

## Assumptions

1. Every assumption has identity, value, unit, effective period, and owning context.
2. Every rate declares whether it is monthly, annual, or period-specific.
3. A calculated value cannot be manually overwritten as an assumption.
4. Assumption changes take effect from explicit periods.
5. Undefined values are not zero.

## Scenarios

1. Every Startup Model has exactly one Base Scenario.
2. A Derived Scenario has exactly one parent.
3. Scenario inheritance cannot cycle.
4. Scenario overrides are effective-dated.
5. Non-overridden assumptions remain inherited.
6. Scenario comparisons require compatible model identity, currency, and horizon.

## Revenue

1. Revenue is not recognised before earned.
2. Billing, recognition, and cash collection are separate concepts.
3. Deferred revenue is recognised over the service period.
4. Accounts receivable is collected according to the collection policy.
5. Price changes declare whether they apply to new customers, existing customers, or all eligible customers.
6. Price changes do not alter prior recognised revenue.
7. Every model has at least one customer segment.
8. Segment-level assumptions cannot silently override another segment.
9. Cross-segment totals must reconcile to the sum of included segment results.

## Expenses

1. Every expense has a non-negative amount rule.
2. Every expense has exactly one accounting treatment for statement recognition.
3. Every expense has one primary business function.
4. Every expense has one cost behaviour.
5. Every expense has explicit cash timing.
6. An expense may have economic-purpose allocations.
7. Economic-purpose allocations cannot exceed 100%.
8. Metric allocations cannot cause double counting within one metric.
9. CAPEX cannot also be fully recognised as OPEX in the purchase period.
10. Depreciation and amortisation must not be counted as both original purchase cash and current-period cash outflow.

Detailed expense rules live in [Expense and Allocation Model](05-expense-and-allocation-model.md).

## Metrics

1. Every material metric is calculated using a named, versioned metric policy.
2. A metric policy defines numerator, denominator, eligibility, timing, and undefined states.
3. CAC includes only acquisition-attributed costs under the selected CAC policy.
4. Blended CAC does not mean all company expenses.
5. Runway uses projected closing unrestricted cash, not current cash divided by current burn.
6. Every investor-facing metric discloses the policy used.

Detailed metric rules live in [Metric Policies](06-metric-policies.md).

## Conditional Plans

1. A condition must reference metrics available at evaluation time.
2. A one-time plan triggers at most once per Forecast Run.
3. A recurring plan defines recurrence and reset behaviour.
4. An action cannot take effect before its trigger period.
5. Equal-priority incompatible actions fail validation.
6. Trigger and action lineage are recorded.
7. Same-period circularity is prohibited by default.

Detailed conditional rules live in [Scenarios and Conditional Planning](08-scenarios-and-conditional-planning.md).

## Forecasting

1. Forecasting is deterministic.
2. Every period carries prior closing balances forward.
3. Every period must reconcile the accounting equation.
4. Cash is never silently introduced or removed.
5. A statement imbalance invalidates the Forecast Run.
6. An invalid Forecast Run cannot be investor-ready.
7. Display rounding never feeds calculations.
8. Every material result has calculation lineage.

Detailed forecast rules live in [Forecast Engine](07-forecast-engine.md).

## Readiness

1. Readiness is not investment quality.
2. A complete model may still describe a weak startup.
3. Missing downside analysis reduces readiness.
4. Missing material assumptions create findings.
5. Unsupported material assumptions create findings.
6. Statement imbalance caps readiness at not ready.
7. Readiness cannot include valuation or investment advice in V1.
