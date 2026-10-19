<img width="1222" height="721" alt="Overview" src="https://github.com/user-attachments/assets/6b2c267b-f344-4b63-8cc2-cb734c9e0257" />
# ⚽ Football-Performance-Analysis

End-to-end data analytics project: cleaning a 170K-row football dataset, exploring it, building a machine learning model to predict player match ratings, and presenting the results in an interactive Power BI dashboard.

## Overview

Football player-match data is messy by nature — different leagues track different stats, goalkeepers and field players have fundamentally different action profiles, and "missing" doesn't always mean "zero." This project works through that mess end-to-end: cleaning ~173K player-match records with a root-cause approach to missing data, exploring performance patterns, training and comparing three regression models to predict `player_rating`, and shipping a 4-page Power BI dashboard built on the final model's predictions.

## Dataset

- **~173,587 records, 63 columns** — one row per player per match
- **Period:** January 31 – June 8, 2025
- **Coverage:** multiple leagues and competitions worldwide
- **Target variable:** `player_rating` (0–10 scale)
- Features span identifiers, match context, attacking stats (goals, assists, dribbles, crosses, passing), and defensive stats (tackles, interceptions, duels)

## Project Workflow

### 1. Data Cleaning
Rather than a blanket imputation strategy, missing values were diagnosed by cause before deciding how to handle them:
- Checked whether missingness correlated with `is_goalkeeper` (goalkeepers don't attempt dribbles/crosses/shots) or with `league_name` (some leagues lack advanced stat tracking)
- Columns with missingness fully explained by goalkeeper role → filled with 0
- Columns with ~50% missingness, independent of league → left as `NaN` with a `stat_recorded` helper flag, since filling with 0 would misrepresent "not tracked" as "did nothing"
- `expected_assists` / `xg_plus_xa` showed missingness ranging from 25% to 99.7% across leagues → dropped from modeling entirely to avoid bias
- Checked for duplicate records, logical inconsistencies (e.g. successful actions exceeding attempts), and redundant columns
- **Result:** 170,273 clean rows, 58 columns

### 2. Exploratory Data Analysis
- Target distribution, home-field advantage, win/draw/loss effect on rating
- League-level rating comparison
- Correlation analysis to identify the strongest rating drivers
- Descriptive leaderboards (top scorers, assisters, highest-rated players)

### 3. Modeling
Three models were trained and compared on a held-out 20% test set:

| Model | Test RMSE | Test MAE | Test R² |
|---|---|---|---|
| Linear Regression (baseline) | 0.348 | 0.238 | 0.782 |
| Random Forest | 0.293 | 0.197 | 0.846 |
| **XGBoost (final model)** | **0.265** | **0.167** | **0.873** |

Tree-based models (Random Forest, XGBoost) were trained directly on raw data with `NaN` preserved, rather than imputed, so they could learn missingness patterns natively instead of treating an imputed value as if it were real.

**Top predictive features:** `goals`, `goal_difference`, `assists`, `touches`, `minutes_played`

### 4. Power BI Dashboard
A 4-page interactive dashboard (Overview, Player Analysis, Team Analysis, Model Evaluation) built on the XGBoost model's predictions, with cross-filtering by league, team, and match result. All model-evaluation metrics are computed on test-set rows only, to avoid inflating reported accuracy with training data the model had already seen.


### Overview
<img width="1300" height="700" alt="Overview" src="https://github.com/user-attachments/assets/e503ac79-4174-42cc-a78c-252375bf7545" />


### Player Analysis
<img width="1300" height="700" alt="Player Analysis" src="https://github.com/user-attachments/assets/4fcb9ec7-209d-479c-85da-ec5e63ba1715" />


### Team Analysis
<img width="1300" height="700" alt="Team Analysis" src="https://github.com/user-attachments/assets/b3b4cd6a-eae3-4647-b159-1ca43be2ae0a" />


### Model Evaluation
<img width="1300" height="700" alt="Model Evaluation" src="https://github.com/user-attachments/assets/d62dafd9-2966-4e33-9270-eb9e3faaf64b" />



## Key Insights

- Home teams average a higher player rating than away teams (6.85 vs. 6.71)
- Players on winning teams average 7.18 vs. 6.38 for losing teams
- League identity has a weak effect on rating (0.22-point spread between highest and lowest league) — not used as a model feature
- Prediction error is higher for players on losing teams (MAE ≈ 0.20) than winning teams (MAE ≈ 0.14), likely due to greater variance in individual performance during losses

## Tech Stack

**Python:** Pandas, NumPy, Scikit-learn, XGBoost, Matplotlib, Seaborn
**BI/Visualization:** Power BI (Power Query, DAX)


---

### 👤 Contact
* **LinkedIn:** [My LinkedIn Profile](https://www.linkedin.com/in/zahrasadr)
* **Portfolio:** [My GitHub Profile](https://www.github.com/zsadr67)

*Designed by ZahraSadr*
