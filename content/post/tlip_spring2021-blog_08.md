+++
title = "TMLE of a treatment-specific multivariate survival curve"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2021-05-01T17:54:00+00:00"
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
> I have a survival analysis question. I am working with a dataset that is left-
> and right-truncated. I am interested in estimating the treatment-specific
> multivariate survival function of a time-to-event variable. For example,
> a study where subjects have been randomized to two different treatment groups
> with baseline covariates `$W$`, but we only observe the outcome -- time at
> death -- for a left- and right-truncated window. Is it possible to use
> Targeted Maximum Likelihood Estimation (TMLE) for estimating the
> treatment-specific multivariate survival curve?
>
> I have seen a few papers using TMLE for right-censored data, but I assume
> there are important considerations when working with doubly-truncated data.
>
> Best,
>
> C.B.

---

## Answer:

Hi C.B.,

Thank you for the excellent question. You are asking about estimation of
a treatment-specific survival curve when we have a time window and a subject is
only part of the sample if a particular event such as death does not occur
before the start of the window, so the sample is conditional on `$T > C_l$`,  or
we only observe units when `$T < C_l$`, for some truncation random variable
`$C_l$` and time until event `$T$`.

In another post, I talk more about this problem of left-truncation and
censoring. I will refer you to that for your question as well. Either way, yes,
TMLE can be applied for any estimation problem, so it is just a matter of
establishing the identification of the full-data distribution `P_X$` from
observing `$O = \Phi(C, X)$` from a conditional distribution of `$T > C_l$`,
say, thereby handling both the biased sampling due to sampling conditional on
`$T > C_l$` as well as the more regular right-censoring, etc., making up
a censored data structure `$\Phi(C, X)$`. For example, one might be able to
assume `$C_l$` is independent of `$X$`, conditional on measured variables, and
show that a conditional distribution of `$X$`, given `$T > C_l$`, implies the
distribution of the full-data random variable `$X$` or a large part of it, so
that a two-stage identification, first identifying `$P_X$` from`$P_{X \mid
T > C_l}$` and then identifying `$P_{X \mid T > C_l}$` from `$P$` of `$O
= \phi(C, X)$` given `$T > C_l$`. Once we have done that, we can map the target
quantity `$\Psi^F(P_X)$` into an estimand `$\Psi(P)$`, specify the statistical
model, and then we are ready to apply TMLE.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
