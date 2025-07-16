## `README.md` — Aave V2 Wallet Credit Scoring

````markdown
#  Aave V2 Wallet Credit Scoring System

This project builds a behavior-driven **credit scoring system** for Ethereum wallets using historical DeFi interactions with the **Aave V2 protocol**. 

Each wallet is assigned a score from **0 to 1000**, representing its reliability, responsibility, and trustworthiness based solely on **on-chain transactional behavior** — no identity, KYC, or custodial data required.

---

## Dataset

- Source: Sample of ~100,000 raw transaction-level interactions from Aave V2
- Format: JSON (≈87MB)
- Contents: Each record includes a wallet address, timestamp, action (`deposit`, `borrow`, `repay`, `redeemUnderlying`, `liquidationCall`), amount, and asset.

---

##  Approach Overview

### Step 1: Data Cleaning

- Loaded the full JSON and normalized into a structured DataFrame.
- Removed or handled invalid, zero, or missing transaction values.
- Converted timestamps into human-readable datetime formats.
- Ensured consistency of action labels and units.

### Step 2: Feature Engineering

For each wallet, we computed:

-  Total counts per action (e.g., `num_deposit`, `num_borrow`, ...)
-  Total volume per action (e.g., `sum_borrow`, `sum_repay`, ...)
-  Average and max transaction value per type
-  Active days & transaction frequency
-  Behavior ratios:
  - Repay ratio = `sum_repay / sum_borrow`
  - Liquidation rate = `num_liquidationCall / total_actions`
  - Diversity index = count of unique actions used

### Step 3: Scoring Logic

We defined a **custom weighted formula** to assign a credit score from 0 to 1000:

```python
score = (
    (repay_ratio * 300) +
    (1 - liquidation_rate) * 250 +
    (activity_score_normalized * 200) +
    (diversity_score_normalized * 150) +
    (avg_tx_value_normalized * 100)
)
````

* Scores are bounded using min-max normalization
* Higher scores = safer, long-term, multi-functional wallets
* Lower scores = risky, short-lived, or exploitive wallets

---

##  Outputs

*  `wallet_scores.csv`: Final scores for each wallet
*  `score_distribution.png`: Visualization of score band distribution
*  `analysis.md`: Insightful analysis on high/low scoring wallet behaviors

---

##  How to Run

###  1. Install Requirements

```bash
pip install -r requirements.txt
```

###  2. Run Scoring Script

```bash
python src/score_generator.py --input data/user-wallet-transactions.json --output outputs/wallet_scores.csv
```

###  3. Generate Analysis

After scoring, use the helper script to:

* Save markdown-based insights to `outputs/analysis.md`
* Generate a plot image: `outputs/score_distribution.png`

---

##  Example Score Distribution

| Score Band | Number of Wallets |
| ---------- | ----------------- |
| 0–100      | 91                |
| 100–200    | 117               |
| 200–300    | 99                |
| 300–400    | 97                |
| 400–500    | 83                |
| 500–600    | 105               |
| 600–700    | 92                |
| 700–800    | 102               |
| 800–900    | 110               |
| 900–1000   | 104               |

---

##  Applications

This score can be used to:

* Build **risk-adjusted interest rates** on DeFi platforms
* Filter **bots, flash-loan attackers, or exploiters**
* Offer **cross-platform lending** based on wallet reputation
* Power **on-chain credit markets** without centralized KYC

---

##  Future Work

* Use unsupervised clustering or LLMs for behavioral grouping
* Extend model to multiple DeFi protocols (Compound, MakerDAO, etc.)
* Incorporate token volatility, collateralization ratio trends

---

##  Repo Structure

```
aave-credit-scoring/
├── data/                    # Raw input dataset
├── src/                     # Scoring script
├── outputs/                 # Scores, plots, analysis
├── analysis.md              # Insightful markdown summary
├── readme.md                # Project overview
├── requirements.txt         # Python dependencies
```

