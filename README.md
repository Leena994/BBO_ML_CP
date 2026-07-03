# Black-Box Optimisation (BBO)
> Sample-efficient optimisation of eight unknown functions using Gaussian Process-based Bayesian search
---

## Overview

This project solves eight independent optimisation problems where the relationship between inputs and outputs is completely unknown — a black box. Each function can only be queried by submitting an input and observing the output it returns; the underlying mathematics is never revealed.

Because every evaluation is costly, the goal isn't just to find a good solution — it's to find it in as few evaluations as possible. This project builds a Bayesian optimisation pipeline that models each function's behaviour from limited observations, decides where to search next, and adapts its strategy week over week as evidence accumulates.

Alongside the optimisation engine, the project includes a full dashboard-generation system that turns the search history into a clear, visual record of what was tried, why, and what happened — for eight functions across twelve rounds of experimentation.

---

## Why This Problem Matters

Many real-world problems share the same shape: you can't test every possible option, and each test is expensive, slow, or resource-intensive. Tuning a manufacturing process, screening a chemical compound, or configuring a machine learning model all come down to the same question — *where do I run my next experiment to learn the most, or do the best, with the least waste?*

Rather than searching by trial and error, this project uses a search process that learns from every prior result before choosing what to try next. Early on, it explores broadly to understand each function's behaviour. As evidence builds, it shifts toward exploitation — concentrating evaluations around the strongest known regions while still allowing for course correction if a function's behaviour shifts.

The accompanying dashboards make this process auditable: every decision — whether to explore, exploit, or roll back to a previous basin — is documented and visualised, not just the final result.

---

## The Eight Optimisation Problems

Each function stands in for a distinct real-world optimisation scenario, spanning a range of input dimensionalities and search-space behaviours:

| Function | Represents | Input Dimensions |
|----------|------------|-------------------|
| F01 | Radiation Field Detection | 2 |
| F02 | Noisy ML Log-Likelihood | 2 |
| F03 | Drug Compound Side Effects | 3 |
| F04 | Warehouse Placement | 4 |
| F05 | Chemical Process Yield | 4 |
| F06 | Recipe Quality Scoring | 5 |
| F07 | ML Hyperparameter Tuning | 6 |
| F08 | Neural Network Configuration | 8 |

---

## Approach

- **Surrogate modelling:** A Gaussian Process is fitted to each function's accumulated history, providing both a prediction and an uncertainty estimate at any candidate point.
- **Adaptive strategy selection:** Each function is assigned a search strategy — broad exploration (UCB), anchored local exploitation, trust-region refinement, or rollback — chosen per week based on how that function has behaved so far.
- **Anchor-based safety net:** Rather than always chasing the most recent result, the pipeline tracks the strongest verified result across the full history for each function and anchors the next search around it, preventing a single noisy week from derailing progress.
- **External benchmarking:** An independent reference dataset is checked against each function's own best result, and only adopted where it demonstrably outperforms it.
- **Full transparency:** Every strategy switch, rollback, and exploitation decision is logged and rendered into the dashboards below — the reasoning is visible, not just the outcome.

---

## Results

Working solutions were identified for all eight functions using twelve rounds of optimisation. The notebook automatically produces a full set of dashboards covering:

- Weekly progress and output trends
- Search behaviour and model belief over the input space
- Per-function performance summaries
- Final applied strategy and next-step recommendations
- How the strategy for each function evolved over time

---

## Overall Optimisation Summary
![Overall Summary](Images/Weekly_progress_overview.png)

Compares optimisation performance across all eight functions.

---

## Optimisation Strategy
![Strategy](Images/strategy_followed_each_week.png)

Shows how the search strategy for each function evolved week to week — where it explored, where it exploited, and where it rolled back.

---

## Final Strategy Summary
![Final Strategy](Images/final_applied_strategy_table.png)

The optimisation decisions applied across all eight functions, end to end.

---

## Example Dashboard — Function F01
![F01](Images/function_01_dashboard.png)

---

## Example Dashboard — Function F05
![F05](Images/function_05_dashboard.png)

---

## Example Dashboard — Function F08
![F08](Images/function_08_dashboard.png)

---

## Repository Structure

```
BBO_ML_CP
│
├── Data/
├── Images/
├── Gaussian-Bayesian Weekly Optimisation Tracker.ipynb
├── README.md
├── BBO_Datasheet.md
├── BBO_Model_Card.md
├── BBO_Project_Overview.md
└── LICENSE
```

---

## Documentation

- [`BBO_Datasheet.md`](BBO_Datasheet.md) — data sources, structure, and provenance
- [`BBO_Model_Card.md`](BBO_Model_Card.md) — modelling approach, assumptions, and limitations
- [`BBO_Project_Overview.md`](BBO_Project_Overview.md) — extended write-up of the project

---

## Technologies Used

- Python
- NumPy / Pandas
- scikit-learn (Gaussian Process regression)
- Matplotlib (dashboard rendering)
- Jupyter Notebook

---

## License

See [`LICENSE`](LICENSE) for details.
