BBO Capstone Project Datasheet for the Query History and Function Evaluations Dataset

1. Motivation:
This dataset was created to support a Black-Box Optimisation (BBO) challenge, specifically to document and analyse the query history and function evaluation outputs generated across ten rounds of optimisation. The primary task it supports is the sequential maximisation of eight unknown synthetic functions using Gaussian Process models and acquisition functions, without direct access to the function structure, gradients, or closed-form expressions.
The dataset was created as part of the Imperial College Professional Certificate in Machine Learning and AI, under the supervision of the programme facilitators. It serves both as a record of optimisation decisions and as a reproducible audit trail for evaluating strategy evolution across rounds.

2. Composition:
The dataset consists of two components per function: the input query arrays submitted across all rounds, and the corresponding scalar output values returned by each function evaluation.
Function	Input Dimensions	Output Dimensions	Real-World Analogy
Function 1	2D (array of 2 values)	1D scalar	Radiation field contamination detection
Function 2	2D (array of 2 values)	1D scalar	Log-likelihood of a noisy ML model
Function 3	3D (array of 3 values)	1D scalar	Drug compound side effect minimisation
Function 4	4D (array of 4 values)	1D scalar	Warehouse product placement optimisation
Function 5	4D (array of 4 values)	1D scalar	Chemical process yield maximisation
Function 6	5D (array of 5 values)	1D scalar	Cake recipe scoring (negative by design)
Function 7	6D (array of 6 values)	1D scalar	ML model hyperparameter tuning
Function 8	8D (array of 8 values)	1D scalar	Neural network training configuration

All input values are continuous and bounded between 0 and 1, represented to six decimal places. Output values are scalar real numbers. All tasks are framed as maximisation problems; where the underlying objective is naturally a minimisation (such as side effects in Function 3), the output has been negated so that higher values consistently indicate better performance.
The dataset does not contain personally identifiable information. There are no missing values in the output records, though some functions returned near-zero outputs during early rounds due to poor initialisation, which is noted as a known gap in early-round coverage of the search space.

3. Collection Process:
Queries were generated through an iterative, strategy-driven process across ten biweekly rounds. In each round, input arrays were constructed based on Gaussian Process model outputs and acquisition function evaluations. The specific strategy used evolved across rounds as follows:
•	Round 1: Baseline UCB exploration using Matern kernel GPs with beta values assigned per function based on estimated surface complexity.
•	Round 2: Performance-based differentiation, switching underperforming functions to exploitation-focused strategies and introducing grid scans where outputs remained near zero.
•	Round 3 onwards: Function-specific fine-tuning using anchor-and-perturb methods, tight exploitation around identified peaks, and continued grid scanning for unresolved surfaces.

The dataset was collected between the programme start date and the final submission round. No human subjects were involved and no ethical review was required. All queries were submitted through the course platform interface, with outputs recorded upon each evaluation response.

4. Preprocessing and Uses:
No preprocessing was applied to the raw input or output values beyond formatting to six decimal places for submission. The raw query history and evaluation outputs have been preserved alongside the processed records to support future replication and analysis.
Intended uses of this dataset include analysis of optimisation strategy effectiveness, benchmarking of different acquisition function behaviours across function types, and educational demonstration of Bayesian optimisation principles in practice.
This dataset is not suitable for training general-purpose machine learning models, for drawing statistical inferences about the broader population of black-box functions, or for any application outside the specific BBO challenge context for which it was created.

5. Distribution and Maintenance:
The dataset is stored in the GitHub repository associated with this capstone project and is publicly accessible to course peers and facilitators for review and feedback. The repository serves as the primary distribution channel.
The dataset is made available under the terms of use specified by the Imperial College programme. No additional licensing or intellectual property restrictions apply beyond those governing the course platform and its associated materials.
The dataset is maintained by the student author of this capstone project. Updates or corrections will be applied directly to the repository with version notes where relevant. No automated update pipeline is in place; all changes are managed manually and documented in the repository commit history
