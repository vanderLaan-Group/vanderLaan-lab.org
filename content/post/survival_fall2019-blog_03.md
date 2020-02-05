+++
title = "Simultaneous Inference with Kaplan-Meier Estimator of Survival"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2019-12-29T17:30:00+00:00"
categories = [
    "Resources",
    "Statistics",
    "Targeted Learning",
    "Q&A"
]
+++

_This post is part of our Q&A series._

A question from graduate students in our Fall 2019 offering of "Biostatistical
Methods: Survival Analysis and Causality" at UC Berkeley:

<u>**Question:**</u>
> Hi Mark,
>
> First of all, I have doubts regarding the simultaneous confidence interval 
> for Kaplan-Meier, since I am not necessarily interested in inference for a 
> parameter. I would like to know if the 95% confidence band for my KM estimator 
> will hold using the same formula we did in our `R` lab without covariates 
> (taken from lectures). 
>
> Secondly, I am interested in testing a hypothesis. I remember discussing in 
> classes the difference between testing the null hypothesis when each of my 
> parameters is 0, is different than the null hypothesis that all of the 
> parameters are 0. I understand that the first is trying to address each 
> parameter individually, while the second one is testing whether all of the 
> parameters, simultaneously, are significant.
>
> For instance, for three covariates.
> Case 1:
> `$H_01: w1 = 0$`
> `$H_02: w2 = 0$`
> `$H_03: w3 = 0$`
> Case 2:
> `$H_0: w1 = w2 = w3 = 0$`
>
> Therefore, is the simultaneous confidence interval calculating the second case 
> only? For a KM estimator, can it be understood that the simultaneous 
> confidence interval can be computed for one parameter across time, and for all 
> of the parameters across time?
>
> Thanks, F.M.

---

<u>**Answer:**</u>

Hi F.M.,

I believe your question concerns understanding of the construction and utility 
of a simultaneous confidence interval.

Let's consider the case that we are interested in a survival function `$S(t)$`.
Suppose that we have an estimator `$S_n(t)$` which is asymptotically linear with 
known influence curve `$IC_t(O)$`, such as a Kaplan-Meier estimator for the simple 
marginal right-censored data structure or a TMLE for longitudinal data structures 
involving baseline covariates, treatment and potentially time-dependent covariates.

Then, `$S_n(t)-S(t)$` across a collection of points `$t$` will have influence curve the 
stacked `$IC=(IC_t: t)$` across these time points `$t$`. This allows us to estimate the 
correlation matrix of `$(\sqrt{n}(S_n(t)-S(t)): t)$` by the correlation matrix `$\rho$` 
of stacked influence curve `$IC=(IC_t: t)$`. The `$0.95$` quantile `$q(0.95)$` of the 
max of absolute value of a random draw from `$N(0,\rho)$` consistently estimates the 
`$0.95$` quantile of `$\text{max}_t | S_n-S|(t)/SE(S_n(t))$`, where `$SE(S_n(t))$` is 
an estimator of the standard error (SE) of `$S_n(t)$` such as the square root of the 
sample variance of `$IC_t$`, divided by `$n$`.  In particular, `$S_n(t)\pm q(0.95) 
SE(S_n(t))$` is now a simultaneous confidence band.

The multivariate normal distribution can be used as a null distribution in 
multiple testing of `$H_0(t): S(t)=S_{H_0}(t)$` for some hypothesized survivor function 
`$S_{H_0}$`, across collection of time points `$t$`, thereby obtaining a powerful 
multiple testing procedure taking into account the strong correlations between 
`$S_n(t)$`across `$t$`. This improves strongly on Bonferroni adjustments or, in 
general, methods that only utilize marginal `$p$`-values (thus ignore correlations 
between the test statistics).

Similarly, we could do all this for a contrast `$S_1-S_0(t)$` given an estimator of 
`$S_1-S_0$`, so that it would provides inference for a difference of survival
functions w.r.t two populations or difference of treatment specific survival
functions. In general, the above story applies to any functional valued or vector 
valued pathwise differentiable parameter `$\mu=(\mu(t); t)$` for which an 
asymptotically linear estimator `$\mu_n$` is available (when doing functions we would 
establish `$\sqrt{n}(\mu_n-\mu)$` as random function converges in distribution to a 
Gaussian process in function space, but these results generally hold under bounded 
influence curves.

The multivariate normal distribution `$N(0,\rho)$` can also be used to obtain a test 
for an overall test `$H_0: S=S_{H_0}$` across all `$t$`. To start with the simultaneous 
confidence interval could be used as a test by checking if `$S_{H_0}$` is contained in 
the simultaneous band. Alternatively, we could define as our test statistic the 
Euclidean norm of `$\rho^{-1/2} (S_n-S_{H_0})/SE(S_n)$`, where `$\rho^{1/2}$` is the 
square root of the correlation matrix `$\rho$`, so that` $\rho{-1/2}$` is its inverse. 
Under `$H_0:S=S_{H_0}$`, this Euclidean norm converges to the Euclidean norm of a 
`$N(0,\mathbb{1}_{t \times t})$`, and thus has  a limit `$\Chi^2$` distribution with as 
many degrees of freedom as number of time points `$t$`.

The main point is that we have an estimate of the sampling distribution of 
`$(S_n-S)/ SE(S_n) \sim N(0,\Sigma)$`, or better, we have its sample mean 
approximation as the sample mean of the stacked influence curve, and that allows 
us to understand the distribution of any functional of this (Functional delta 
method), or any test statistic.

So the influence curve based inference can be used for single overall test about 
the whole curve $S$, or for testing multiple hypotheses under any type of Type I error 
control (see [the book by Dudoit and van der 
Laan](https://link.springer.com/book/10.1007/978-0-387-49317-6) for the use of
this multivariate normal distribution to obtain general multiple testing 
procedures, even based on step-down, step-up, and any kind of Type I error).

Best Wishes,
Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!