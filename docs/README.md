# Startup Decision Modelling V1 Spec Pack

This folder turns the original V1 PRD into an interconnected working specification for product, domain modelling, Clean Architecture, and TDD.

The goal is to avoid one oversized PRD becoming the only source of truth. Each document owns a different layer of the specification and links to the documents it depends on.

## Document Map

Read in this order when onboarding:

1. [Product Scope](01-product-scope.md) - what V1 is, who it serves, what it excludes, and how success is measured.
2. [Domain Map](02-domain-map.md) - bounded contexts and how they collaborate.
3. [Ubiquitous Language](03-ubiquitous-language.md) - shared terms used by product, domain, tests, and code.
4. [Domain Invariants](04-domain-invariants.md) - rules that must hold regardless of interface or implementation.
5. [Expense and Allocation Model](05-expense-and-allocation-model.md) - expense classification, OPEX, CAPEX, cost of revenue, and metric allocation.
6. [Metric Policies](06-metric-policies.md) - CAC, LTV, runway, burn, margin, funding requirement, undefined values, and disclosures.
7. [Forecast Engine](07-forecast-engine.md) - monthly simulation order, financial statements, and calculation lineage.
8. [Scenarios and Conditional Planning](08-scenarios-and-conditional-planning.md) - scenario inheritance, overrides, trigger rules, actions, timing, and conflicts.
9. [Application Use Cases](09-application-use-cases.md) - use-case boundaries that orchestrate domain behaviour.
10. [Clean Architecture](10-clean-architecture.md) - dependency rules, ports, adapters, and module ownership.
11. [TDD and Acceptance Specifications](11-tdd-and-acceptance-specs.md) - acceptance features, test hierarchy, golden models, and definition of done.
12. [Open Decisions and Deep Dives](12-open-decisions-and-deep-dives.md) - unresolved questions and areas requiring deeper analysis.
13. [Traceability Matrix](99-traceability-matrix.md) - links goals to documents, use cases, policies, and acceptance tests.

Supplementary implementation guides:

- [Engineer Onboarding Guide](13-engineer-onboarding-guide.md) - how to approach the project if the domain is new.
- [Startup Finance Primer](14-startup-finance-primer.md) - finance concepts required to implement V1 safely.
- [Worked Forecast Example](15-worked-forecast-example.md) - a small reconciled model showing how statements connect.
- [Implementation Guardrails](16-implementation-guardrails.md) - common mistakes and rules of thumb for implementation.
- [Testing Guide](17-testing-guide.md) - practical guidance for domain, service, property, and golden-model tests.

## Source Of Truth Rules

- Product positioning and release boundaries live in [Product Scope](01-product-scope.md).
- Bounded context ownership lives in [Domain Map](02-domain-map.md).
- Shared terminology lives in [Ubiquitous Language](03-ubiquitous-language.md).
- Cross-domain rules live in [Domain Invariants](04-domain-invariants.md).
- Expense classification rules live in [Expense and Allocation Model](05-expense-and-allocation-model.md).
- Formula and metric eligibility rules live in [Metric Policies](06-metric-policies.md).
- Forecast execution order lives in [Forecast Engine](07-forecast-engine.md).
- Scenario and conditional-action semantics live in [Scenarios and Conditional Planning](08-scenarios-and-conditional-planning.md).
- Testable behaviour lives in [TDD and Acceptance Specifications](11-tdd-and-acceptance-specs.md).
- Supplementary finance explanations live in [Startup Finance Primer](14-startup-finance-primer.md) and [Worked Forecast Example](15-worked-forecast-example.md).

If two documents conflict, the more specific policy document wins over the general product document. For example, the CAC rules in [Metric Policies](06-metric-policies.md) override any casual CAC wording elsewhere.

## V1 Definition

V1 is a startup decision-modelling system for early-stage recurring-revenue startups.

It lets a founder define assumptions, produce linked monthly financial statements, compare scenarios, model milestone-based actions, calculate viability metrics, and generate an explainable investor-readiness assessment.

The defining V1 capability is:

> Model not only what the founder expects to happen, but also what the startup plans to do when business conditions change.
