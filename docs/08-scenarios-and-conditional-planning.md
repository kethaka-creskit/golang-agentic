# Scenarios and Conditional Planning

This document specifies alternative futures and milestone-based actions. Forecast execution is defined in [Forecast Engine](07-forecast-engine.md).

## Scenario Model

Scenario types:

- Base
- Downside
- Upside
- Custom

Rules:

1. Every Startup Model has exactly one Base Scenario.
2. A Derived Scenario has exactly one parent.
3. Scenario inheritance cannot cycle.
4. Scenario overrides are effective-dated.
5. A Derived Scenario changes only explicit overrides.
6. Scenario comparison requires compatible Startup Model, currency, horizon, and policy set.
7. Forecast Runs for different scenarios are independent outputs.

## Scenario Override

An override identifies:

- Assumption id
- Replacement value
- Effective period
- Optional end period
- Scenario id
- Rationale or note

Examples:

- Monthly churn increases from 4% to 8%.
- Paid conversion falls from 8% to 4%.
- Subscription price increases from 20 USD to 25 USD for new customers.
- Marketing spend starts three months later.

## Conditional Plan Structure

```text
WHEN [condition]
THEN [action]
STARTING [timing]
WITH [repeatability and priority]
```

Examples:

```text
WHEN active customers are at least 1000
THEN add marketer at 1000 USD per month
STARTING next month
```

```text
WHEN ARR is at least 100000 USD
THEN increase subscription price by 10% for new customers
STARTING next month
```

## Conditions

V1 conditions may reference:

- Active customers
- Paying customers
- MRR
- ARR
- Revenue
- Gross margin
- CAC
- Churn
- Closing cash
- Runway
- Operating profit
- Calendar period

Supported comparisons:

- Greater than
- At least
- Less than
- At most
- Equal
- Within range

Rules:

1. A condition must be evaluable from available metrics.
2. Undefined metrics make the condition unevaluable.
3. An unevaluable condition creates a finding and does not silently become false.
4. V1 supports conjunction across conditions.
5. Disjunction is an open decision unless prioritised.

## Actions

V1 actions may:

- Add a workforce position
- Change workforce quantity
- Add a recurring cost
- Change a recurring cost
- Change a pricing plan
- Change marketing spend
- Schedule a financing event
- Suspend a discretionary cost

Rules:

1. An action affects forecasted scenario state, not canonical approved assumptions.
2. An action cannot affect periods before its effective period.
3. An action must record trigger and effective periods.
4. An action must record lineage.

## Trigger Timing

Default V1 policy:

- Evaluate conditions after period metrics are calculated.
- Apply triggered actions from the following period.

Reason:

This avoids same-period circularity, such as increasing price in the same month that ARR triggered the price change.

## Repeatability

Supported V1 trigger policies:

- One-time: triggers at most once per Forecast Run
- Recurring: may trigger repeatedly according to a recurrence and reset policy

Recommended V1 release:

- One-time plans as must-have
- Recurring plans as could-have

## Conflict Resolution

Conflicts occur when two actions affect the same target in an incompatible way.

Rules:

1. Higher priority wins when actions are compatible with priority resolution.
2. Equal-priority incompatible actions fail validation.
3. Conflict results must be recorded in lineage.
4. Conflict resolution cannot silently drop an action.

## Required Acceptance Coverage

The acceptance suite must cover:

- Scenario inheritance
- Effective-dated overrides
- Non-overridden assumptions staying inherited
- Cyclic inheritance rejection
- Scenario comparison compatibility
- Conditional hiring
- Conditional pricing
- Conditional spending reduction
- Conditional financing schedule
- Same-period circularity prevention
- Unevaluable condition finding
- Conflict priority
- Equal-priority incompatible action failure

