+++
title = "Positivity assumption violations and TMLE for longitudinal data with many time-varying covariates"
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
> For longitudinal data such as `$O=(L_0,A_0,Y_0,L_1,A_1,Y_1,L_2,A_2,Y_2,\ldots
> )$`, we can use G-computation formula with sequential regression method if we
> treat time `$t$` as discrete variable. And you also mentioned that there are
> more general methods which can deal with the case when `$t$` is continuous.
> However, it seems there is a dilemma that the positivity assumption will be
> threatened more as we create more time points. I wonder how this positivity
> violation issue can be fixed in those methods? Thanks!
>
> H.L., Y.L. and W.Z.

---

<u>**Answer:**</u>

Dear H.L., Y.L. and W.Z.,

Yes, let's say that the longitudinal data structure is collected on a daily
basis.

That means every day we have an indicator of the subject being monitored, and if
monitored, we measure various time dependent covariates. Each day we also have
an indicator of the subject being assigned a treatment change (possibly a zero
change, but nonetheless, it is a moment where treatment is random), and if
treated, then we will measure the actual assigned treatment. It might be the
case that these two indicators are the same: i.e., when a subject is monitored
it is also subject to a treatment change.

Note that such a data set could be represented with a random number of lines for
each subject, one line for each time the person is monitored (i.e., a change of
one or more of it's variables occurs). It is common that the actual times at
which changes occur within a single subject is limited, so that the size of this
data set might be fine.

One now needs to define a causal question, i.e., a choice of intervention on the
intervention nodes. Firstly, if a subject is only treated at a monitoring time,
one might define a dynamic treatment regimen that is only active at these
monitoring times. Such a dynamic treatment can be specified in `ltmle()`, where
the indicator of being monitored is the covariate used by the dynamic treatment
rule. The inverse weighting in the efficient influence curve (and thus in an
IPTW or TMLE targeting step) will now involve a product over these monitoring
times of corresponding conditional distribution of the intervention at that time
given its past. So the positivity assumption is driven by the actual active
intervention times, even though these can vary across subjects. So this is one
important way the positivity assumption is controlled.

To fit the treatment and censoring mechanism one could use a pooled logistic
regression/ super learner that pools across all time points (treating time as
another covariate). So that is how one would deal with fact that the data is
essentially continuous in time.

A TMLE would also require estimation of the relevant factors of the likelihood
(beyond censoring and treatment mechanism). If one would carry out targeted
maximum likelihood, then one would also estimate these conditional densities of
`$L(t)$` and `$Y(t)$` given the past by pooling across time. We implemented such
a TMLE in work with Ori Stitelman (see e.g., van der Laan, 2010, parts I and II
and related articles). The current sequential regression TMLE aims to avoid
estimation of conditional densities, but does not naturally pool across time (it
fits a separate regression at each time point). Similarly, the targeting step is
done separately at each time point. So contrary to this earlier targeted maximum
likelihood estimator, the sequential regression TMLE relies on discrete time,
and one might experience it being sensitive to the number of time points chosen
(although in [article with Jordan Brooks on causal effect of warfarin on stroke
with Kaiser
data](https://www.degruyter.com/view/j/jci.2013.1.issue-2/jci-2013-0001/jci-2013-0001.pdf)
we did an longitudinal TMLE on daily basis and obtained robust and good results,
and see also [article by Oleg Sofrygin with Kaiser
data](https://doi.org/10.1002/sim.8164), so it appears that for various
statistical estimation problems it is less problematic than one mathematically
expects).

Therefore, I am still a fan of the targeted maximum likelihood type TMLE as in
our original work around 2010, but one wants to reduce the dimension of `$L(t)$`
to make it do-able (see [*A General Implementation of TMLE for Longitudinal Data
Applied to Causal Inference in Survival
Analysis*](https://www.degruyter.com/doi/10.1515/1557-4679.1334)). In recent
work with Helene Rijtgaard we have worked on a new TMLE that is a mix of
regression and targeted maximum likelihood and handles continuous time.

Suppose now that a subject is at risk of a change in treatment at any time
point, or many time points. In that case, one could decide to define a more
limited set of intervention nodes such as a subset of the potential treatment
change times, and/or one could define a stochastic intervention (which becomes
deterministic if certain event occurs, say) that follows the observed treatment
distribution when acceptable and only enforces an intervention when it deviates
too much from a desired intervention.

In this case, one aims to assess the causal impact of intervening only every
month or so, or, when using such a stochastic intervention it evaluates the
impact of following a stochastic intervention that only intervenes on natural
treatment a limited number of times. In this manner, we can still define
interesting longitudinal interventions with limited change points so that the
positivity assumption remains controlled.

An exception is if the treatment process is very simple in the sense that it
will only jump once (when to start, when to switch). In that case, even though
it can jump at any point in time, the positivity is not much affected by the
total number of time points; essentially, the probability of following
a particular regimen is a product integral of 1 minus the an intensity of the
counting process `$A(t)$` that only jumps once, and behaves as a conditional
survivor function.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
