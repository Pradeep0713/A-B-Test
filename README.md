# A/B Test – Mobile Game Retention (Cookie Cats)

Does moving the first in-game "gate" from **level 30 to level 40** change how many players keep playing?
This project analyses a real A/B test on **90,189 players** of the mobile puzzle game *Cookie Cats* and uses statistical testing to decide which version the business should keep.

---

## Business Problem

In Cookie Cats, players hit a **gate** where they must wait or pay before they can continue.
The game team ran an experiment:

| Group | Version | Gate position |
|---|---|---|
| Control | `gate_30` | Level 30 (original) |
| Test | `gate_40` | Level 40 (new) |

**Question:** Should the gate be moved to level 40, or kept at level 30?
**Main metric:** 7-day retention (D7): did the player come back 7 days after installing?

---

## Dataset

- **Source:** Kaggle – *Cookie Cats* mobile game A/B test
- **Rows:** 90,189 players (90,188 after removing one outlier)
- **Columns:**

| Column | Description |
|---|---|
| `userid` | Unique player ID |
| `version` | `gate_30` (control) or `gate_40` (test) |
| `sum_gamerounds` | Game rounds played in the first 14 days |
| `retention_1` | Player returned 1 day after install (True/False) |
| `retention_7` | Player returned 7 days after install (True/False) |

---

## Approach

1. **Data check:** no missing values and no duplicate users
2. **Group balance:** gate_30 = 44,700 players and gate_40 = 45,489 players (about 50/50, so randomisation looks fair)
3. **EDA and outliers:** removed one extreme player with 49,854 rounds; found 3,994 players who installed but never played
4. **Retention rates:** D1 and D7 retention per group
5. **Hypothesis testing:** two-proportion z-test, confirmed with a chi-square test (α = 0.05)
6. **Confidence intervals:** 95% CIs for D7 retention
7. **Bootstrap:** 1,000 resamples to estimate how likely gate_30 is better

**Hypotheses (D7 retention)**
- H0: there is no difference in D7 retention between gate_30 and gate_40
- H1: there is a difference

---

## Results

| Version | D1 retention | D7 retention |
|---|---|---|
| gate_30 (control) | **44.82%** | **19.02%** |
| gate_40 (test) | **44.23%** | **18.20%** |

| Metric | z-test p-value | Chi-square p-value | Significant at 0.05? |
|---|---|---|---|
| D1 retention | 0.0739 | 0.075 | No |
| D7 retention | **0.0016** | **0.0016** | **Yes** |

**95% confidence intervals (D7 retention)**
- gate_30: 19.02% (18.65% – 19.38%)
- gate_40: 18.20% (17.85% – 18.55%)

The intervals do not overlap, and almost all bootstrap samples show gate_30 with higher D7 retention.

---

## Conclusion and Recommendation

- Moving the gate to level 40 **reduced 7-day retention from 19.02% to 18.20%**, and the drop is **statistically significant** (p ≈ 0.0016).
- The small difference in 1-day retention is **not statistically significant** (p ≈ 0.07).
- **Recommendation: keep the gate at level 30.** An earlier break appears to keep players engaged for longer.
- **Business impact:** with thousands of new installs every day, a 0.8 percentage-point drop in D7 retention means hundreds fewer active players each week, and lower in-game revenue.

---

## Tools Used

Python · Pandas · NumPy · SciPy · statsmodels · Matplotlib · Seaborn · Jupyter / Google Colab

---

## How to Run

1. Download `cookie_cats.csv` from Kaggle and place it in the project folder.
2. Install the libraries:
   ```bash
   pip install pandas numpy scipy statsmodels matplotlib seaborn
   ```
3. Open and run `ab_test_cookie_cats.ipynb`.

---

## Repository Structure

```
ab-testing-cookie-cats/
├── ab_test_cookie_cats.ipynb   # full analysis notebook
├── README.md                   # project summary (this file)
└── images/                     # charts used in the README (optional)
```

---

**Author:** Pradeep Amileneni · [LinkedIn](https://linkedin.com/in/amileneni-pradeep) · [GitHub](https://github.com/Pradeep0713)
