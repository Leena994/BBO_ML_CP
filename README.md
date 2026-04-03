# BlackBoxOptimization

Black-Box Optimisation (BBO) Capstone Project
**Section 1: Project Overview**
This project is part of a Bayesian optimisation challenge where the goal is to find the maximum output of eight unknown black-box functions by submitting carefully chosen input queries. Each function is synthetic but designed to mimic real-world complexity — including noise, multiple local peaks, and non-linear behaviour. Because each function can only be queried occasionally, every submission must be as informative as possible.
The overall goal is to maximise the output of each function using as few queries as possible, without ever being able to inspect the function directly. This is directly relevant to real-world machine learning scenarios such as hyperparameter tuning, drug discovery, chemical process optimisation, and model selection — all situations where evaluating a candidate solution is expensive, slow, or both.
This project develops skills in probabilistic modelling, sequential decision-making, and adaptive strategy — all of which are transferable to any data science role involving experimentation under uncertainty.

**Section 2: Inputs and Outputs**
Each function takes a fixed-length array of continuous input values between 0 and 1, and returns a single scalar output value. The number of input dimensions varies by function:
FunctionInput dimensionsReal-world analogy12DRadiation field contamination detection22DLog-likelihood of a noisy ML model33DDrug compound side effect minimisation44DWarehouse product placement optimisation54DChemical process yield maximisation65DCake recipe scoring (negative by design)76DML model hyperparameter tuning88DNeural network training configuration
Queries must follow this format — each input value starts with 0. and is given to six decimal places:
x1 — x2 — x3 — ... — xn
Example for a 3D function: 0.413867 — 0.793714 — 0.715778
All eight tasks are framed as maximisation problems. Where a function is naturally a minimisation (such as Function 3 which measures side effects), the output is negated so that higher is still better.

**Section 3: Challenge Objectives**
The objective is to find the input combination that maximises each function's output within a limited number of queries submitted across weekly rounds.
Key constraints and challenges include:
Queries are submitted biweekly, so there is a fixed budget — wasted queries cannot be recovered. The function structure is entirely unknown — there is no formula, no gradient, and no way to inspect the internals. Some functions have multiple local peaks, meaning a good result in one region does not guarantee it is the global maximum. Response surfaces vary widely — some are smooth and unimodal, others are noisy with many local optima, and some are nearly flat in most of the space with a single sharp spike. Higher-dimensional functions are harder to optimise because the search space grows exponentially with each additional input variable.

**Section 4: Technical Approach**
Round 1 — Baseline UCB exploration
All eight functions were initialised with Gaussian Process (GP) models using a Matérn kernel. Queries were selected using Upper Confidence Bound (UCB) acquisition with beta values assigned per function based on their known characteristics — higher beta for functions expected to have many local optima or complex surfaces, lower beta for functions expected to be unimodal. This gave a broad initial exploration across all functions.
Round 2 — Performance-based differentiation
After observing which functions improved and which did not, strategies were differentiated for the first time. Functions showing consistent improvement kept their UCB settings. Functions showing no improvement were switched to exploitation-focused strategies — tighter search areas around the best known point, lower beta values, and in one case a systematic grid scan to map the space before any optimisation could be meaningful.
Round 3 — Function-specific fine-tuning
By the third round, each function had its own tailored strategy based on accumulated evidence. A grid scan continued for the function whose output remained near zero. An anchor-and-perturb approach was applied to a function whose best result had been accidentally abandoned — this locks the search onto the best known point and systematically varies one input at a time to identify which variable drives improvement. Functions that found sharp peaks were immediately switched to tight exploitation to extract further gains before the search could drift away.
Balancing exploration and exploitation
The core principle applied throughout is: explore broadly until a promising region is found, then exploit tightly before moving on. UCB beta controls this balance — high beta favours exploration, low beta favours exploitation. The xi parameter in Expected Improvement acquisition was set near zero for pure exploitation when needed. Search radius in exploit mode was progressively tightened as confidence in the best region grew.
What makes this approach thoughtful
Rather than applying a single strategy uniformly, this project treats each function as an independent optimisation problem with its own characteristics. Decisions to change strategy are driven by data — if a function's output does not improve, the strategy changes. If a peak is found, the search locks onto it. This adaptive, evidence-based approach mirrors how a data scientist should respond to model behaviour in practice.
