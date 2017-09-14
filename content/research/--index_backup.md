---
date: 2016-12-23T08:21:44+08:00
title: Research
---
## Statement of purpose

Current statistical practice typically involves application of  parametric models, even though everybody agrees that these parametric models are wrong. That is, they agree that one somehow needs to interpret the fitted coefficients in this parametric model when it is known that the parametric model is misspecified.  Moreover, they accept these wrong methods, even though these are guaranteed to result in a biased estimate of the target parameter they had in mind when applying these parametric models, and, consequently, biased confidence intervals and p-values. The parametric models are used for convenience, not because they represent knowledge. For example, one applies linear regression and cox proportional hazard regressions to  analyze effects of certain variables on an outcome of interest. In addition, to deal with complexity of real data sets, one often ignores large chunks of the observed data on a unit in order to be able to fit one of the available parametric models to the data, thereby not only causing bias but also inflating the variance of the estimator by failing to explain the inhomogeneity of the measured outcomes with the available information. For example, randomized controlled trials are often analyzed by fitting a cox proportinal hazards model with only treatment included as a covariate, thereby ignoring all the other baseline and time-dependent covariates.  As a consequence, the current practice of statistics often fails to learn the truth from data, and, at a minimum, due to the toolbox consisting of the application of wrong parametric models, is more of an art than a science. For society this means a great loss of resources and missed opportunities. 

Our goal is to develop fully automated targeted estimators of target parameters within the context of realistic semiparametric models. This involves the following steps. Firstly, one defines the data structure of the experimental unit, so that one is able to write down the probability  distribution of the data, i.e. the so-called likelihood of the data. Secondly, one states the actual known assumptions about this distribution of the data, which represents the so-called model, i.e., the collection of possible data generating distributions. One might be able to augment this statistical model with some non-testable assumptions that allow non-statistical (e.g., causal) interpretations of the target parameter. This model will almost always be a semi-parametric model that involves at most some restrictions on the distribution of the data, but is far from identifying the distribution of the data by a finite dimensional parameter. Thirdly, one now needs to define the target parameter as a mapping from a candidate distribution of the data to its value. This means we cannot think in terms of coefficients of a parametric regression model, rather, one needs to explicitly define (nonparametrically) the target feature(s) of the data generating distribution one wishes to learn from the data.  Just like one does in parametric maximum likelihood estimation, we now develop substitution estimators of the target parameter based on a data adaptive maximum likelihood (or other loss function) based estimator of the relevant portion of the distribution of the data. The two-stage methodology aims to obtain an estimator of the data generating distribution with as small mean squared error for the target parameter as possible. The first stage involves loss-based super learning based on a loss function that identifies the part of the data generating distribution that is needed to evaluate the target parameter.  The super learning allows risk-free and extensive modeling: the user can build a library of candidate estimators based on a variety of prior beliefs/models and algorithms, and wrong guesses do not hurt, but, can greatly improve the practical performance of the resulting super learner which uses cross-validation to select the best weighted combination of the candidate estimators. For example, beyond including in this library a set of data-adaptive estimators respecting the actual knowledge of the semiparametric model, one can also include a collection of guessed parametric model based maximum likelihood estimators. The super learner now provides an estimator of the (part of) the data generating distribution. Each candidate estimator represents a particular bias-variance trade-off whose effectiveness in approaching the true data generating distribution will very much depend on the true data generating distribution, but the super learner will use cross-validation to select the best bias-variance trade-off.  Though super learning is optimal for estimation of infinite dimensional parameters w.r.t. the loss-function based dissimilarity, such as for nonparametric density estimation and prediction, it is overly biased for smooth target parameters of the infinite dimensional parameter. Therefore, a second updating step is needed. The second stage involves applying the targeted maximum likelihood update of the super learner, which corresponds with fitting the data w.r.t. the target parameter so that bias (due to optimal global learning of the data generating distribution in first stage) of the super learner  w.r.t. the target parameter gets removed. This targeted maximum likelihood estimator requires specification of a least favorable model (used to define fluctuations of the initial estimator) which is implied by the so-called efficient influence curve/canonical gradient of the target parameter. As a consequence, the construction of this final targeted estimator of the data generating distribution and thereby the target parameter requires, in essence, determining a loss function, a library of candidate estimators, and the efficient influence curve.  The estimators are accompanied with confidence intervals and and are used to carry out tests of null hypotheses, including multiple testing. The acceptance of the non-testable assumptions in the model can allow the target parameter, and thereby the estimator and confidence interval, to be interpreted as (e..g) a causal effect, but the pure statistical interpretation (e.g, an effect controlling for measured confounders) of the target parameter should be respected. 

We develop these targeted learning tools to estimate causal and non-causal parameters of interest based on observational longitudinal studies, with informative censoring and missingness, as well as randomized controlled trials. To develop these methods to their fullest potential, we work with simulated and real data in collaboration with biologists, medical researchers, government agencies such as the FDA, epidemiologists, and other companies.

## Projects and grants

Below is a list of current projects. Please also see the [students' pages](../students) for additional projects. Previous projects can also be found through the following links: [censored data and causal inference](./causality), [computational biology](./compbio), [data-adaptive learning](./crossval), and [multiple hypothesis testing](./multtest).

### Current Projects

- **Biomedical Informatics for Critical Care**<br>
Intel Corporation<br>
P.I. Stuart Russell, PhD and Geoffrey Manley, MD, PhD

Here we propose to bring together a powerful multidisciplinary team from academic medicine (UCSF), computer science (UCB), and industry (Intel’s Digital Health Group) to develop a high performance information system and novel computational techniques for critical care medicine. To create the infrastructure and methods needed to achieve this goal, we will develop a scalable warehouse for critical care data to facilitate multi-institutional collaboration and knowledge discovery. In parallel, we will explore the potential of advanced informatics methods to improve patient classification, prognostic accuracy, and clinical decision-making. We believe that this multidisciplinary approach will ultimately fuel the development of a new generation of ICU information systems to improve the diagnosis, treatment and outcome of critically ill and injured patients.


- **Fresno Asthmatic Children’s Environment Study**<br>
National Institutes of Health/National Heart, Lung and Blood Institute   <br>
P.I. Ira Tager
                   
The specific aims of the study are the following:  1) to evaluate the long-term health effects of exposure to air pollutants/bioaerosols on symptoms, asthma severity and growth of lung function; 2) to evaluate the extent to which genes involved in defense against oxidative stress influence short and long-term pollutant effects; 3) to evaluate the effects of interactions between exposures to traffic related pollutants and bioaerosals on both short-term and long-term pollutant effects; 4) to use methods of causal analysis (marginal structural models) and compare the quantitative inferences between causal and association analyses.

 
- **Toxic Substances in the Environment, Subproject Core D, Biostatistics and Computing**<br>
National Institutes of Health/National Institute of Environmental Health Sciences                <br>
P.I. Martyn Smith

The major goals of this project are to provide project investigators with consultative support in biostatistics, and to support computer-based communication and database needs.                                                     

                                   
- **Statistical Methods to Study the Epidemiology of HIV & Other Diseases**<br>
National Institutes of Health <br>
P.I. Nicholas Jewell

Development of statistical techniques for the analysis of a variety of data sets relating to HIV disease, uterine fibroids and other diseases including STIs.

                      
- **Statistical Techniques for Complex Environmental Epidemiological Studies**<br>
National Institutes of Health <br>
P.I. Nicholas Jewell

This study is devoted to the development of statistical techniques to understand the effect of environmental exposures on the onset of disease in the presence of diagnosis data and assess the effects of multiple environmental exposures on a variety of pregnancy and child development outcomes.

         
- **Pregnancy Outcomes in Polycystic Ovary Syndrome**<br>
Kaiser Permanente Division of Research (NIH Prime) <br>
P.I. Joan Lo

This is an epidemiologic study focused on the pregnancy outcomes of women with polycystic ovary syndrome using retrospective data obtained from Kaiser Permanente Northern California.  Dr. van der Laan will provide oversight and input in regarding to modeling approaches and will supervise the work of the Assistant Researcher.


- **Targeted Empirical Super Learning in HIV Research**<br>
National Institutes of Health <br>
P.I. Mark van der Laan

This project will develop a general statistical methodology, called Targeted Empirical Learning, into a practical product that can be applied to answer scientific questions.   Specific applications include treatment rules for HIV-infected patients; activity levels in the elderly; mutation of HIV-virus for prediction of clinical response to drug combinations; measures of variable importance/causal effects of air pollution components on asthmatic children.


- **Biological Response Indicators of Environmental Stress Center:  Project 1 protein Adducts as Molecular Signatures of Carcinogen Dose**<br>
National Institutes of Health <br>
P.I. Steven Rappaport

The major goal of Project 1 is to demonstrate the viability of protein adductomics as a true omics approach.


- **Center for Integrative Research on Childhood Leukemia and the Environment: Project 2 - Exposure Assessment for Childhood Leukemia**<br>
National Institutes of Health/National Institute of Environmental Health Sciences <br>
P.I. Patricia Buffler
Project Leader: Stephen Rappaport 

A project to assess exposures to persistent contaminants present in homes that may cause leukemia, based upon analysis of house dust, blood collected at the time of the diagnosis of leukemia cases, and archived newborn dried blood spots collected at birth.


- **Contemporary Treatment and Outcomes for Atrial Fibrillation in Clinical Practice**<br>
National Institutes of Health/National Heart, Lung and Blood Institute <br>
P.I. Alan Go

Collaboration between two members of the NHLBI-sponsored Cardiovasuclar Research Network (KPNC, KPSC) and a clinical trialist consortium to address the following aims: Aim 1:Develop and test novel risk stratification schemes for adverse outcomes (thromboembolism/stroke, bleeding) in patients with atrial fibrillation on and off anticoagulation from a large-scale community-based cohort and additionally validate risk models against patients enrolled in randomized clinical trials. Aim 2: Establish and characterize contemporary registries of incident and prevalent atrial fibrillation within very large, diverse community-based populations to provide critical insights into current outcome event rates and practice patterns, potential health disparities, and to facilitate more rapid enrollment into future effectiveness studies and clinical trials.  Aim 3: Identify and validate novel statistical approaches for conducting comparative effectiveness (CE) studies (eg, antithrombotic therapy, catheter ablation) in observational studies and clinical trials

### Recent Abstracts

- **Methodology Development in Comparative Effectiveness Research**<br>
Jennifer Creasman, Susan Gruber, Mark van der Laan<br>

We have three objectives for this project.  First, we hope to bridge the gap between scientific progress and broad adoption by industry, academic, and government agencies by developing professional, user-friendly super learner and targeted maximum likelihood estimation (SL-TMLE) software that will allow researchers to compare patient management strategies, identify which strategies work best for specific subgroups and predict outcomes based on individual clinical and demographic characteristics. Second, we aim to demonstrate the superiority of SL-TMLE by analyzing simulated data based on a broad range of existing “real” datasets. Collaborators have pledged access to ten datasets that address some of the priority areas outlined by the Institute of Medicine including cardiovascular disease, functional limitations and HIV/AIDS. These collaborations will result in publications demonstrating SL-TMLE to solve outstanding data analysis problems in CER.  Finally, we aim to increase the understanding and use of SL-TMLE throughout the research community by disseminating SL-TMLE materials via publications and presentations.

## Papers

Mark's and his student's papers and publications can be found on the bepress website. Recent and important publications are also listed below.

Recent editorial in Amstat News by Mark van der Laan & Sherri Rose: "Statistics Ready for a Revolution"

### 2009 Papers

Causal Inference in Epidemiological Studies with Strong Confounding 
Kelly L. Moore, Romain S. Neugebauer, Mark J. van der Laan, and Ira B. Tager (2009)

Readings in Targeted Maximum Likelihood Estimation
Mark J. van der Laan, Sherri Rose, and Susan Gruber (2009)

Causal Inference for Nested Case-Control Studies using Targeted Maximum Likelihood Estimation 
Sherri Rose and Mark J. van der Laan (2009)

Targeted Maximum Likelihood Estimation: A Gentle Introduction 
Susan Gruber and Mark J. van der Laan (2009)

Nonparametric population average models: deriving the form of approximate population average models estimated using generalized estimating equations
Alan E. Hubbard and Mark J. van der Laan (2009)

Resampling-Based Multiple Hypothesis Testing with Applications to Genomics: New Developments in the R/Bioconductor Package multtest
Houston N. Gilbert, Katherine S. Pollard, Mark J. van der Laan, and Sandrine Dudoit (2009)

Application of Time-to-Event Methods in the Assessment of Safety in Clinical Trials 
Kelly L. Moore and Mark J. van der Laan (2009)

Collaborative Double Robust Targeted Penalized Maximum Likelihood Estimation
Mark J. van der Laan and Susan Gruber (2009)

Joint Multiple Testing Procedures for Graphical Model Selection with Applications to Biological Networks
Houston N. Gilbert, Mark J. van der Laan, and Sandrine Dudoit (2009)

Selecting Optimal Treatments Based on Predictive Factors
Eric C. Polley and Mark J. van der Laan (2009)

Modified FDR Controlling Procedure for Multi-Stage Analyses
Catherine Tuglus and Mark J. van der Laan (2009)

Confidence Intervals for the Population Mean Tailored to Small Sample Sizes, with Applications to Survey Sampling 
Michael A. Rosenblum and Mark J. van der Laan (2009)

Why Match? Investigating Matched Case-Control Study Designs with Causal Effect Estimation
Sherri Rose and Mark J. van der Laan (2009)

### Important Papers

Important papers can be found in the "Readings in Targeted Maximum Likelihood Estimation" on the bepress website. The papers contained in that publication are also listed below:

Targeted Maximum Likelihood Learning
M.J. van der Laan, D. Rubin (2006) 

Super Learner
M.J. van der Laan, E.C. Polley, A.E. Hubbard (2007)

Loss-Based Cross-Validated Deletion/Substitution/Addition Algorithms in Estimation
S.E. Sinisi, M.J. van der Laan (2004)

Collaborative Double Robust Targeted Penalized Maximum Likelihood Estimation
M.J. van der Laan, S. Gruber (2009)

Covariate Adjustment in Randomized Trials with Binary Outcomes: Targeted Maximum Likelihood Estimation
K.L. Moore, M.J. van der Laan (2008)

Selecting Optimal Treatments Based on Predictive Factors
E.C. Polley, M.J. van der Laan (2009)

Simple, Efficient Estimators of Treatment Effects in Randomized Trials Using Generalized Linear Models to Leverage Baseline Variables
M. Rosenblum, M.J. van der Laan (2009)

Estimating the Effect of Vigorous Physical Activity on Mortality in the Elderly Based on Realistic Individualized Treatment and Intention-to-Treat Rules
O. Bembom, M.J. van der Laan (2007) 

Targeted Methods for Biomarker Discovery, the Search for a Standard
C. Tuglus, M.J. van der Laan (2008)

Biomarker Discovery using Targeted Maximum Likelihood Estimation: Application to the Treatment of Antiretroviral Resistant HIV Infection
O. Bembom, M.L. Petersen, S.-Y. Rhee , W. J. Fessel, S.E. Sinisi, R.W. Shafer, M.J. van der Laan (2008)

Data-adaptive Selection Of The Adjustment Set in Variable Importance Estimation
O. Bembom, W. J. Fessel, R.W. Shafer, M.J. van der Laan (2008) 

Estimation Based on Case-Control Designs with Known Prevalance Probability
M.J. van der Laan (2008)

Simple Optimal Weighting of Cases and Controls in Case-Control Studies
S. Rose, M.J. van der Laan (2008)

Why Match? Investigating Matched Case-Control Study Designs with Causal Effect Estimation
S. Rose, M.J. van der Laan (2009) 

Causal Inference for Nested Case-Control Studies using Targeted Maximum Likelihood Estimation
S. Rose, M.J. van der Laan (2009)

A Note on Targeted Maximum Likelihood and Right Censored Data
M.J. van der Laan, D. Rubin (2007)

Application of Time-to-Event Methods in the Assessment of Safety in Clinical Trials
K.L. Moore, M.J. van der Laan (2009)

Targeted Maximum Likelihood Estimation: A Gentle Introduction
S. Gruber, M.J. van der Laan (2009)

Targeted Maximum Likelihood Learning: Examples and Generalizations
M.J. van der Laan (2009)

Unified Cross-Validation Methodology For Selection Among Estimators and a General Cross-Validated Adaptive Epsilon-Net Estimator: Finite Sample Oracle Inequalities and Examples
Mark J. van der Laan and Sandrine Dudoit (2003)