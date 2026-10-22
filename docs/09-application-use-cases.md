# Application Use Cases

Application use cases orchestrate domain behaviour. They do not contain financial rules. Domain rules live in [Domain Invariants](04-domain-invariants.md), [Expense and Allocation Model](05-expense-and-allocation-model.md), [Metric Policies](06-metric-policies.md), and [Forecast Engine](07-forecast-engine.md).

## Use Case List

| Id | Use Case | Intent | Main Output |
|---|---|---|---|
| UC-001 | Register Startup Boundary | Create or select the startup being modelled. | Startup identity |
| UC-002 | Establish Startup Model | Define model boundary, currency, horizon, and opening facts. | Draft Startup Model |
| UC-003 | Define Commercial Assumptions | Define segments, acquisition, pricing, churn, billing, recognition, and collection. | Commercial assumption set |
| UC-004 | Define Expense Plan | Define expenses, classifications, payment policies, and metric allocations. | Cost and capacity plan |
| UC-005 | Define Workforce Plan | Define roles, compensation, employer on-cost, capacity, and timing. | Workforce plan |
| UC-006 | Define Financing Plan | Define equity cash, grants, debt, repayments, and minimum cash. | Financing plan |
| UC-007 | Select Metric Policies | Choose CAC, LTV, gross margin, burn, runway, and funding policies. | Metric policy set |
| UC-008 | Approve Model Version | Validate and freeze assumptions and policies. | Immutable Model Version |
| UC-009 | Generate Forecast | Produce deterministic monthly results. | Forecast Run |
| UC-010 | Define Derived Scenario | Override selected assumptions from effective periods. | Valid Scenario |
| UC-011 | Compare Scenarios | Compare compatible Forecast Runs. | Scenario Comparison |
| UC-012 | Define Conditional Plan | Connect a condition to a future action. | Conditional Plan |
| UC-013 | Evaluate Viability | Calculate runway, break-even, funding, unit economics, and sensitivity. | Viability Analysis |
| UC-014 | Perform Sensitivity Analysis | Vary assumptions across ranges. | Sensitivity Result |
| UC-015 | Assess Investor Readiness | Evaluate completeness, consistency, scenario coverage, and credibility. | Readiness Assessment |
| UC-016 | Produce Investor Summary | Create explainable investor-facing summary from approved runs. | Investor Summary |
| UC-017 | Explain Forecast Result | Trace a result to inputs, policies, and prior results. | Calculation Lineage |
| UC-018 | Compare Model Versions | Explain changed assumptions, policies, and outcomes. | Version Difference |

## Use Case Principles

1. Use cases coordinate aggregates and domain services.
2. Use cases do not implement formulas.
3. Use cases do not decide expense eligibility.
4. Use cases do not contain UI concepts.
5. Use cases return domain outcomes, validation errors, warnings, and lineage.

## UC-004 Define Expense Plan

This use case is a V1 critical path because expense classification drives statements and metrics.

Inputs:

- Expense name
- Amount rule
- Effective period range
- Accounting treatment
- Business function
- Cost behaviour
- Payment policy
- Economic-purpose allocations
- Metric allocation policies

Validations:

- Non-negative amount
- Valid period range
- Valid variable driver, when applicable
- Economic-purpose allocations not above 100%
- Metric allocations not above allowed purpose allocations
- CAPEX has useful life or amortisation policy
- Payment policy is compatible with accounting treatment

Outputs:

- Accepted cost plan
- Validation errors
- Policy warnings
- Classification lineage

## UC-007 Select Metric Policies

Inputs:

- CAC policy
- LTV policy
- Gross margin policy
- Net burn policy
- Runway policy
- Funding requirement policy

Validations:

- Selected policies are supported in V1.
- Required expense classifications exist for selected policies.
- Undefined states are defined.
- Investor-facing disclosures are configured.

Outputs:

- Metric policy set
- Disclosure requirements
- Validation warnings
