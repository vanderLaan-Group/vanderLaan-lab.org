+++
title = "Unified Data Adaptive Learning"
date = "2016-12-23"
sidemenu = "false"
description = "Projects on Unified Data Adaptive Learning"
+++

We have developed a unified loss based methodology for data adaptive estimation/learning of any parameter, including regression and density estimation as special cases, based on a sample of i.i.d. observations. The parameter of interest is defined as the minimizer of an expectation of a loss function of the experimental unit, a candidate parameter, and possibly a nuisance parameter. By allowing the loss function to depend on a nuisance parameter such a loss function can be constructed for any parameter (finite or infinite dimensional). An important component of this data adaptive learning methodology involves a unified loss-based cross-validation methodology for selection among a set of candidate estimators, typically indexed by elements of a user supplied sequence of subspaces of the parameter space (also called a sieve). Since the loss function is aimed at the parameter of interest, it selects the estimator (e.g. indexed by a model choice) which is closest to the true parameter of interest. This general framework encompasses in particular a number of selection and estimation problems which have traditionally been treated separately in the statistical literature: predictor selection and data adaptive learning based on censored outcomes, predictor selection and data adaptive learning based on multivariate outcomes, density estimator selection and data adaptive density estimation, survival function estimator selection and learning, and counterfactual predictor selection and data adaptive learning in causal inference. Finite sample and asymptotic optimality results were derived for the cross-validation selector for general data generating distributions, loss functions (possibly depending on a nuisance parameter), and estimators. The asymptotic optimality result states that the cross-validation selector performs asymptotically as well as an optimal benchmark selector based on the true unknown data generating distribution. A broad definition of cross-validation is used in order to cover leave-one-out cross-validation, V-fold cross-validation, and Monte Carlo cross-validation. We developed a general adaptive loss based estimator for which we establish finite sample results which establish formally the minimax adaptivity. Applications to genomic data analysis include: the prediction of biological and clinical outcomes (possibly censored) using microarray gene expression measures, the identification of regulatory motifs in DNA sequences, and genetic mapping using single nucleotide polymorphism (SNP) haplotypes. In addition, we provide an analogue of the loss based learning methodology based on estimating functions identifying the parameter of interest, and establish the wished theoretical results.
Specific Projects:

**Efficient, Double Robust Estimation in a Weight Loss Study.**<br>
(with Daniel Rubin)

**Deletion/Substitution/Addition (D/S/A) algorithm**<br>
(with Sandra Sinisi)

**Loss-based estimation in causal inference**<br>
(with Yue Wang)

**HIV-1 genotype data**<br>
(with Jefferey Fessel [Kaiser Permanente], Robert Shafer [Div. Infectious Diseases, Stanford University Medical Center], Sandra Sinisi, Art Reingold, Maya Peterson)

## Specific Papers:

Mark J. van der Laan and Sandrine Dudoit (April 7, 2003)  Unified Cross-Validation Methodology For Selection Among Estimators: Finite Sample Results, Asymptotic Optimality, and Applications

Sandrine Dudoit and Mark J. van der Laan (February 5, 2003) Asymptotics of Cross-Validated Risk Estimation in Model Selection and Performance Assessment

Mark J. van der Laan, Sandrine Dudoit, and Sunduz Keles (February 5, 2003) Asymptotic Optimality of Likelihood Based Cross-Validation

Sunduz Keles, Mark J. van der Laan, and Sandrine Dudoit (January 28, 2003) Asymptotically Optimal Model Selection Method for Regression on Censored Outcomes
