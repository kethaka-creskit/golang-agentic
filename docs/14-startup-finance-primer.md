# Startup Finance Primer

This primer explains the minimum finance concepts needed to implement V1. It is not accounting, tax, legal, investment, or fundraising advice.

For authoritative product rules, use [Expense and Allocation Model](05-expense-and-allocation-model.md), [Metric Policies](06-metric-policies.md), and [Forecast Engine](07-forecast-engine.md).

## The Three Statements

V1 produces three linked management-forecast statements:

- Income statement
- Cash flow statement
- Balance sheet

They answer different questions.

| Statement | Main Question | Example |
|---|---|---|
| Income statement | Did the business make a profit in the period? | Revenue minus expenses. |
| Cash flow statement | What happened to cash? | Customer collections minus payments plus financing. |
| Balance sheet | What does the business own and owe at period end? | Cash plus assets equals liabilities plus equity. |

The three statements must reconcile.

## Revenue Is Not Always Cash

Revenue means the business earned value by delivering a product or service.

Cash collection means money arrived in the bank.

They can happen in different periods.

Example:

```text
January:
  Revenue earned: 1000 USD
  Cash collected: 0 USD
  Accounts receivable: 1000 USD

February:
  Revenue earned: 0 USD
  Cash collected: 1000 USD
  Accounts receivable: 0 USD
```

The January income statement shows revenue. The February cash flow shows collection.

## Annual Billing And Deferred Revenue

If a customer pays upfront for future service, not all cash is revenue immediately.

Example:

```text
Customer pays 1200 USD for 12 months of service.
Monthly recognised revenue = 100 USD.
Initial deferred revenue = 1100 USD after the first month is recognised.
```

Deferred revenue is a liability because the startup still owes service.

## OPEX

OPEX is operating expenditure consumed during normal operations.

Examples:

- Salaries
- Advertising
- Software subscriptions
- Rent
- Legal fees
- Accounting

OPEX usually affects profit in the period incurred and cash when paid.

## CAPEX

CAPEX creates or improves an asset expected to provide value over multiple periods.

Examples:

- Equipment
- Purchased hardware
- Office fit-out
- Certain acquired software

CAPEX usually affects:

- Cash when purchased
- Balance sheet as an asset
- Income statement gradually through depreciation or amortisation

Important:

CAPEX is not fully expensed as OPEX in the purchase period.

## Depreciation And Amortisation

Depreciation and amortisation spread an asset's cost across useful life.

Example:

```text
Laptop cost: 1200 USD
Useful life: 12 months
Monthly depreciation: 100 USD
```

Depreciation reduces profit. It does not mean 100 USD leaves the bank each month. The cash left when the asset was purchased.

## Cost Of Revenue

Cost of revenue means costs directly required to deliver the product or service to customers.

For SaaS, examples may include:

- Customer-specific hosting
- Payment processing
- Customer-driven API usage
- Data or licensing costs caused by customers
- Implementation labour, when delivery requires implementation

Cost of revenue affects gross margin.

Do not classify a cost as cost of revenue just because it feels technical. The policy must be explicit.

## Gross Margin

```text
Gross Margin
= (Revenue - Cost of Revenue) / Revenue
```

If revenue is zero, gross margin is undefined.

Gross margin quality depends heavily on cost-of-revenue classification.

## CAC

CAC means customer acquisition cost under a named policy.

Basic shape:

```text
CAC
= Eligible Acquisition Cost / Eligible New Customers
```

CAC does not mean:

```text
All expenses / new customers
```

Marketing is not automatically acquisition. Some marketing is retention, brand, research, or communications.

## LTV

Default recurring-revenue approximation:

```text
LTV
= Monthly ARPA x Gross Margin Percentage / Monthly Customer Churn Rate
```

If churn is zero, the default formula is undefined. Do not show infinity unless a specific policy allows it.

## Burn

Gross burn is usually total cash outflow from operations.

Net burn is operating cash outflows minus operating cash inflows.

Financing inflows should not make a weak operating model look healthy.

## Runway

Runway is the time until projected unrestricted cash becomes negative.

V1 calculates runway from projected monthly closing cash.

Do not use only:

```text
Current cash / current monthly burn
```

That shortcut breaks when revenue, hiring, financing, pricing, or conditional plans change over time.

## Funding Requirement

Funding requirement answers:

How much capital is needed to keep cash above the minimum cash policy through a selected period or milestone?

Example:

```text
Minimum cash policy: 50000 USD
Lowest projected cash before target: -120000 USD
Funding requirement: 170000 USD
```

## Balance Sheet Equation

Every period must satisfy:

```text
Assets = Liabilities + Equity
```

If it does not reconcile, the Forecast Run is invalid.

Do not create a fake balancing account to make the equation pass.

## Undefined Is Not Zero

Some metrics cannot be calculated.

Examples:

- CAC when new customers are zero
- Gross margin when revenue is zero
- LTV when churn is zero under the default formula
- Payback when gross profit per customer is not positive

Undefined metrics must include a reason.

