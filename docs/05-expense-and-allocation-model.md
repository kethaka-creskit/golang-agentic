# Expense and Allocation Model

This document is a deep specification for costs, OPEX, CAPEX, cost of revenue, business function, economic purpose, payment timing, and metric eligibility.

Metric formulas are defined in [Metric Policies](06-metric-policies.md). Forecast execution is defined in [Forecast Engine](07-forecast-engine.md).

## Why This Exists

An expense cannot be classified using one flat category such as "marketing" or "OPEX".

Those words answer different questions:

- OPEX answers how the expense affects financial statements.
- Marketing answers which function consumed the spend.
- Acquisition answers which economic purpose the spend served.
- CAC eligibility answers whether the expense belongs in a customer-acquisition metric.

V1 must model these dimensions separately.

## Expense Conceptual Shape

```text
Expense
  id
  name
  amount_rule
  effective_period_range
  recurrence_policy
  accounting_treatment
  business_function
  cost_behaviour
  payment_policy
  economic_purpose_allocations
  metric_allocation_policies
  scenario_applicability
  lineage
```

## Accounting Treatment

Accounting treatment determines how the expense affects financial statements.

Supported V1 treatments:

| Treatment | Statement Effect |
|---|---|
| OPEX | Recognised as operating expense in the income statement. |
| Cost of revenue | Recognised above gross profit and affects gross margin. |
| CAPEX | Recognised as an asset when purchased and expensed over time through depreciation or amortisation. |
| Financing cost | Recognised as interest or financing-related expense. |
| Tax | Recognised according to the simplified tax policy. |
| Non-cash expense | Affects profit but not current-period operating cash. |

Rules:

1. Every expense has exactly one primary accounting treatment.
2. CAPEX cannot be fully expensed as OPEX in the same purchase period.
3. Depreciation and amortisation are non-cash expenses.
4. Whether internally developed software is CAPEX is not inferred automatically in V1.
5. The founder or adviser must explicitly choose expense immediately, capitalise, or mark for professional review.

## Business Function

Business function identifies which part of the startup consumes the cost.

Supported V1 functions:

- Product and engineering
- Sales
- Marketing
- Customer success
- Customer support
- General and administrative
- Finance
- Legal
- People and recruitment
- Operations
- Infrastructure
- Research

Rules:

1. Every expense has one primary business function.
2. Business function does not determine accounting treatment by itself.
3. Business function does not determine CAC eligibility by itself.

Example:

| Expense | Business Function | Accounting Treatment |
|---|---|---|
| Google Ads | Marketing | OPEX |
| Marketing salary | Marketing | OPEX |
| Paid acquisition provider | Marketing | OPEX |
| Development laptop | Product and engineering | CAPEX |
| Customer-specific cloud usage | Infrastructure | Cost of revenue |
| Annual audit | Finance | OPEX |

## Economic Purpose

Economic purpose identifies why the expense exists.

Supported V1 purposes:

- Acquisition
- Retention
- Expansion
- Delivery
- Brand
- Product development
- Administration
- Research
- Compliance
- Fundraising

An expense may serve several purposes.

Example:

```text
Marketing employee salary:
  Acquisition: 70%
  Brand: 20%
  Retention: 10%
```

Rules:

1. Economic-purpose allocations may total less than 100%.
2. Economic-purpose allocations cannot exceed 100%.
3. Unallocated portions remain company expenses but do not enter purpose-specific metrics.
4. Economic-purpose allocation is required when an expense may affect CAC, retention cost, expansion cost, or cost of revenue.

## Cost Behaviour

Cost behaviour determines how the expense changes.

Supported V1 behaviours:

| Behaviour | Description |
|---|---|
| Fixed | Amount does not vary with volume unless changed by policy. |
| Variable | Amount is driven by a customer, revenue, transaction, usage, or other driver. |
| Semi-variable | Includes fixed and variable components. |
| One-time | Occurs once in a defined period. |
| Recurring | Repeats across a period range. |
| Step cost | Changes when a threshold is reached. |

Rules:

1. Variable expenses require a valid driver.
2. Step costs require a threshold and step amount or capacity rule.
3. One-time expenses cannot recur unless modelled as separate expenses.
4. Recurring expenses require start period and optional end period.

## Payment Policy

Payment policy distinguishes recognition from cash movement.

Supported V1 payment policies:

- Paid immediately
- Paid after a delay
- Paid in advance
- Accrued and unpaid
- Depreciated or amortised

Rules:

1. Payment timing affects cash and payable balances.
2. Payment timing does not change expense recognition unless the accounting treatment requires it.
3. Accrued unpaid expenses increase accounts payable.
4. Prepaid expenses may be deferred if V1 enables prepayments for the cost type.

## Cost Of Revenue Policy

Cost of revenue includes costs directly required to deliver the product or service to customers under the selected policy.

Usually included for SaaS:

- Customer-specific cloud usage
- Payment processing fees
- Third-party API usage caused by customers
- Customer-specific data or licensing costs
- Implementation labour when delivery requires implementation
- Support labour if support is part of service delivery under the selected policy

Usually excluded:

- Product development
- General marketing
- Fundraising
- General administration
- Finance and legal
- Recruiting
- Broad brand activity

Rules:

1. Cost of revenue classification must be explicit.
2. Ambiguous costs may use a delivery allocation percentage.
3. Gross margin must disclose the cost-of-revenue policy.
4. The same cost portion cannot be counted both as cost of revenue and another metric numerator unless that metric explicitly allows the same classified amount.

## Metric Allocation

Metric allocation determines whether an expense belongs to a metric.

Supported allocation types:

- Direct allocation
- Partial allocation
- Driver-based allocation
- Excluded

Examples:

| Expense | Acquisition Allocation | CAC Treatment |
|---|---:|---|
| Search ads targeting prospects | 100% | Included in paid CAC and blended CAC. |
| Marketing salary | 70% | Included in fully loaded CAC if policy includes acquisition payroll. |
| CRM subscription | 80% | Included in fully loaded CAC if policy includes acquisition software. |
| Renewal campaign | 0% | Excluded from new-customer CAC. |
| Customer support salary | 0% acquisition | Excluded from new-customer CAC unless policy explicitly allocates support to acquisition. |
| Annual audit | 0% | Excluded from CAC. |

Rules:

1. Metric allocations are policy-driven.
2. A metric allocation must state allocation basis.
3. Allocation basis may be direct, employee time, revenue, customer count, transaction count, usage, campaign spend, or headcount.
4. A metric must disclose whether it is direct, allocated, or fully loaded.
5. The same amount cannot be counted twice in one metric.

## Expense Classification Defaults

V1 may provide defaults to help founders, but defaults must be visible and overridable.

| Expense Type | Default Treatment | Default Function | Default Purpose |
|---|---|---|---|
| Paid ads | OPEX | Marketing | Acquisition |
| Sales commission for new customer | OPEX | Sales | Acquisition |
| Renewal commission | OPEX | Sales | Retention |
| Cloud usage per active customer | Cost of revenue | Infrastructure | Delivery |
| Product developer salary | OPEX | Product and engineering | Product development |
| Development laptop | CAPEX | Product and engineering | Product development |
| Accounting fee | OPEX | Finance | Administration |
| Fundraising legal fee | OPEX | Legal | Fundraising |

Defaults are not accounting advice.

## Required Gherkin Coverage

The acceptance suite must cover:

- OPEX recognition
- CAPEX purchase and depreciation
- Cost of revenue classification
- Variable cost driver validation
- Payment delay and accounts payable
- Partial economic-purpose allocation
- CAC allocation eligibility
- Exclusion of retention expenses from new-customer CAC
- Prevention of double counting
- Ambiguous cost marked for policy review
