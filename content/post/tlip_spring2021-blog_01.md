+++
title = "Adaptive Designs and Continuous Treatments"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2021-05-01T15:00:00+00:00"
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
> You've got me thinking about selecting optimal experiments in the context of
> shift interventions. For the example we talked about in class, in order to
> avoid positivity violations, we define shift interventions such that an
> individual's value of the intervention node `$A$` is shifted by a specified
> amount `$\delta$` unless there is no support for such a shift based on the
> covariates `$W$`, in which case `$A$` is shifted to the maximum value
> available for that `$W$`. This makes me think that there may be situations
> where there is a health systems issue that causes a positivity violation that
> prevents the full shift `$\delta$` from being evaluated for some people. For
> example, maybe your intervention is based on increasing the number of visits
> to a health center for a given problem, but people who either live a certain
> distance away from the health center or whose insurance has a high copay for
> the visits are very unlikely to participate in the number of proposed sessions
> for logistical and financial reasons in your dataset. You could potentially
> design multiple experiments where you intervene not only on `$A$` but also on
> the `$W$` covariate that is preventing certain people from having the full
> shift `$\delta$`. This could be a practically interesting question as in what
> would be the mean outcome under the shifted `$A$` if you also intervened to
> decrease peoples' distance to a health center (i.e., by offering the
> intervention at more health centers). I would think then that you could shift
> `$A$` further for more people -- either by `$\delta$` or the maximum of
> `$\delta$` for people living within a short distance from the health center
> based on the rest of the `$W$` covariates. As long as the probability of this
> joint intervention given the rest of the covariates is not very small, then
> I would think the effect of such an intervention would be identifiable. Could
> you then define a data-adaptive target parameter as the mean of the outcome
> under the intervention to both shift `$A$` and intervene on whichever of the
> `$W$` covariates leads to the largest mean shift in `$A$`? This selection of
> the `$W$` intervention would not need to depend on the outcome. I wonder if
> this could be a means of both designing and evaluating the potential impact of
> interventions aimed at improving access to care.
>
> Thanks,
>
> L.E.D.

---

__Answer:__

Hi L.E.D.,

Thank you for the interesting question.

For concreteness, we observe `$O = (W, A, Y)$` where `$A$` is a continuous or
discrete-ordered variable (number of visits to healthcare per year). In
addition, there are some key variables in `$W$`, say `$V$`, that are known to be
important for achieving a high level of `$A$`. You then talk about an
intervention on `$V$` through another `$A_1$` variable making it easier to
achieve `$A$` for the person. It is like providing another incentive `$A_1$`
such as money, travel or bringing the service closer to them. Normally, `$W$`
has an arrow going into `$A_1$`, but this `$A_2$` is pre-`$V$`. So, the ordering
becomes `$(W, A_1, V, A_2, Y)$`. So, we now have a longitudinal two-timepoint
intervention data structure, and we are interested in two-timepoint
interventions `$(g^{\star}_1, g^{\star}_2)$` that optimize `$\mathbb{E}
Y_{g^{\star}}$`. You indicate a situation in which the ability to increase $A_2
= d(V)$ or $g^{\star}_{2 \mid V}$, to high levels is very much affected by how
`$A_1$` is set and thereby `$V$` is realized. Just as in cancer trials where we
h ave two-stage treatment regimen, first-line and second-line treatment.

If we restrict to shift interventions on `$A_2$`, then, depending on `$A_1$`, we
can push the shift further. So, we could define `$g^{\star}_{2,\delta}$` with
`$\delta$` chosen maximally in response to the value of `$V$`. Even though we
fix `$g^{\star}_2$` we still have flexibility in what variable among the `$V$`
components we intervene on. So, we really have a set `$(A_{1k}: k)$` of
treatment nodes for `$A_1$`, where `$k$` identifies which `$V_k$` we target with
this intervention.

So, we have `$(W, (A_1(k): k), V, A_2, Y)$`. We now want to consider, say, rules
`$d_{1k}(W)$` that intervene on `$A_1(k)$` with a rule targeting `$V_k$`. We
could consider regimens that only intervene on a single `$A_1(k)$`, and we are
then interested in not only optimizing `$d_{1k}(W)$` for a given `$k$` but also
in selecting among `$k$`, or, maybe even among joint interventions that target
subsets of `$A_1(k)$`. Given an observational data set, we could estimate the
mean outcome among a user-supplied class of such interventions or define optimal
rules `$d_{1k,P_0}(W)$` for each `$k$` and thus also an single optimal rule
`$d_0$` for `$A_1$`. We then want to learn that rule and, given that estimated
rule, we care about the mean outcome with respect to using that rule on `$A_1$`
and the corresponding shift `$g^{\star}_{2}$` on `$A_2$`. So, this falls in the
category of learning joint interventions from data and then performing a TMLE
for the corresponding target parameter, thus using CV-TMLE.

It is very much like making the choice of interventin on `$A_2$` a deterministic
function of the `$A_1$` intervention and then defining the optimal rule for
`$A_1$`. It avoids the notion of sequentially defined optimal rules, as in our
work with [Alex Luedtke](https://www.alexluedtke.com/) by making the second
treatment a deterministic function of the first -- quite interesting and
sensible. Overall, this certainly falls in the category of data-adaptive target
parameters defined by searching among subsets of variables of `$V$` and
corresponding rules based on data, using CV-TMLE as a general method for
inference.

If indeed one can already do some learning without outcome data then that
learning comes "for free," so that would not be bad either. However, I think you
have in mind that we can investigate how `$A_1$` affects `$V(A_1)$` but that is
in essence looking at the outcome, so it is informative since `$V$` is just one
of the time-dependent markers on the pathway to the final outcome. Maybe you are
getting at the point that one could learn an optimal rule or so for targeting
`$V$` only using data `$(W, A_1, V)$`, and, after having run that and set the
intervention on `$A_1$`, we consider the next problem of learning the rule for
`$A_2$` based on `$(W, V), A_2, Y$`. If indeed it is relatively cheap to measure
`$V$` and impact of `$A_1$` on `$V$`, then that would be an effective
experiment.

You also appear to talk about running actual trials. This falls then in the
category of adaptive sequential designs (i.e., reinforcement learning), such as
targeted adaptive design learning of the optimal rule. We have work on that,
including recent work by Aurelien Bibaut. There is still a lot of progress to be
made there, see also my lectures notes from the class on adaptive design (Public
Health 243D), covering topics such as being able to adapt to surrogate outcomes
so that one does not need to wait until outcomes are measured before being able
to adapt the randomization probabilities for next enrolled subject. A lot of
exciting and important research to do there.

Great questions.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
