# Metric Policies

This document defines metric formulas, eligibility policies, undefined states, and required disclosures.

Expense eligibility depends on [Expense and Allocation Model](05-expense-and-allocation-model.md). Forecast values come from [Forecast Engine](07-forecast-engine.md).

## Policy Object

Every investor-facing metric must be calculated under a named, versioned policy.

```text
MetricPolicy
  id
  name
  version
  metric
  numerator_definition
  denominator_definition
  eligibility_rules
  timing_rules
  undefined_rules
  disclosure_rules
```

Rules:

1. A Forecast Run records the metric policies used.
2. A Model Version records selected default metric policies.
3. A report must disclose metric policy names.
4. A changed policy creates a model change even if assumptions are unchanged.

## Customer Movement

```text
Ending Paying Customers
= Beginning Paying Customers
+ New Paying Customers
- Churned Paying Customers
```

Rules:

1. Expected customer quantities may be decimal internally.
2. Display rounding cannot feed calculations.
3. Customer quantities cannot be negative.

## MRR

```text
MRR
= Sum of recurring monthly charges for eligible paying customers
```

MRR is a run-rate measure. It is not the same as recognised revenue.

## ARR

```text
ARR = Ending MRR x 12
```

ARR is a run-rate measure. It is not recognised annual revenue.

## Gross Margin

```text
Gross Margin
= (Revenue - Cost of Revenue) / Revenue
```

Undefined when revenue is zero.

Required disclosure:

- Cost-of-revenue policy
- Included expense classes
- Excluded expense classes
- Period measured

## CAC Family

CAC never means all company expenses divided by new customers.

All CAC policies follow:

```text
CAC
= Eligible Acquisition Cost / Eligible New Customers
```

The policy defines "eligible".

### Paid CAC

```text
Paid CAC
= Paid Acquisition Spend / Customers Acquired Through Paid Channels
```

Usually includes:

- Paid ads
- Paid partnerships
- Affiliate fees
- Paid campaign vendor fees

Usually excludes:

- Sales salaries
- General marketing payroll
- Organic brand activity
- Retention campaigns
- Customer support
- Product development

### Sales And Marketing CAC

```text
Sales and Marketing CAC
= Eligible Sales and Marketing Acquisition Cost / Total New Paying Customers
```

May include:

- Paid acquisition spend
- Sales compensation allocated to new-customer acquisition
- Marketing compensation allocated to new-customer acquisition
- New-business commissions
- Acquisition software
- Acquisition agencies
- Acquisition events

Does not automatically include every sales and marketing cost.

### Blended CAC

```text
Blended CAC
= All Eligible Acquisition Costs Across Channels / All New Paying Customers
```

Blended CAC combines acquisition costs across channels. It does not include all company expenses.

### Fully Loaded CAC

```text
Fully Loaded CAC
= ( Direct Acquisition Spend
+ Allocated Acquisition Payroll
+ Allocated Acquisition Software
+ Allocated Acquisition Overheads )
/ New Paying Customers
```

Rules:

1. Fully loaded CAC must disclose allocation basis.
2. Founder salary is included only to the configured acquisition allocation.
3. Unpaid founder labour is excluded from V1 cash CAC unless an economic CAC policy is added later.
4. Retention and expansion spend are excluded from new-customer CAC unless a policy explicitly defines a separate expansion CAC.
5. The same expense portion cannot be counted twice in one CAC numerator.

### CAC Timing

Supported V1 timing policies:

- Same-period CAC
- Rolling three-month CAC
- Optional acquisition-delay CAC

Deferred from V1 unless specifically prioritised:

- Campaign cohort CAC
- Channel-specific multi-touch attribution
- Economic CAC including imputed unpaid founder labour

## LTV

Default recurring-revenue approximation:

```text
LTV
= Monthly ARPA x Gross Margin Percentage / Monthly Customer Churn Rate
```

Undefined when:

- Churn is zero under the default formula
- Gross margin is undefined
- Monthly ARPA is unavailable

Required disclosure:

- ARPA policy
- Gross margin policy
- Churn policy
- Whether expansion revenue is included

## CAC Payback

```text
CAC Payback Months
= CAC / Monthly Gross Profit Per New Customer
```

Undefined when:

- CAC is undefined
- Monthly gross profit per new customer is not positive

Required disclosure:

- CAC policy
- Gross profit policy
- Customer denominator

## Net Burn

```text
Net Burn
= Operating Cash Outflows - Operating Cash Inflows
```

A negative result indicates operating cash generation.

Rules:

1. Net burn excludes financing inflows by default.
2. Net burn must disclose whether unusual one-time operating payments are included.

## Runway

```text
Cash-Out Period
= First period in which closing unrestricted cash is below zero
```

Runway is not:

```text
Current Cash / Current Monthly Burn
```

Rules:

1. Runway uses projected monthly closing unrestricted cash.
2. Runway is recalculated for each scenario.
3. Runway is recalculated after conditional actions.
4. If no cash-out occurs within the horizon, the result is "beyond horizon", not infinite.
5. Reports must disclose the horizon end.

## Funding Requirement

Default policy:

```text
Funding Requirement
= Capital required to keep closing cash at or above the minimum cash policy through the selected funding target
```

Supported V1 target types:

- Forecast horizon
- Selected period
- Selected business milestone

Rules:

1. A period target calculates capital required through the selected period.
2. A milestone target calculates capital required through the first period in which the selected milestone is reached.
3. If the selected milestone is not reached within the horizon, the funding requirement uses the full forecast horizon and reports that the milestone was not reached.
4. The report must disclose whether the target was a period or milestone.

## Undefined Values

Rules:

1. Undefined is not zero.
2. Undefined metrics must include a reason.
3. Reports must not hide undefined metrics that materially affect readiness.
4. Readiness may be capped when core metrics are undefined.

## Required Metric Disclosures

Every investor-facing metric must disclose:

- Metric policy
- Numerator definition
- Denominator definition
- Included expense classes, when applicable
- Excluded expense classes, when applicable
- Allocation percentages, when applicable
- Measurement period
- Scenario
- Model Version
- Undefined reason, when applicable
