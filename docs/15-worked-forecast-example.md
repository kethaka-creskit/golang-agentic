# Worked Forecast Example

This document shows a small financial model that reconciles across the income statement, cash flow statement, and balance sheet.

Use it to understand the mechanics before implementing the full forecast engine.

## Example A: Three-Month Subscription Model

Opening position on 1 January 2027:

| Account | Amount |
|---|---:|
| Cash | 10000 USD |
| Contributed capital | 10000 USD |
| Retained earnings | 0 USD |

Assumptions:

| Assumption | Value |
|---|---:|
| Paying customers | 100 |
| Monthly subscription price | 20 USD |
| Monthly subscription revenue | 2000 USD |
| Customer-specific hosting cost | 2 USD per customer per month |
| Monthly salary OPEX | 1000 USD |
| January launch ads | 500 USD |
| March laptop purchase | 1200 USD |
| Laptop useful life | 12 months |
| Monthly depreciation from March | 100 USD |

The model assumes immediate cash collection and immediate supplier payment.

## Income Statement

| Period | Revenue | Cost of Revenue | OPEX | Depreciation | Net Income |
|---|---:|---:|---:|---:|---:|
| Jan 2027 | 2000 | 200 | 1500 | 0 | 300 |
| Feb 2027 | 2000 | 200 | 1000 | 0 | 800 |
| Mar 2027 | 2000 | 200 | 1000 | 100 | 700 |

Notes:

- January OPEX includes 1000 salary plus 500 launch ads.
- March depreciation is non-cash.
- The laptop purchase itself is not OPEX.

## Cash Flow Statement

| Period | Opening Cash | Collections | Operating Payments | CAPEX | Net Cash Movement | Closing Cash |
|---|---:|---:|---:|---:|---:|---:|
| Jan 2027 | 10000 | 2000 | 1700 | 0 | 300 | 10300 |
| Feb 2027 | 10300 | 2000 | 1200 | 0 | 800 | 11100 |
| Mar 2027 | 11100 | 2000 | 1200 | 1200 | -400 | 10700 |

Notes:

- Operating payments include hosting, salary, and January ads.
- Depreciation is excluded from cash flow because the cash left when the laptop was purchased.
- CAPEX appears as cash outflow in March.

## Balance Sheet

| Period | Cash | Fixed Assets At Cost | Accumulated Depreciation | Total Assets | Contributed Capital | Retained Earnings | Total Equity |
|---|---:|---:|---:|---:|---:|---:|---:|
| Jan 2027 | 10300 | 0 | 0 | 10300 | 10000 | 300 | 10300 |
| Feb 2027 | 11100 | 0 | 0 | 11100 | 10000 | 1100 | 11100 |
| Mar 2027 | 10700 | 1200 | -100 | 11800 | 10000 | 1800 | 11800 |

Reconciliation:

```text
Assets = Liabilities + Equity
```

There are no liabilities in this simple example, so total assets equal total equity.

March:

```text
Cash 10700
+ Fixed assets 1200
- Accumulated depreciation 100
= Total assets 11800

Contributed capital 10000
+ Retained earnings 1800
= Total equity 11800
```

## Example B: Annual Billing And Deferred Revenue

A customer pays 1200 USD in January 2027 for 12 months of service.

Policy:

- Cash is collected immediately.
- Revenue is recognised monthly.
- Monthly recognised revenue is 100 USD.

January result:

| Item | Amount |
|---|---:|
| Cash collected | 1200 USD |
| Recognised revenue | 100 USD |
| Deferred revenue after January recognition | 1100 USD |

Simplified January balance sheet:

| Account | Amount |
|---|---:|
| Cash | 1200 USD |
| Deferred revenue | 1100 USD |
| Retained earnings | 100 USD |

Reconciliation:

```text
Assets 1200 = Liabilities 1100 + Equity 100
```

The startup has cash, but most of it is tied to future service obligations.

## Example C: Funding Requirement

Minimum cash policy:

```text
Closing cash must stay at or above 50000 USD.
```

Projected cash without new funding:

| Period | Closing Cash |
|---|---:|
| Jan 2027 | 80000 |
| Feb 2027 | 40000 |
| Mar 2027 | -10000 |
| Apr 2027 | -80000 |

Funding requirement through April 2027:

```text
Minimum cash policy: 50000
Lowest projected cash before target: -80000
Funding requirement: 130000
```

Reason:

```text
50000 - (-80000) = 130000
```

If the target milestone is reached in March 2027 instead, funding requirement through the milestone is:

```text
50000 - (-10000) = 60000
```

## Example D: Multiple Customer Segments

Assumptions:

| Segment | Paying Customers | Monthly Price |
|---|---:|---:|
| SMB | 100 | 20 USD |
| Enterprise | 10 | 500 USD |

MRR:

| Segment | MRR |
|---|---:|
| SMB | 2000 USD |
| Enterprise | 5000 USD |
| Total | 7000 USD |

Rules:

1. Segment totals must sum to model totals.
2. Segment assumptions may differ.
3. A scenario override for one segment must not silently change another segment.

## What This Example Tests

This example should become one or more golden models covering:

- Revenue versus cash
- OPEX versus CAPEX
- Depreciation as non-cash
- Statement reconciliation
- Retained earnings
- Deferred revenue
- Funding requirement
- Multiple customer segments

