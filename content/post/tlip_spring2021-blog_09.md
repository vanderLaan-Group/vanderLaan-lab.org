+++
title = "Tuning the highly adaptive lasso estimator"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2021-05-02T11:54:00+00:00"
categories = [
    "Resources",
    "Statistics",
    "Targeted Learning",
    "Q&A"
]
+++

_This post is part of our Q&A series._

A question from graduate students in our Spring 2021 offering of the new course
"Targeted Learning in Practice" at UC Berkeley:

## Question:

> Hi Mark,
>
> In chapter 6 of your 2018 book [_Targeted Learning in Data
> Science_](https://www.springer.com/us/book/9783319653037), co-authored with
> [Sherri Rose](https://drsherrirose.org/), you discuss the practical necessity
> of reducing the number of basis functions incorporated into the highly
> adaptive lasso (HAL) estimator when the number of covariates grows. There, you
> point out that any HAL estimator attaining the minimum empirical risk up to an
> approximation error smaller than the desired rate of convergence will converge
> to the target parameter at that rate, and that we might expect a less complex
> HAL to achieve this minimum since the empirical risk depends on the parameter
> only through a number of values equal to the observed sample size. A super
> learner strategy is then proposed using a library comprised of HALs, using
> increasing numbers of basis functions, which would be practically implemented
> by tracking cross-validated risk along this sequence of increasingly complex
> HALs and choosing a final estimator when the cross-validated risk stops
> increasing based on the assumption that more aggressive HALs will not achieve
> significantly better cross-validated risk.
>
> __Q1__: Are there cases where we might expect significantly improved
> performance by more aggressive HALs after an earlier plateau or dip in
> cross-validated risk?
>
> __Q2__: When would it be beneficial to incorporate other candidate estimators
> along with HAL in super learner, and when might we expect a super learner only
> tuning HALs in the above manner to perform adequately well?
>
> __Q3__: Can you briefly explain or provide some intuition regarding HAL and
> the reduction f finite-sample remainders?
>
> Best,
>
> D.C.

---

## Answer:

Hi D.C,

Thank you for the excellent questions.

__Q1__: It will depend on the ordering of the indicator basis functions. One
attractive strategy is to randomly sample basis functions in a manner
proportional to their sparsity (i.e., minimum proportion of 1s, minimum of
proportion of 0s). In that case, I would expect that it would gradually get
better so that one could stop at a plateau. Rachael Phillips and Zeyi Wang have
been investigating this somewhat, so we will hopefully learn more soon. Without
random sampling, we need to have a deeper understanding to make sure that our
ordering works well. I am interested in an ordering that is outcome-blind, so
that you can think of it as an ordering that would work for a regression
function that has support all over the `$d$`-dimensional unit cube in the HAL
representation `$Q(x) = \int_{u} \mathbb{I}_{[0, x]}(u) dQ(u)$`, i.e., with
`$dQ(u)$` being somewhat uniform. Maybe this will turn out not to be possible,
and we will have to generate many orderings and include these ordering-specific
HALs in the super learner. Having such a screening procedure, with
cross-validation to tune where to cut off the sequence, will improve the
statistical performance of HAL and also improve its scalability.

__Q2__: I believe that a super learner with a variety of HAL algorithms varying
over screening and using higher-order splines, resulting in the
smoothness-adaptive HAL (as defined in my technical report), will be hard to
beat. The `$L_1$` norm covers the range from parametric to aggressive, making it
a robust algorithm (i.e., zeroth-order HAL does not do any extrapolation), and,
by being an MLE over function classes, it has a dimension-free convergence rate
`$n^{-1/3}$`, is efficient for smooth features, and has an almost normal
sampling distribution. There are no other machine learning algorithms out there
that can compete with these properties.

__Q3__: I presume you are referring to using HAL in TMLE or undersmoothed HAL as
a plug-in estimator for a smooth feature of the target function, to improve the
finite-sample performance of an estimator of a smooth target feature. The key is
that HAL solves a lot of score equations and thereby solves any score in the
linear span of the HAL estimator. This linear span grows to all functions as
`$n$` increases, when some undersmoothing is used. Exact second-order remainders
`$R(P, P_0) = \Psi(P) - \Psi(P_0) + P_0 D^{\star}_P$` can be represented as
`$P_0 A_P(h_P)$`, where `$A_P(h_{P,P_0})$` is a score, i.e., `$A_P(h)$` is
a score operator mapping an index `$h$` into a score at `$P$`, and `$h_{P,
P_0}$` is a special index generating the exact remainder. As a consequence of
using HAL-MLE `$\hat{P}$`, then `$P_n A_{\hat{P}}(h) = 0$` for many `$h$`, and
the linear span approximates `$A_P(h_{P, P_0})$`. Once `$P_n
A_{\hat{P}}(h_{\hat{P}, P_0}) \approx 0$`, we have reduced the exact remainder
to `$(P_0 - P_n) A_{\hat{P}}(h_{\hat{P}, P_0})$`, which is now similar to
a sample mean of mean-zero random variables. So, by using an estimator that
solves not only the score `$P_n D^{\star}_{\hat{P}} = 0$` but also many other
score equations, we knock out most of the exact remainder. The [higher-order
TMLE technical report](https://arxiv.org/abs/2101.06290) formally proves that by
solving the right score equations, we make the remainder
a `$k^{\text{th}}$`-order difference instead of a second-order difference. The
higher-order TMLE utilizes the fact that we know which scores we truly need to
target and directly targets them beyond the `$D^{\star}_P$`, while HAL targets
lots of scores globally, though it will still get there too. So, HAL,
undersmoothed, can act as a higher-order TMLE.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
