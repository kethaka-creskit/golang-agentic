# Clean Architecture

This document defines the architecture rules for implementing V1. Domain behaviour is specified in the domain documents, not in adapters or UI.

## Dependency Rule

Dependencies point inward:

```text
Adapters -> Application Use Cases -> Domain Model
```

The domain model does not depend on:

- UI framework
- Database
- Web framework
- Queue
- File system
- LLM provider
- Payment provider
- Accounting provider
- Presentation format

## Architecture Rings

### Domain

Contains:

- Aggregates
- Entities
- Value objects
- Domain services
- Domain events
- Policy objects
- Domain errors and warnings

Owns:

- Financial formulas
- Invariants
- Expense classification rules
- Metric eligibility rules
- Scenario rules
- Conditional planning rules
- Forecast reconciliation rules

### Application

Contains:

- Use cases
- Ports
- Transaction boundaries
- Authorization checks at application boundary
- Command and query models

Owns:

- Orchestration
- Loading aggregates
- Calling domain services
- Persisting outputs
- Returning results

Does not own:

- Formulas
- Financial policy decisions
- Domain invariants

### Adapters

Contains:

- Persistence implementations
- Web/API controllers
- File exporters
- Report renderers
- Authentication integration
- Future payment/accounting integrations

Does not own:

- Domain logic
- Formula rules
- Forecast order

## Suggested Module Boundaries

```text
src/
  domain/
    startup_catalog/
    venture_model/
    commercial_model/
    cost_capacity/
    financing/
    scenario/
    conditional_planning/
    forecasting/
    viability/
    readiness/
  application/
    use_cases/
    ports/
  adapters/
    persistence/
    api/
    reports/
```

## Domain Services

Expected domain services:

- Forecast Evaluation Service
- Revenue Recognition Service
- Expense Recognition Service
- Metric Calculation Service
- Scenario Resolution Service
- Conditional Plan Evaluation Service
- Conflict Resolution Service
- Runway Calculation Service
- Funding Requirement Service
- Readiness Assessment Service
- Calculation Lineage Service

## Policy Objects

Expected policy objects:

- Revenue Recognition Policy
- Collection Policy
- Supplier Payment Policy
- Expense Classification Policy
- Cost of Revenue Policy
- CAC Policy
- LTV Policy
- Gross Margin Policy
- Runway Policy
- Break-Even Policy
- Funding Requirement Policy
- Conditional Action Timing Policy
- Conflict Resolution Policy
- Readiness Policy

Rules:

1. Policies are explicit and versioned when investor-facing outputs depend on them.
2. Forecast Runs record policy identities.
3. Reports disclose policy identities for material metrics.

## Ports

Expected application ports:

- Startup Repository
- Model Version Repository
- Forecast Run Repository
- Policy Repository
- Report Writer
- Clock
- Id Generator

Future ports, outside V1:

- Accounting Actuals Port
- Payment Actuals Port
- Investor Room Port
- AI Assistant Port

## Prohibited Implementation Patterns

- Formula hidden in controller or presenter
- UI option directly changing forecast output without a use case
- Persistence model treated as domain model when it violates invariants
- Report renderer recalculating metrics
- Undefined metric rendered as zero
- Plug account used to force balance-sheet reconciliation
- CAC calculated from all expenses without a named policy

