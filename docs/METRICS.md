# Metric definitions

Status: **accepted 2026-11-11**. Every rule here needs a hand-calculated case in `docs/fixtures/` before Phase 4.

## 1. Portfolio boundary

The portfolio is the set of B3 assets held (stocks, FIIs, ETFs). Cash is outside. The B3 statement does not show deposits to the broker, so a "contribution" is defined by what crosses this boundary.

| Statement row | Quantity effect | External flow |
| --- | --- | --- |
| Buy | + | Contribution of the trade total |
| Sell | − | Withdrawal of the trade total |
| Dividend, JCP, FII yield | none | Withdrawal of the amount credited |
| Subscription exercised | + | Contribution of the amount paid |
| Fraction auction proceeds | − (fraction) | Withdrawal |
| Transfer between brokers | none (net zero) | None |
| Split, reverse split, bonus | × factor / + bonus shares | None |

Known limits, shown in the UI:

- Brokerage fees are not in the statement and are ignored.
- All figures are gross of income tax, on both the portfolio and the benchmarks.
- JCP is taken as credited (already net of withholding).

## 2. Portfolio value

`V(t) = Σ quantity(i, t) × close(i, t)` using unadjusted COTAHIST closes. If an asset has no trade on day `t`, the last close is carried forward for at most 5 business days; after that the asset is flagged as stale.

## 3. Net invested and income

- Net invested = Σ contributions − Σ withdrawals from sells.
- Income received = Σ dividends + JCP + FII yields.

## 4. Money-weighted return (XIRR)

Solve for `r`:

```
Σ CF_i / (1 + r)^((d_i − d_0) / 365) = 0
```

Contributions are negative, withdrawals positive, and `V(T)` is a final positive flow on the valuation date `T`.

- Solver: bisection on `[−0.99, 10]`, tolerance `1e-9`. Bisection over Newton because it cannot diverge.
- No sign change in the bracket returns the domain error `xirr_no_root`.
- Span under 365 days: report the period return `(1 + r)^(days / 365) − 1`, never the annualized figure.

## 5. Time-weighted return (TWR)

Daily sub-period return, trades assumed at start of day, income at end of day:

```
r_t = (V_t + income_t) / (V_{t−1} + buys_t − sells_t) − 1
TWR = Π (1 + r_t) − 1
```

- Denominator zero (portfolio empty): `r_t = 0`, the chain pauses.
- Annualized as `(1 + TWR)^(365 / days) − 1`, only when the span is at least 365 days.
- Known distortion: the price drops on the ex-date but income is counted on the payment date, so TWR dips between the two. No receivables accrual in the MVP.

## 6. Counterfactual

Every external flow in section 1, income included, is replayed on the same date in the benchmark. Both sides hand the investor the same cash on the same days, so the terminal values are comparable.

```
B(T) = Σ −CF_i × growth(d_i → T)
```

| Benchmark | `growth(d → T)` |
| --- | --- |
| CDI | Product of `(1 + rate_d / 100)` over business days in `[d, T)`, SGS series 12 |
| IPCA + 0% | Product of monthly factors, SGS series 433, pro rata by business days in partial months; unpublished months count as zero |
| BOVA11 | `close(T) / close(d)`, fractional units allowed |

Assumption to verify in Phase 3: BOVA11 pays no distributions and has had no splits in the period.

**Depleted benchmark:** if the running benchmark balance goes below zero on any date, the result carries `benchmark_depleted` with that date. The headline then states the fact ("in CDI the money would have run out in MM/YYYY") instead of a benchmark annual rate, which would be meaningless.

The benchmark annual rate is the XIRR of the same flows with `B(T)` as the final flow.

## 7. Concentration

`weight_i = V_i(T) / V(T)`. Top five is the sum of the five largest weights.

## 8. Tolerances

| Quantity | Tolerance against the hand-calculated case |
| --- | --- |
| Money | 1 cent |
| Rates and returns | `1e-6` absolute |
| Weights | `1e-6` absolute |
