# Black-Box Optimisation (BBO) Capstone Project

> Imperial College Professional Certificate in Machine Learning and AI

---

## Project Overview

This project is part of a Bayesian optimisation challenge where the goal is to find the maximum output of eight unknown black-box functions by submitting carefully chosen input queries. Each function is synthetic but designed to mimic real-world complexity, including noise, multiple local peaks, and non-linear behaviour. Because each function can only be queried occasionally, every submission must count.

The goal is to maximise each function's output using as few queries as possible, without ever inspecting the function directly. This mirrors real-world scenarios such as hyperparameter tuning, drug discovery, and chemical process optimisation — situations where evaluating a candidate solution is expensive or slow.

This project builds skills in **probabilistic modelling**, **sequential decision-making**, and **adaptive strategy** — all directly transferable to data science roles involving experimentation under uncertainty.

---

## Inputs and Outputs

Each function takes a fixed-length array of continuous values between 0 and 1, and returns a single scalar output. The number of input dimensions varies by function:

| Function | Dimensions | Real-World Analogy |
|----------|------------|-------------------|
| Function 1 | 2D | Radiation field contamination detection |
| Function 2 | 2D | Log-likelihood of a noisy ML model |
| Function 3 | 3D | Drug compound side effect minimisation |
| Function 4 | 4D | Warehouse product placement optimisation |
| Function 5 | 4D | Chemical process yield maximisation |
| Function 6 | 5D | Cake recipe scoring (negative by design) |
| Function 7 | 6D | ML model hyperparameter tuning |
| Function 8 | 8D | Neural network training configuration |

### Query Format

Each input value starts with `0.` and is given to six decimal places, separated by dashes:

```
0.413867 — 0.793714 — 0.715778
```

All tasks are framed as **maximisation** problems. Where a function is naturally a minimisation — such as side effects in Function 3 — the output is negated so that higher values consistently indicate better performance.

---

## Challenge Objectives

The objective is to find the input combination that maximises each function's output across biweekly query rounds. The key constraints governing the challenge are outlined below:

| Constraint | Description |
|------------|-------------|
| Unknown structure | No formula, no gradient, and no way to inspect the function directly |
| Local optima | A strong result in one region does not guarantee it is the global maximum |
| Variable surfaces | Some functions are smooth; others are noisy or nearly flat with a single sharp spike |
| Dimensionality | Higher-dimensional functions are harder — the search space grows exponentially with each added dimension |

---

## Technical Approach

The optimisation approach was iterative and data-driven, with each submission informed by the results obtained from previous evaluations. Initial efforts focused on exploring the search space broadly to identify promising regions, after which the search became increasingly concentrated around areas demonstrating strong performance.

Different strategies were applied depending on the observed behaviour of each function, including local refinement of high-performing regions and broader exploration where improvement remained limited or uncertain. Throughout the project, optimisation decisions were continuously adapted based on emerging evidence.

Functions showing consistent gains were subjected to more focused searches, while functions exhibiting complex or unstable behaviour received additional exploration to improve understanding of the solution space. This adaptive approach enabled efficient use of the available query budget while progressively improving the quality of solutions identified.

### Key Principles Applied Throughout

- **Explore broadly** until a promising region is identified
- **Exploit tightly** once a strong output is found, before the search drifts
- **Change strategy** if a function stagnates across consecutive rounds
- **Treat each function independently** based on its observed behaviour

### Strategy Evolution Across Rounds

| Phase | Rounds | Strategy | Key Technique |
|-------|--------|----------|---------------|
| Initialisation | Round 1 | Broad exploration across all functions | UCB with high beta values; Matern kernel GPs |
| Differentiation | Round 2 | Performance-based strategy divergence | Grid scanning for stagnant functions; lower beta for converging ones |
| Fine-tuning | Rounds 3–5 | Function-specific tailored strategies | Anchor-and-perturb; tight exploitation around identified peaks |
| Exploitation | Rounds 6–8 | Deep exploitation of confirmed regions | Narrow search radii; near-pure EI with xi close to zero |
| Finalisation | Rounds 9–10 | Final refinement and peak locking | Concentrated queries around global best known points |

---

## Repository Structure

```
├── README.md
├── datasheet/
│   └── BBO_Datasheet.docx
├── model_card/
│   └── BBO_Model_Card.docx
└── data/
    └── query_history/
```

---

## Documentation

| Document | Description |
|----------|-------------|
| [Datasheet](datasheet/BBO_Datasheet.docx) | Documents the query history and function evaluation dataset — motivation, composition, collection process, preprocessing, and maintenance |
| [Model Card](model_card/BBO_Model_Card.docx) | Documents the optimisation approach — overview, intended use, strategy, performance, assumptions, limitations, and ethical considerations |

---

## Acknowledgements

This project was completed as part of the **Imperial College Professional Certificate in Machine Learning and AI**, under the supervision of the programme facilitators.
