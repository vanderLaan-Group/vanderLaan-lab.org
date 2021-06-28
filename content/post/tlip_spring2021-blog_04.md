+++
title = "Longitudinal Causal Inference with Left-Censoring and Left-Truncation"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2021-05-01T17:23:00+00:00"
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
> As epidemiologists, we wish to study the relationship between time-varying
> exposure and disease progression over time. A natural choice of study design
> would be the longitudinal cohort study. In prospective cohorts, participants
> are not selected from existing data, but enrolled during some enrollment
> period. When establishing a retrospective cohort, participants are selected
> from data bounded within some calendar period. For simplicity, we may also
> call this an enrollment period. In both cases, follow up begins once
> eligibility criteria are met: on the index date. We say that an individual is
> left censored if they have experienced the outcome of interest prior to their
> index date, but entered follow-up nonetheless. We say they are left truncated
> if they experience the outcome prior to their index date and as a result, fail
> to enter follow-up.
>
> Here, we present two scenarios in which we wish to minimize bias due to issues
> such as left censoring and left truncation. We would very much appreciate your
> thoughts on the matter!
>
> __Scenario 1__:
>
> We wish to establish a retrospective study using data from a large
> longitudinal database (possibly from an integrated healthcare system) where
> the completeness of data varies over time. Among individuals represented in
> the time-bounded database, eligibility must be assessed using possibly
> incomplete data regarding exposure and outcome. The data are more complete in
> later years; we are therefore more likely to miss diagnostic criteria,
> exposure information, and outcome data among records further back in calendar
> time. Ineligible individuals may have been included, and eligible individuals
> may have been excluded simply because of missing exclusion/inclusion criteria
> data. Furthermore, the population alive later would have more complete data,
> but would exclude left truncated individuals.
>
> How might we choose an interval of calendar time to query the database and/or
> define cohort inclusion criteria/index date in a way that minimizes bias from
> inappropriately including/excluding person-time while maximizing the size of
> the study population? How might this work specifically under a causal
> framework? How can we use observed data to inform our decision making in this
> scenario?
>
> __Scenario 2__:
>
> In an ideal prospective cohort study, we know participants' disease status at
> enrollment. However we often do not know disease status until follow-up
> starts, after enrollment. Participants may experience disease onset or death
> due to that disease between enrollment and start of follow-up. We may
> ascertain mortality due to the disease for those who died in this liminal
> time, but have no knowledge of disease onset. The population followed-up in
> our cohort includes only those who survived to the start of follow-up, so they
> are presumably healthier than the total enrolled population. How can we use
> our knowledge of mortality due to that disease to estimate a causal parameter
> for disease onset for the entire enrolled population? We were curious to get
> your feedback on the possibility of conceptualizing this as
> a prediction/imputation problem in which we think about "risk of being left
> censored at start of follow-up" as a missing baseline covariate.
> Alternatively, would it be possible to develop a causal approach possibly
> involving an estimated probability of experiencing the outcome prior to the
> start of follow up conditional on covariates? What are the main challenges to
> causal identification?
>
>
> Best,
> K.C. and N.N.

---

__Answer:__

Hi K.C. and N.N.,

Thank you for the excellent question.

This concerns two important types of complications in a data structure. The
first one is left-censoring but no truncation. In that case, we still obtaine
a random sample from our target population and we aim to observe a longitudinal
data structure say `$X(t): t \geq 0$` for the subject, but it is left-censored
so that we only observe `$(C_l, (X(t): t \geq C_l)$` for some `$C_l$`. Of
course, on top of this, we might have right-censoring as well as a causal
inference problem/missing data on counterfactual outcome processes.

One then likes to assume coarsening-at-random (CAR) on `$C_l$` given `$X$`. This
corresponds with assuming that the `$\mathbb{P}(C_l = c \mid X)$` only depends
on `$(X(t): t > c)$`, i.e., the observed `$o$` when `$C_l=c$`.

Once we have that, then the likelihood becomes `$\mathbb{P}(X = (X(t): t \geq
c_l))$` for a single observation `$(C_l = c_l, (X(t): t \geq C_l)$`. So,
a product over time starting at `$c_l$` of `$\mathbb{P}(X(t) \mid
\bar{X}(t-1))$`. If for the full-data target parameter `$\Psi^F(P_X)$` we need
to understand the distribution of `$X(t)$` also close to `$t=0$`, then we would
need that `$C_l = 0$` has positive probability or at least that small `$C_l$`
occur. For example, maybe you need to observe a baseline treatment and we then
certainly need a proportion of subjects for which we actually know what
treatment they received at the origin. Estimators such as TMLE, in essence, will
be imputing left-censored tails based on learning from the units that have the
left-tail of `$X(t)$` observed. Either way, we can develop a TMLE for such
a problem.

Anyway, this is an important problem, and some work definitely needs to be done
-- a good motivational study would be excellent starting point for an article.
It fits in the world of censored data, assuming CAR presumably if at all
possible, and still carrying out causal inference for such left- and
right-censored longitudinal data structures. In certain cases where we
just can't identify what happens in the early part of the `$X(t)$` distribution,
we have to change the questions to ones that can be answered based on the data.
If there is some other data set that allows to learn about the initial part of
the `$X(t)$` distribution, maybe that can be used to make part of the model
assumption that this first part was generated from some known distribution. But
such mixtures often lack a lot of information, making estimation hard.

Truncation is about biased sampling. So, now we have a `$C_l$` (time from
initial zero origin of subject to start of study, at which time the subject
would get enrolled) and a `$T$` (e.g., time until death), and we sample `$X$`
from `$X$` conditional on `$T > C_l$`. There is certainly work on this topic.
I also have an article on it: bivariate truncated survival data which provides
insight in this type of problem. I believe there is a way to map the observed
data distribution, i.e., the conditional distribution of `$X$` given `$T > C_l$`
into the desired distribution of `$X$`, under assumptions on `$C_l$` independent
of `$X$` conditional on observed variables, and that could then be used to
define the target estimands.

Again, these are important types of problems for the field of causal inference,
since left-truncation and left-censoring have always been intrinsic parts of the
survival analysis literature but have not (yet) been integrated in the modern
TMLE causal inference-type estimation problems.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
