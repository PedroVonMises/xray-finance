# synthetic-dividend-investor

Invented statements for parser, deduplication and end-to-end tests. **Not valid for metric assertions**: prices, income amounts and corporate event dates are approximations, not B3 data.

## Files

- `fixtures/synthetic-dividend-investor-movimentacao.xlsx` (sheet `Movimentação`, 229 rows)
- `fixtures/synthetic-dividend-investor-negociacao.xlsx` (sheet `Negociação`, 125 rows)

The column layout was written from memory of the B3 Área do Investidor export and still has to be checked against a real export header.

## Persona

Fullstack developer working as a contractor (PJ), R$ 20,000 monthly income, 30% (R$ 6,000) invested every month. Buy and hold of high dividend stocks, no sells, one broker.

## Rules used

| Rule | Value |
| --- | --- |
| Period | Jan 2023 to Oct 2026, 46 monthly purchases |
| Purchase day | First business day on or after the 8th; settlement two business days later |
| Assets | BBAS3, ITSA4, TAEE11, BBSE3, PETR4, VALE3, EGIE3, CXSE3; two per month in rotation, R$ 3,000 each |
| Lots | Multiples of 100 in `Mercado à Vista`; the remainder in `Mercado Fracionário` with the `F` suffix |
| Income | Dividends and JCP on the 20th of each asset's payment months, on the position held 15 days earlier; JCP net of 15% |
| Split | BBAS3 2:1 on 2024-04-16 |
| Bonus | ITSA4 5% on 2023-11-29 and 2024-12-18, with fraction debit and fraction auction |
| Identical rows | Two identical TAEE11F buys on 2024-06-10 (ADR-004) |

## Totals

| Item | Value |
| --- | --- |
| Total bought | R$ 274,558.99 |
| Income received | R$ 45,610.21 (2023: 3,003.84; 2024: 9,549.51; 2025: 15,366.84; 2026: 17,690.02) |
| Final position | ITSA4 3,755; CXSE3 2,592; BBAS3 1,550; BBSE3 1,042; TAEE11 1,008; PETR4 990; EGIE3 830; VALE3 505 |

## Not covered

Sells, FII yields, transfers between brokers, subscriptions, reverse splits and ticker changes. `synthetic-basic` covers those.
