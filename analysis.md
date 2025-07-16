#  Aave Wallet Score Analysis

##  Score Distribution

We analyzed ~100,000 DeFi transactions across various wallets and assigned scores (0–1000) using behavior-based heuristics.

### Distribution of Scores:

| Score Band  | Number of Wallets |
|-------------|-------------------|
| 0–100       | 91                |
| 100–200     | 117               |
| 200–300     | 99                |
| 300–400     | 97                |
| 400–500     | 83                |
| 500–600     | 105               |
| 600–700     | 92                |
| 700–800     | 102               |
| 800–900     | 110               |
| 900–1000    | 104               |

---

## Observations

###  Wallets in Lower Score Bands (0–300)

- **Low repay ratios**: Borrowed more than they repaid.
- **High liquidation events**: Frequently liquidated by Aave.
- **Low diversity**: Typically used only 1-2 actions (e.g., borrow only).
- **Short lifespan**: Operated over just a few days.
- **Bot-like behavior**: Rapid, repetitive small-value actions.

###  Wallets in Higher Score Bands (700–1000)

- **High repay ratio**: Repaid nearly all borrowed funds.
- **Healthy activity**: Active over long periods (30+ days).
- **Diverse usage**: Used deposit, borrow, repay, redeem strategically.
- **Low liquidation rate**: Rarely liquidated — indicative of good health.
- **Low volatility**: Stable transaction patterns.

---

##  Insights

- There's a healthy spread of scores — not too skewed.
- The score bands help easily separate "power users" from risky or spammy ones.
- This system could be integrated into lending dApps for dynamic interest rates or credit lines.

---
