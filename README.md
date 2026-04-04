# BlackBoxOptimization

Black-Box Optimisation (BBO) Capstone Project

Section 1: Project Overview
This project is part of a Bayesian optimisation challenge where the goal is to find the maximum output of eight unknown black-box functions by submitting carefully chosen input queries. Each function is synthetic but designed to mimic real-world complexity — including noise, multiple local peaks, and non-linear behaviour. Because each function can only be queried occasionally, every submission must count.
The goal is to maximise each function's output using as few queries as possible, without ever inspecting the function directly. This mirrors real-world scenarios such as hyperparameter tuning, drug discovery, and chemical process optimisation — situations where evaluating a candidate solution is expensive or slow.
This project builds skills in probabilistic modelling, sequential decision-making, and adaptive strategy — all directly transferable to data science roles involving experimentation under uncertainty.

Section 2: Inputs and Outputs
Each function takes a fixed-length array of continuous values between 0 and 1, and returns a single scalar output. The number of input dimensions varies by function:
FunctionDimensionsReal-World Analogy12DRadiation field contamination detection22DLog-likelihood of a noisy ML model33DDrug compound side effect minimisation44DWarehouse product placement optimisation54DChemical process yield maximisation65DCake recipe scoring (negative by design)76DML model hyperparameter tuning88DNeural network training configuration
Query Format
Each value starts with 0. and is given to six decimal places, separated by dashes:
0.413867 — 0.793714 — 0.715778
All tasks are framed as maximisation problems. Where a function is naturally a minimisation (such as side effects), the output is negated so higher is always better.

Section 3: Challenge Objectives
The objective is to find the input combination that maximises each function's output across weekly query rounds.
Key Constraints
ConstraintDescriptionFixed budgetQueries are biweekly — wasted queries cannot be recoveredUnknown structureNo formula, no gradient, no way to inspect the functionLocal optimaA good result in one region does not guarantee it is the global maximumVariable surfacesSome functions are smooth, others are noisy or nearly flat with one sharp spikeDimensionalityHigher-dimensional functions are harder — the search space grows exponentially

Section 4: Technical Approach
Round 1 — Baseline UCB Exploration
All functions were modelled with Gaussian Process (GP) models using a Matérn kernel. Queries were selected via Upper Confidence Bound (UCB) acquisition, with beta values assigned per function — higher beta for complex surfaces to encourage exploration, lower beta for simpler functions to encourage exploitation.
Round 2 — Performance-Based Differentiation
After reviewing results, strategies were differentiated for the first time:

✅ Improving functions — kept UCB settings
❌ Non-improving functions — switched to exploitation-focused strategies, including tighter search areas, lower beta values, and in one case a systematic grid scan to map the space before further optimisation

Round 3 — Function-Specific Fine-Tuning
Each function received its own tailored strategy:

Grid scan continued for the function whose output remained near zero
Anchor-and-perturb applied to a function whose promising region had been abandoned — locks onto the best known point and varies one input at a time to find what drives improvement
Tight exploitation applied immediately when a sharp peak was found, to extract further gains before the search could drift away

Balancing Exploration and Exploitation

Explore broadly until a promising region is found, then exploit tightly.

ParameterEffectHigh betaMore exploration — searches new regionsLow betaMore exploitation — refines known good regionsxi ≈ 0Near-pure exploitation via Expected ImprovementTight search radiusConstrains search close to the best known point
What Makes This Approach Thoughtful
Each function is treated as an independent problem. Strategy decisions are driven entirely by data — if a function does not improve, the strategy changes. If a peak is found, the search locks onto it. This adaptive approach mirrors how a data scientist must respond to model behaviour when the underlying system is unknown.

Last updated: Week 3 — strategy continues to evolve each round.
