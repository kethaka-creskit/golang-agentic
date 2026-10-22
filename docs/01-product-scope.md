# Product Scope

This document defines the V1 product boundary. Domain terms are defined in [Ubiquitous Language](03-ubiquitous-language.md). Detailed financial policies are defined in [Expense and Allocation Model](05-expense-and-allocation-model.md) and [Metric Policies](06-metric-policies.md).

## Product Thesis

V1 helps an early-stage founder answer whether a startup idea can become financially viable before the founder commits major time, money, or hiring capacity.

The product is not a generic spreadsheet replacement. It is a decision model that connects:

- Customer assumptions
- Revenue and pricing assumptions
- Expense and workforce plans
- Financing assumptions
- Scenarios
- Conditional actions
- Runway, break-even, unit economics, and investor readiness

## Primary Customer

The primary customer is a pre-seed or seed-stage founder building a recurring-revenue startup, especially a SaaS or subscription business.

The founder may not have a full-time finance leader and may need to evaluate:

- Whether the idea is viable
- How much cash is required
- When the company runs out of cash
- When to hire
- How pricing affects runway
- What downside and upside cases look like
- What information is credible enough for investor discussion

## Secondary Users

Secondary users may include:

- Fractional CFOs
- Startup accountants
- Advisers
- Accelerator operators
- Finance-literate co-founders

These users may help construct or review models, but V1 domain language remains centred on the startup and its decisions.

## Entity Boundary

A user may manage multiple startups.

Each startup is modelled independently:

- One Startup Model represents exactly one startup.
- A startup may have multiple approved Model Versions.
- Scenarios belong to one Startup Model.
- Forecast Runs belong to one Model Version and one Scenario.
- V1 does not consolidate financial results across startups.
- Assumptions, scenarios, conditional plans, and forecast outputs cannot cross startup boundaries.

User access, collaboration, and permissions are outside the core financial domain, but the application must preserve this startup boundary.

## V1 Must Support

- Monthly forecast horizon from 12 to 60 months
- One reporting currency per model
- Opening financial position
- Multiple customer segments with segment-level acquisition, conversion, churn, retention, and pricing assumptions
- Subscription pricing with effective-dated price changes
- Optional one-time, setup, usage, and service revenue
- Billing, collection, receivables, and deferred revenue
- Fixed, variable, workforce, one-time, and capital costs
- Expense classification and metric allocation policies
- Planned financing without cap-table effects
- Linked income statement, cash flow statement, and balance sheet
- Runway, cash-out, break-even, period-target and milestone-target funding requirement, unit economics, and sensitivity
- Base, downside, upside, and custom scenarios
- Effective-dated scenario overrides
- Conditional hiring, pricing, spending, and financing actions
- Calculation lineage and model versioning
- Investor-readiness assessment and summary

## V1 Must Exclude

- Market research
- Competitor research
- Customer interview management
- Customer validation experiments
- Cap table management
- Valuation
- SAFE or convertible-note conversion
- Liquidation preferences
- Interactive investor deck
- Investor sandbox
- Investor engagement analytics
- Accounting and payment integrations
- Actual-versus-plan reconciliation
- AI-generated assumptions
- AI investor assistant
- Multi-currency
- Multi-entity consolidation
- Statutory accounting
- Tax filing

## Success Measures

Commercial validation hypotheses:

| Measure | Initial Target |
|---|---:|
| Qualified founders who complete a base model | 40% or more |
| Completed models with an alternative scenario | 50% or more |
| Completed models with a conditional plan | 30% or more |
| Founders reporting a material decision was influenced | 60% or more |
| Models regenerated after an assumption change | 40% or more |

Financial integrity requirements:

| Measure | Required Standard |
|---|---:|
| Valid forecasts with unexplained statement imbalance | 0% |
| Material outputs with calculation lineage | 100% |
| Approved Model Versions reproducible from stored inputs and policies | 100% |
| Undefined metrics silently displayed as zero | 0% |

The commercial targets are hypotheses to test in founder validation. They are not universal benchmarks.
