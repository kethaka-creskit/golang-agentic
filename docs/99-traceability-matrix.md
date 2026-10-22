# Traceability Matrix

This matrix links V1 objectives to documents, domain areas, and acceptance coverage.

| Objective | Primary Docs | Domain Areas | Required Acceptance Coverage |
|---|---|---|---|
| Model one startup independently | [Product Scope](01-product-scope.md), [Domain Invariants](04-domain-invariants.md) | Startup Catalog, Venture Model | Establish startup boundary, reject cross-startup scenario inheritance |
| Build recurring-revenue model | [Forecast Engine](07-forecast-engine.md), [Metric Policies](06-metric-policies.md) | Commercial Model, Forecasting | Subscription revenue, MRR, ARR, churn, collection timing |
| Model multiple customer segments | [Product Scope](01-product-scope.md), [Forecast Engine](07-forecast-engine.md) | Commercial Model, Forecasting | Segment pricing, segment churn, segment totals, segment-specific overrides |
| Estimate costs accurately | [Expense and Allocation Model](05-expense-and-allocation-model.md) | Cost and Capacity | Fixed cost, variable cost, workforce cost, payment delay |
| Separate OPEX, CAPEX, and cost of revenue | [Expense and Allocation Model](05-expense-and-allocation-model.md), [Forecast Engine](07-forecast-engine.md) | Cost and Capacity, Forecasting | OPEX recognition, CAPEX purchase, depreciation, gross margin |
| Calculate trustworthy CAC | [Expense and Allocation Model](05-expense-and-allocation-model.md), [Metric Policies](06-metric-policies.md) | Cost and Capacity, Viability | Paid CAC, blended CAC, fully loaded CAC, exclusion of retention spend |
| Produce linked statements | [Forecast Engine](07-forecast-engine.md) | Forecasting | Income statement, cash flow, balance sheet, accounting equation |
| Calculate runway and funding dynamically | [Metric Policies](06-metric-policies.md), [Forecast Engine](07-forecast-engine.md) | Viability, Forecasting | Cash-out period, beyond-horizon runway, post-action recalculation, period-target funding, milestone-target funding |
| Compare alternative futures | [Scenarios and Conditional Planning](08-scenarios-and-conditional-planning.md) | Scenario | Scenario inheritance, overrides, compatible comparison |
| Model milestone-based decisions | [Scenarios and Conditional Planning](08-scenarios-and-conditional-planning.md) | Conditional Planning, Forecasting | Conditional hiring, pricing, spending, financing, conflict resolution |
| Preserve reproducibility | [Domain Invariants](04-domain-invariants.md), [Clean Architecture](10-clean-architecture.md) | Venture Model, Forecasting | Immutable Model Version, deterministic Forecast Run |
| Explain outputs | [Forecast Engine](07-forecast-engine.md), [Metric Policies](06-metric-policies.md) | Forecasting, Viability | Calculation lineage for MRR, cash, CAC, runway, conditional actions |
| Assess investor readiness | [Product Scope](01-product-scope.md), [Metric Policies](06-metric-policies.md) | Investor Readiness | Missing assumptions, statement imbalance, undefined metrics, scenario gaps |
| Keep implementation clean | [Clean Architecture](10-clean-architecture.md) | All contexts | Domain logic outside adapters, policies versioned, report renderer does not recalculate |
| Drive implementation through tests | [TDD and Acceptance Specifications](11-tdd-and-acceptance-specs.md) | All contexts | Gherkin, aggregate tests, service tests, property tests, golden models |

## Coverage Gaps To Close Before Build

1. Turn all expense classification examples into Gherkin scenarios.
2. Define exact CAC policy defaults and report disclosures.
3. Create at least one golden model for fully loaded CAC.
4. Create at least one golden model with CAPEX, depreciation, and cash impact.
5. Create a golden model for period-target and milestone-target funding.
6. Decide whether annual billing and deferred revenue are must-have or should-have.
7. Define readiness scoring policy enough to test it deterministically.
