# Open Decisions and Deep Dives

This document captures product and domain decisions that required explicit discussion. Accepted decisions should be reflected in the relevant policy document, invariant, or acceptance test.

## OD-001 New-Customer Revenue Timing

Question:

When does a newly acquired paying customer start generating revenue?

Options:

- Acquisition month
- Following month
- Configurable activation delay

Accepted V1 decision:

Acquisition month with explicit activation-delay policy.

## OD-002 Churn Timing

Question:

When does churn reduce customer count and revenue?

Options:

- Beginning of month
- End of month
- Evenly through month

Accepted V1 decision:

End of month for customer quantity, explicitly identified.

## OD-003 Conditional Action Timing

Question:

When does a triggered action affect the forecast?

Options:

- Same period
- Following period
- Configured delay

Accepted V1 decision:

Following period.

## OD-004 Expense Classification Depth

Question:

Which expense dimensions are required in V1 versus optional?

Accepted V1 decision:

Required:

- Accounting treatment
- Business function
- Cost behaviour
- Payment policy

Required when relevant to metrics:

- Economic-purpose allocation
- Metric allocation policy

## OD-005 CAC Policies

Question:

Which CAC policies are required in V1?

Accepted V1 decision:

Must-have:

- Paid CAC
- Blended CAC
- Fully loaded CAC

Should-have:

- Rolling three-month versions

Deferred:

- Campaign cohort CAC
- Multi-touch channel attribution
- Economic CAC including unpaid founder labour

## OD-006 Founder Compensation In CAC

Question:

When should founder compensation be included in CAC?

Accepted V1 decision:

Founder cash compensation may be included only through explicit acquisition allocation. Unpaid founder labour is excluded from V1 cash CAC and may become a later economic-CAC policy.

## OD-007 Cost Of Revenue Policy

Question:

When should support, implementation, infrastructure, or API costs be treated as cost of revenue?

Accepted V1 decision:

Require explicit cost-of-revenue classification and allow allocation for ambiguous costs.

## OD-008 Internally Developed Software CAPEX

Question:

Should product development costs be capitalised?

Accepted V1 decision:

Do not infer. Let founder or adviser choose expense immediately, capitalise with useful life, or mark for professional review.

## OD-009 Readiness Result

Question:

Should readiness produce a score, classification, or findings only?

Accepted V1 decision:

Dimension scores, overall classification, and mandatory explanations. Make clear that readiness is not investment quality.

## OD-010 Funding Target

Question:

Should funding requirement target a date or business milestone?

Accepted V1 decision:

Funding requirement supports both selected period targets and selected business milestone targets.

## OD-011 Simplified Tax

Question:

Should simplified tax be included in controlled release?

Accepted V1 decision:

Optional simplified tax, disabled by default.

## OD-012 Customer Cohorts

Question:

How much cohort detail is required for V1?

Accepted V1 decision:

Track enough cohort detail to support pricing changes for new customers versus existing customers, churn, MRR, and lineage. Defer advanced retention cohort analytics.

## OD-013 Multiple Customer Segments

Question:

Should V1 support multiple customer segments?

Accepted V1 decision:

Multiple customer segments are supported in V1.

## OD-014 Market Ceiling Warning

Question:

Should V1 warn when projected customers exceed estimated market size?

Accepted V1 decision:

Could-have only, because market analysis is outside V1. If included, it should be a manually entered ceiling, not a market-research module.

## OD-015 Rounding And Precision

Question:

How should decimal customers and money rounding be handled?

Accepted V1 decision:

Internal calculations keep decimal precision. Display rounding never feeds subsequent calculations.

## OD-016 Report Disclosures

Question:

Which policy disclosures are required in investor-facing summaries?

Accepted V1 decision:

Always disclose:

- Model Version
- Scenario
- Forecast horizon
- Currency
- Metric policies
- Expense inclusion policies for gross margin and CAC
- Undefined metric reasons
- Statement validity

## OD-017 Actuals And Integrations

Question:

How should V1 prepare for future Stripe, Xero, and QuickBooks actuals?

Accepted V1 decision:

Keep actuals out of V1, but preserve clear boundaries:

- Opening facts
- Assumptions
- Calculated results
- Future verified actuals

Do not mix actuals into assumptions silently.
