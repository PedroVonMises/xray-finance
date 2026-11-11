# Fixtures

| Path | Committed | Content |
| --- | --- | --- |
| `fixtures/synthetic-*.xlsx` | Yes | Invented portfolios in the B3 export layout. Run in CI. |
| `fixtures/malformed-*.xlsx` | Yes | Broken layouts that must produce a clear parser error. |
| `fixtures/real/` | **No** (gitignored) | Real statements, used with the owner's consent. Local only. |

See ADR-005 in `docs/DECISIONS.md`.

## Preparing a real statement

1. Export "Movimentação" from the B3 Área do Investidor.
2. Delete any cell holding name, CPF or account number.
3. Optional: multiply every quantity by one secret integer per investor. Percentage metrics stay identical; the size of the portfolio is hidden. This is obfuscation, not anonymity.
4. Save as `fixtures/real/<alias>.xlsx`. The alias is never the person's name.

## Synthetic fixture template

Each synthetic fixture has a matching `docs/fixtures/<name>.md` with the expected metrics calculated by hand.

`synthetic-basic` must contain at least one of each: buy, sell, dividend, JCP, FII yield, split, bonus, transfer between brokers, and two identical buys on the same day (ADR-004).
