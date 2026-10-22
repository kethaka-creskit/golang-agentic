# Ubiquitous Language

This document defines shared terms for V1. If a term needs formula rules, see [Metric Policies](06-metric-policies.md). If a term relates to expenses, see [Expense and Allocation Model](05-expense-and-allocation-model.md).

| Term | Definition |
|---|---|
| Startup | The business entity being modelled. |
| Startup Catalog | The application boundary that lets a user manage multiple startups without consolidating them. |
| Startup Model | The aggregate representation of one startup's assumptions, scenarios, conditional plans, and forecast configuration. |
| Model Version | An immutable approved snapshot of model inputs and policies. |
| Forecast Horizon | The ordered monthly periods for which the model calculates results. |
| Opening Fact | A factual starting position, such as opening cash, debt, receivables, or payables. |
| Assumption | A founder-provided input that is not calculated by the model. |
| Policy | A versioned rule that determines how a calculation, recognition, allocation, or timing decision is performed. |
| Scenario | A named view of the model produced by a base assumption set plus optional overrides. |
| Base Scenario | The founder's principal expected case. |
| Derived Scenario | A scenario that inherits assumptions from another scenario and overrides selected assumptions. |
| Scenario Override | A scenario-specific replacement for an inherited assumption from an effective period. |
| Forecast Run | An immutable result produced from one Model Version and one Scenario. |
| Calculation Lineage | The assumptions, policies, formulas, prior results, and events that produced a value. |
| Revenue Stream | A defined mechanism through which the startup earns revenue. |
| Pricing Plan | Commercial terms applied to eligible customers or transactions. |
| Customer Segment | A modelled customer group with its own acquisition, conversion, pricing, churn, retention, and cost assumptions where applicable. |
| Customer Cohort | Customers acquired in the same period and governed by common assumptions. |
| Active Customer | A customer expected to be eligible for service in a period. |
| Paying Customer | An active customer expected to generate revenue in a period. |
| Churn | Expected loss of customers or recurring revenue in a period. |
| Expansion Revenue | Additional revenue from existing customers. |
| Expense | A planned economic sacrifice that may affect financial statements, cash, and metrics according to policies. |
| Accounting Treatment | How an expense affects the financial statements, such as OPEX, CAPEX, cost of revenue, financing, tax, or non-cash expense. |
| Business Function | The team or function that consumes the expense, such as sales, marketing, product, customer success, support, G&A, infrastructure, legal, finance, or people. |
| Economic Purpose | The business purpose served by spending, such as acquisition, retention, expansion, delivery, administration, research, compliance, fundraising, or brand. |
| Economic Purpose Allocation | A percentage allocation of an expense across one or more economic purposes. |
| Cost Behaviour | How a cost changes over time or volume, such as fixed, variable, semi-variable, one-time, recurring, or step cost. |
| Metric Allocation Policy | A policy that determines which portion of an expense is eligible for a metric. |
| Cost of Revenue | Costs directly required to deliver the product or service to customers under the selected policy. |
| OPEX | Operating expenditure recognised in the income statement as operating expense. |
| CAPEX | Capital expenditure recognised as an asset and expensed over time through depreciation or amortisation. |
| Workforce Position | A planned role with compensation, employer on-costs, capacity, and timing. |
| Financing Event | A planned cash movement related to equity contribution, debt, grant, or repayment. |
| Conditional Plan | A policy that activates an action when a condition becomes true in a forecast. |
| Condition | A Boolean expression evaluated against forecast state. |
| Action | A future model change scheduled when a condition is satisfied. |
| Trigger Period | The first period in which a condition is satisfied. |
| Effective Period | The period in which a triggered action begins affecting the forecast. |
| Runway | Time until projected unrestricted cash becomes negative. |
| Cash-Out Period | The first period ending with negative unrestricted cash. |
| Accounting Break-Even | The first period satisfying the selected operating-profit persistence policy. |
| Cash Break-Even | The first period satisfying the selected operating-cash-flow persistence policy. |
| CAC | Customer acquisition cost under a named CAC policy. |
| Paid CAC | CAC based on paid acquisition spend and customers acquired through paid channels. |
| Sales and Marketing CAC | CAC based on eligible sales and marketing acquisition cost. |
| Blended CAC | CAC based on all eligible acquisition costs across channels, not all company expenses. |
| Fully Loaded CAC | CAC including direct acquisition spend and configured allocated acquisition payroll, software, and overheads. |
| LTV | Customer lifetime value under a named LTV policy. |
| CAC Payback | Months required for gross profit from a new customer to recover CAC. |
| Funding Requirement | Capital required to maintain a minimum cash policy through a selected period or selected business milestone. |
| Funding Target | The selected period or business milestone used to calculate required capital. |
| Readiness Assessment | Evaluation of completeness, consistency, viability, explainability, and investor-facing quality. |
| Warning | A material concern that does not invalidate a forecast. |
| Validation Error | A condition that prevents a valid forecast. |
| Undefined Metric | A metric that cannot be calculated under the selected policy and must not be displayed as zero. |
