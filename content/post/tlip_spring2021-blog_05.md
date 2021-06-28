+++
title = "Conditions for Asymptotic Efficiency of TMLE"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2021-05-01T17:44:00+00:00"
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

__Question:__

> Hi Mark,
>
> I have a question regarding the requirements for asymptotic efficiency of
> TMLE.
>
> Asymptotic efficiency of TMLE relies on the second-order remainder being
> negligible. Is this purely a finite-sample concern, or are there potentially
> parameters of interest where this isn't true by construction?
>
> Best,
> M.M.

---

__Answer:__

Hi M.M.,

Thank you for the excellent question.

In the past, this was an asymptotic concern as well, since there were no general
estimators that achieved the critical `$n^{-1/4}$` rate of convergence.
Nowadays, we have the highly adaptive lasso (HAL), which achieves `$n^{-1/3}$`
only assuming that the true function is cadlag with finite sectional variation
norm. This type of smoothness assumption is weak, so that this should generally
hold making the HAL-TMLE an asymptotically efficient estimator, assuming strong
positivity for the target estimand. On the other hand, we could think about
problems in which the true function has circular-type discontinuities making it
non-cadlag and have infinite variation norm accordingly, or is a continuous
function with infinite variation norm. In such cases, I am not aware of any
estimator that will get us the desired rate of convergence. Nonetheless, we can
still make it work if either `$Q(A,W)$` or `$g(A \mid W)$` has this desired
smoothness condition satisfied. So `$Q(A,W)$` might not, but a TMLE that
estimates `$g(A \mid W)$` with undersmoothed HAL would still be asymptotically
efficient if `$g(A \mid W)$` is cadlag with finite variation norm. There is also
the option to apply HAL to coordinate transformations which could be learned
from the data, which would make it possible to approximate the true function
with a cadlag function of these transformed covariates. For example, meta-HAL
operates like that and would allow to overcome this if given the right type of
transformations or learners of transformations.

For sure the exact remainder is a finite sample concern. It has motivated the 1)
HAL-based bootstrap for HAL and HAL-TMLE; 2) estimation of nuisance functions
with HAL so that it solves lots of score equation, knocking out a lot about the
exact remainder; and 3) the higher-order TMLE, reducing the exact remainder into
a higher-order difference, to name a few.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
