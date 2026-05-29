BBO Capstone Project Model Card for the Gaussian Process Bayesian Optimisation Approach

1. Overview:
Approach name: Adaptive Gaussian Process Bayesian Optimisation
Type: Sequential model-based black-box optimisation
Version: Round 10 final submission
Probabilistic model: Gaussian Process (GP) with Matern kernel
Primary acquisition function: Upper Confidence Bound (UCB) with function-specific beta values
Secondary acquisition function: Expected Improvement (EI) with xi near zero for exploitation phases

2. Intended Use:
 
Suitable Tasks
•	Sequential maximisation of unknown black-box functions with continuous inputs bounded between 0 and 1
•	Functions ranging from 2 to 8 input dimensions where each evaluation is expensive or infrequent
•	Settings where no gradient information, closed-form expression, or function structure is available
•	Problems where exploration-exploitation balance must be actively managed across a fixed query budget

Cases to Avoid
•	Functions with discrete or categorical inputs, which violate the continuous GP assumption
•	Problems where queries are cheap and unlimited, removing the need for sample-efficient strategies
•	Settings where the response surface is highly non-stationary in ways the Matern kernel cannot capture
•	Applications requiring real-time decisions, as GP inference scales poorly with dataset size


3. Strategy and Techniques Across Rounds
The optimisation approach evolved across ten rounds in direct response to observed function behaviour. The core principle applied throughout was to explore broadly until a promising region was identified, then exploit tightly before the search could drift away.
Phase	Rounds	Strategy	Key Technique
Initialisation	Round 1	Broad exploration across all functions	UCB with high beta values; Matern kernel GPs
Differentiation	Round 2	Performance-based strategy divergence	Grid scanning for stagnant functions; lower beta for converging ones
Fine-tuning	Rounds 3–5	Function-specific tailored strategies	Anchor-and-perturb; tight exploitation around identified peaks
Exploitation	Rounds 6–8	Deep exploitation of confirmed regions	Narrow search radii; near-pure EI with xi close to zero
Finalisation	Rounds 9–10	Final refinement and peak locking	Concentrated queries around global best known points
Beta values were assigned per function based on estimated surface complexity: higher values for high-dimensional or noisy functions to encourage broader exploration, lower values for smoother or lower-dimensional functions where exploitation was more appropriate. Strategy decisions were driven entirely by observed output data — if a function did not improve across consecutive rounds, the approach was changed.

4. Performance
Performance was measured as the best observed output value per function at the end of each round, tracked across all ten rounds. The eight functions presented varying levels of difficulty, and improvement trajectories differed substantially across them.
Function	Dimensions	Surface Character	Outcome
Function 1	2D	Smooth with identifiable peak	Strong improvement; peak located and exploited early
Function 2	2D	Noisy with multiple local optima	Moderate improvement; local optima presented consistent challenges
Function 3	3D	Adversarial negated minimisation	Steady improvement once exploration covered sufficient space
Function 4	4D	Dynamic with local optima	Improvement required robust validation before exploitation
Function 5	4D	Unimodal with single sharp peak	High improvement once peak region was located and locked onto
Function 6	5D	Negative by design; complex scoring	Scores remained negative throughout; goal was to approach zero
Function 7	6D	High-dimensional hyperparameter surface	Incremental gains across rounds; surface proved complex
Function 8	8D	High-dimensional; likely complex	Most challenging; global optima difficult to locate

5. Assumptions and Limitations
Assumptions
•	All functions are stationary enough for the Matern kernel to model adequately across the full input domain
•	Output values are sufficiently informative to guide acquisition function decisions even under noise
•	The input space [0,1]^d contains the global optimum for all eight functions
•	Biweekly query rounds provide sufficient feedback frequency to update GP models meaningfully between submissions
Constraints and Failure Modes
•	Fixed query budget means that wasted queries in early rounds cannot be recovered, limiting recovery from poor initialisation
•	GP inference becomes computationally expensive as the query history grows, particularly for higher-dimensional functions
•	Anchor-and-perturb strategy assumes the best known point is in the basin of the global optimum, which may not hold for highly multimodal surfaces
•	Grid scanning, while useful for initial space coverage, is inefficient in higher dimensions and was therefore limited to lower-dimensional functions

6. Ethical Considerations
This optimisation approach was developed and applied in a controlled educational context using synthetic functions. No real-world data, human subjects, or sensitive information was involved at any stage of the project.
Transparency supports reproducibility and adaptation in the following ways. The full query history and output records are preserved in the project repository, enabling any peer or facilitator to replicate the optimisation trajectory from any round. Strategy decisions are documented per round so that the reasoning behind each submission can be reconstructed and evaluated independently. The adaptive nature of the approach — where strategy changes are driven explicitly by observed data — means that the decision logic is auditable at each step rather than opaque.
Any researcher or practitioner seeking to adapt this approach to a real-world setting should consider whether the assumptions of stationarity, bounded input space, and infrequent but informative evaluations hold in their specific context before applying the methodology directly.
