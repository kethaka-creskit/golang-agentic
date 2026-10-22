# TDD and Acceptance Specifications

This document defines how V1 should be tested. It links product behaviour to domain examples and protects the financial engine from regression.

## Test Hierarchy

1. Acceptance specifications: Gherkin examples that describe user-meaningful domain behaviour.
2. Aggregate tests: invariants for Startup Model, Expense Plan, Scenario, Conditional Plan, Forecast Run, and Readiness Assessment.
3. Value-object tests: Money, Percentage, Period, Period Range, Customer Quantity, Metric Identifier, Policy Identifier.
4. Domain-service tests: forecast evaluation, expense recognition, metric calculation, scenario resolution, conditional evaluation, conflict resolution.
5. Property-based tests: invariants across many generated inputs.
6. Golden-model tests: reviewed reference models with expected statement and metric outputs.

## Acceptance Specification Rules

Gherkin should use domain language.

Use:

```gherkin
Given the expense has acquisition allocation of 70 percent
When fully loaded CAC is calculated
Then 70 percent of the expense is included in the CAC numerator
```

Avoid:

```gherkin
When the user clicks the CAC dropdown
```

## Required Feature Files

| Feature | Primary Document |
|---|---|
| Establish startup model | [Product Scope](01-product-scope.md) |
| Approve immutable model version | [Domain Invariants](04-domain-invariants.md) |
| Model recurring revenue | [Forecast Engine](07-forecast-engine.md) |
| Model multiple customer segments | [Forecast Engine](07-forecast-engine.md) |
| Model collection and deferred revenue | [Forecast Engine](07-forecast-engine.md) |
| Classify expenses | [Expense and Allocation Model](05-expense-and-allocation-model.md) |
| Allocate expenses to metrics | [Expense and Allocation Model](05-expense-and-allocation-model.md) |
| Calculate CAC family | [Metric Policies](06-metric-policies.md) |
| Calculate LTV | [Metric Policies](06-metric-policies.md) |
| Calculate runway | [Metric Policies](06-metric-policies.md) |
| Calculate funding requirement | [Metric Policies](06-metric-policies.md) |
| Produce linked statements | [Forecast Engine](07-forecast-engine.md) |
| Create derived scenario | [Scenarios and Conditional Planning](08-scenarios-and-conditional-planning.md) |
| Trigger conditional action | [Scenarios and Conditional Planning](08-scenarios-and-conditional-planning.md) |
| Resolve conflicts | [Scenarios and Conditional Planning](08-scenarios-and-conditional-planning.md) |
| Assess investor readiness | [Product Scope](01-product-scope.md) |
| Explain calculation lineage | [Forecast Engine](07-forecast-engine.md) |

## Example Gherkin: Expense Classification

```gherkin
Feature: Classify expenses across financial and business dimensions
  In order to produce trustworthy statements and metrics
  As a financial model
  I want each expense to declare separate classification dimensions

  Scenario: Classify paid acquisition spend
    Given an expense named "Search ads" is 10000 USD in January 2027
    And its accounting treatment is OPEX
    And its business function is Marketing
    And its economic purpose allocation is Acquisition 100 percent
    When the expense is recognised
    Then operating expense includes 10000 USD
    And acquisition-attributed cost includes 10000 USD

  Scenario: Prevent economic-purpose allocation above one hundred percent
    Given an expense has Acquisition allocation of 70 percent
    And Retention allocation of 40 percent
    When the expense is validated
    Then validation fails
    And the reason identifies economic-purpose allocation above 100 percent

  Scenario: Separate CAPEX from OPEX
    Given equipment costing 12000 USD is purchased in January 2027
    And the accounting treatment is CAPEX
    And the useful life is 36 months
    When January 2027 is forecast
    Then capital expenditure includes 12000 USD
    And fixed assets increase by 12000 USD
    And January OPEX does not include the 12000 USD purchase
```

## Example Gherkin: Customer Segments

```gherkin
Feature: Model multiple customer segments
  In order to represent materially different customer economics
  As a financial model
  I want segment-level assumptions to calculate segment and total results

  Scenario: Calculate revenue by segment and in total
    Given the SMB segment has 100 paying customers at 20 USD per month
    And the Enterprise segment has 10 paying customers at 500 USD per month
    When MRR is calculated
    Then SMB MRR is 2000 USD
    And Enterprise MRR is 5000 USD
    And total MRR is 7000 USD

  Scenario: Prevent a segment override from changing another segment
    Given the SMB segment has monthly churn of 5 percent
    And the Enterprise segment has monthly churn of 2 percent
    When the downside scenario overrides SMB churn to 8 percent
    Then Enterprise churn remains 2 percent
```

## Example Gherkin: CAC Policy

```gherkin
Feature: Calculate CAC using explicit acquisition eligibility policy
  In order to avoid misleading unit economics
  As a financial model
  I want CAC to include only acquisition-attributed costs

  Scenario: Calculate paid CAC
    Given paid acquisition spend is 10000 USD
    And customers acquired through paid channels are 100
    When paid CAC is calculated
    Then paid CAC is 100 USD

  Scenario: Exclude retention campaign from new-customer CAC
    Given a renewal campaign costs 5000 USD
    And its economic purpose allocation is Retention 100 percent
    When blended CAC is calculated
    Then the renewal campaign is excluded from the CAC numerator

  Scenario: Include allocated marketing salary in fully loaded CAC
    Given marketing salary is 8000 USD
    And its economic purpose allocation is Acquisition 70 percent
    And the selected CAC policy includes acquisition payroll
    When fully loaded CAC is calculated
    Then 5600 USD is included in the CAC numerator

  Scenario: Prevent double counting inside CAC
    Given an expense has 10000 USD eligible direct acquisition spend
    And the same 10000 USD is also allocated through campaign spend
    When CAC inputs are validated
    Then validation fails
    And the reason identifies duplicate CAC numerator allocation
```

## Example Gherkin: Conditional Planning

```gherkin
Feature: Trigger conditional business actions
  In order to model what the startup plans to do when conditions change
  As a financial model
  I want conditions to schedule traceable future actions

  Scenario: Hire after customer milestone
    Given active customers are 900 in June 2027
    And active customers are 1000 in July 2027
    And a one-time conditional plan hires a marketer when active customers are at least 1000
    When the forecast is completed
    Then the condition first triggers in July 2027
    And the marketer cost begins in August 2027

  Scenario: Avoid same-period circularity
    Given ARR reaches 100000 USD in October 2027
    And a conditional action increases new-customer price by 10 percent when ARR is at least 100000 USD
    When October 2027 is forecast
    Then the October ARR used for the trigger does not include the triggered price increase
    And the price increase is effective from November 2027
```

## Example Gherkin: Funding Requirement

```gherkin
Feature: Calculate funding requirement against period and milestone targets
  In order to understand how much capital the startup needs
  As a financial model
  I want funding requirement to support both time-based and milestone-based targets

  Scenario: Calculate funding requirement through a selected period
    Given the minimum cash policy is 50000 USD
    And closing cash would fall to negative 150000 USD before December 2027 without new funding
    When funding requirement is calculated through December 2027
    Then the funding requirement is 200000 USD
    And the funding target type is selected period

  Scenario: Calculate funding requirement through a selected milestone
    Given the selected milestone is "MRR reaches 50000 USD"
    And MRR first reaches 50000 USD in October 2027
    And the minimum cash policy is 50000 USD
    And closing cash would fall to negative 120000 USD before October 2027 without new funding
    When funding requirement is calculated for the selected milestone
    Then the funding requirement is 170000 USD
    And the funding target period is October 2027

  Scenario: Report when the selected milestone is not reached
    Given the selected milestone is "MRR reaches 50000 USD"
    And MRR never reaches 50000 USD within the forecast horizon
    When funding requirement is calculated for the selected milestone
    Then the funding requirement uses the full forecast horizon
    And a warning states that the selected milestone is not reached
```

## Property-Based Tests

Property-based tests should verify:

- Percentages outside valid ranges fail validation.
- Economic-purpose allocations cannot exceed 100%.
- Customer quantities never become negative.
- Scenario inheritance cannot cycle.
- Forecast Runs are deterministic for identical inputs.
- Valid Forecast Runs reconcile every period.
- Undefined values are never coerced to zero.
- Display rounding does not affect internal calculations.

## Golden Models

Golden models should include:

1. Simple monthly subscription model.
2. Annual billing with deferred revenue.
3. Delayed collections and accounts receivable.
4. Variable costs and cost of revenue.
5. CAPEX and depreciation.
6. Paid CAC and fully loaded CAC.
7. Conditional hire.
8. Conditional price increase.
9. Financing event and funding requirement.
10. Downside, base, and upside scenario comparison.
11. Multiple customer segments with segment-level pricing and churn.

Each golden model must include:

- Input assumptions
- Selected policies
- Expected monthly statements
- Expected metrics
- Expected warnings
- Expected lineage samples

## Definition Of Done For A Domain Rule

A domain rule is done when:

1. The rule is written in a policy or invariant document.
2. The ubiquitous language is updated if needed.
3. Positive and negative Gherkin scenarios exist.
4. Aggregate or domain-service tests cover edge cases.
5. Undefined states are specified.
6. Report disclosure requirements are known, when investor-facing.
7. The traceability matrix points to the rule.
