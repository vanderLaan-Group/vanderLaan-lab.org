+++
title = "TMLE for multi-level treatments and methods for sensitivity analysis"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2020-12-03T17:41:00+00:00"
categories = [
    "Resources",
    "Statistics",
    "Targeted Learning",
    "Q&A"
]
+++

_This post is part of our Q&A series._

A question from graduate students in our Fall 2020 offering of "Biostatistical
Methods: Survival Analysis and Causality" at UC Berkeley:

## Question:

> Hi Mark,
>
> We had two questions for you,
> 1. How to apply TMLE to treatment with multiple levels and conduct inference?
>    For example, if the potential outcomes are `$Y_i(0), Y_i(1), \ldots,
>    Y_i(K)$` for `$K$` different possible treatments, i.e., possible values for
>    `$A_i$` are from `$1$` to `$K$`, how would TMLE work?
> 2. Following a question from office hours, can you further compare your
>    approach to sensitivity analysis with some other recent research on
>    sensitivity analysis, like the E-value proposed by Prof. Peng Ding and
>    Prof. Tyler VanderWeele.
>
> Best,
>
> M.L., S.H., and T.Z.

---

## Answer:

Hi M.L., S.H., and T.Z.,

For the first question (1):

I prefer to use a TMLE that targets the multivariate target parameter
`$(\mathbb{E}Y_1, \ldots, \mathbb{E}Y_K)$`, i.e., each treatment-specific mean
across all `$K$` treatment levels. For example, if we observe `$(W, A, Y)$` then
this TMLE involves adding `$K$` clever covariates `$\mathbb{I}(A = k)
/ \mathbb{P}(A = k \mid W)$`, thus requiring the fitting of `$K$` different
`$\epsilon$`'s (one for each clever covariate in the TMLE fluctuation step). If
`$K$` is not too large, we can do that. If `$K$` is large, then it might be
beneficial to use the one-step TMLE using a uniform least favorable path
targeting all `$K$` target parameters. However if `$K$` is very large, then it
is as if `$A$` is continuous, and `$\mathbb{E} Y_a$` is then not pathwise
differentiable. There are nice pathwise differentiable causal quantities for
continuous exposure such as the shift (or other stochastic) interventions,
marginal structural models, and the like.

For the second question (2):

Sensitivity analysis and estimation of an estimand are two separate topics, one
is not interfering with other, if done correctly. Given a causal quantity, one
defines an estimand that best approximates the causal quantity under causal
assumptions. One develops a TMLE of that estimand and corresponding inference
and confidence intervals. At that point, one could augment the analysis with
a sensitivity analysis. Sensitivity analysis corresponds with obtaining
a confidence interval and or p-value for the causal quantity under the
assumption that the causal gap equals a value `$\delta$` (or is bounded by
`$\delta$`), across a range of `$\delta$` values. One might be able to argue
based on subject matter knowledge that the gap cannot exceed a certain bound.
One can then present the confidence interval and p-value for the causal quantity
across all gap values `$\delta$` up until that maximum value
`$\delta_{\text{max}}$`. This can be done without extra work as in our work
[Diaz, van der Laan,
Luedtke](https://link.springer.com/chapter/10.1007/978-3-319-65304-4_27): just
shift the t-statistic and confidence interval by the gap. E-values might
represent p-values proposed in that spirit, i.e., p-values for the causal
quantity under an assumed causal gap, but I would have to be reminded about
precise proposal. One might be able to get a sense of a typical causal gap
(e.g., a distribution) from the  literature or by evaluating how much the
estimand changes when leaving out some of the measured confounders.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!