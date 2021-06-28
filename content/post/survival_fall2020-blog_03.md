+++
title = "Using time-varying covariates in evaluating the causal effect of a single time point intervention"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2020-12-03T17:48:00+00:00"
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
> We have an observational study with fixed baseline intervention, $A$ for
> statin use vs. no statin use, along with baseline covariates, `$L$` such as
> age, gender, marital status, hypertension, diabetes, hypercholesterolemia,
> coronary artery disease. Our goal is to predict conversion to the more
> impaired stage of Alzheimer's disease. Because our data come from a
> longitudinal study, we also have `$L$` measured at each of the follow-up
> times. Some of the `$L$`, including hypertension, diabetes, coronary artery
> disease, etc., are time-dependent; however, the treatment $A$ is only measured
> at baseline and therefore we cannot use L-TMLE. That said, we would like to
> know whether we can/should incorporate these time-dependent covariate
> structure in some capacity to our analysis? Or do we just ignore them and only
> consider the covariates at baseline?
>
> Best,
>
> C.L. and L.S.

---

## Answer:

Hi C.L. and L.S.,

Your data structure is of the form `$L(0), A, L(t), N(t)$`, across time points
`$t = 1, \ldots , \tau$`, where `$L(t)$` are additional time-dependent
covariates and `$N(t)$` might be an outcome/counting process such as the
indicator of developing Alzheimer's disease by time `$t$`. If your interest is
in estimating a causal effect of `$A$` on `$N(\tau)$`, say, then your question
is concerned with the use/relevance of the time-dependent covariates.

Firstly, if the available data on the study units are subject to
right-censoring, i.e., there is an `$A(t)$` indicator of being right-censored,
then the L-TMLE would use the time-dependent covariates to predict censoring
probabilities and thereby allow for dropout informed by the observed past of the
subject. In that case, knowledge of `$L(t)$` allows identification and, even
when censoring is independent, it would improve efficiency relative to an
estimator ignoring `$L(t)$`.

Suppose now that there is no censoring or missingness at all. In that case, one
can prove that the efficient influence curve for the average treatment effect
(ATE), say, of `$A$` on `$N(\tau)$` is the same as the efficient influence curve
for the reduced data structure `$L(0), A, N(\tau)$`. That proves that `$L(t)$`
cannot be used and are thereby irrelevant and should be ignored. However, it is
rare to not have right-censoring or missing visits over time, in which case they
play a fundamental role as mentioned above.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!