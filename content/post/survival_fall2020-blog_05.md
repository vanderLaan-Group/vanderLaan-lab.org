+++
title = "Estimating effects based upon community-level interventions and optimal interventions"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2020-12-04T17:53:00+00:00"
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

<u>**Question:**</u>

> Hi Mark,
>
> We have been discussing questions regarding community-based interventions and
> we would like to hear your input on the following three questions:
>
> 1. When we estimate the causal effects of community-based interventions, we
>    can use baseline variables to block the effect of the environment on the
>    outcome, so that we can change the problem into individual levels. However,
>    this method requires us to have the assumptions that in each community,
>    individuals within it are independent. So, if there is relatively strong
>    dependence between individuals within each community, what shall we do in
>    this case?
> 2. It is great that in community-level intervention studies with multiple
>    communities, the exclusion restriction that the effect of the environment
>    on the outcome is only through our measured W covariates is testable. You
>    have also previously written about the case where we are interested in the
>    causal effect of a community-level intervention/exposure on an outcome and
>    that intervention is implemented for everyone in the community at a given
>    time-point. In this case, we need to measure sufficient individual-level
>    covariates to block the effect of changes in the environment over time on
>    the outcome. For time-points 1 and 2 where `$Y(2)$` is the outcome after
>    the intervention and `$Y(1)$` is the outcome before the intervention, could
>    we test this exclusion restriction assumption by testing the hypothesis
>    that `$\mathbb{E}[Y(2) \mid A = 0, W(2) = w(2)] - \mathbb{E}[Y(1) \mid
>    A = 0, W(1) = w(1)] = 0$` for another, very similar community that did not
>    receive the intervention but that has a similar distribution of `$W$` and
>    similar environmental changes over time?
> 3. We usually try to estimate the effect of a treatment by controlling the
>    covariates, i.e., by estimating the difference in means given the
>    covariates. So, it is clear that the variability in the (observed)
>    covariates plays a fundamental role in our estimated treatment effect. So,
>    my question is: Can we talk about an optimal `$W$` (i.e., patient cohorts)
>    that would give us better treatment effect estimates? If we assume that we
>    already have some idea of the effect of `$W$` (e.g., a Phase-III RCT that
>    has results of previous phases). How can we select (design) an optimal
>    `$W$`? Can we test if this `$W$` was in fact optimal after observing the
>    data?
>
> Best,
> L.E., P.F.D., and Y.Z.

---

<u>**Answer:**</u>


Hi L.E., P.F.D., and Y.Z.,

Thanks for these questions. In regard to the first (1), it is about coming up
with a likelihood of the individuals within a community. It could be that there
is an observed network and we have conditional independence across individuals
as in our work on causal inference for networks. Or, easier, conditional on all
individual covariates `$(w_1, \ldots, w_N)$` we have that the individual
outcomes `$Y_i$` are independent. So in taht case, we want the individual
covariates to block both the environment but also explain dependence of
individual outcomes. Then, we could go after causal quantities that are not
averaging over a marginal distribution of `$W$`, but keep them an empirical
average (as sample average treatment effect type). I imagine that we can
generalize these identification results to any complex data collections
experiment within a community that still allows formal statistical inference
based on TMLE say, still dealing with the environment by blocking it through the
individual level covariates. If the likelihood of all individuals in
a community has no independence at all, then there is no statistical inference
possible for a fixed number of communities.

For the second of your three questions (2), I think you want to show that
`$\mathbb{E}_j(Y(2) \mid A = 0, W(2) = w)$` equals `$\mathbb{E}_{j'}(Y(2) \mid
A = 0, W(2) = w)$` as functions of `$w$` across different communities `$j$` and
`$j'$`, and similarly for `$Y(1)$`. In this way you test that the individual
outcome regression as function in `$w$` is constant across communities at both
stages `$A=0$` and `$A=1$`. It would tell us that the covariates are doing
a great job from community to community. It does not tell us that they do
a great job from one time point to next time point, but it would definitely give
one a lot of confidence. In fact, I would think it is a lot to ask that it is
constant across communities, while less to ask that it is constant across time
points when keeping `$A$` fixed. So it would have been nice to be able to
consider one community, keep `$A=0$` for two stages, take at both stages
a random sample of subjects of that community, and check if the regression
remains constant as a function of `$w$`. Similarly one could then do that for
that same community at `$A=1$`. That would directly test what we care about but
requires that type of design. Perfectly possible to carry out such a repeated
random sampling design across time, and changing the intervention half way.

Of course, there could be differences between the two communities that would not
guarantee that there would be no residual environmental confounding for the
intervention community. Is there a better potential test of the hypothesis that
the individual-level covariates can block the effect of changes in the
environment over time on the outcome?

For your third question (3), it sounds like you are interested in determining
a set `$S$`, so that `$\mathbb{E}(Y_1 - Y_0 \mid W \in S)$` is large/maximal? Of
course, a particular subset of interest is `$S = \{w: d_0(w) = 1 \}$`, where
`$d_0 = \mathbb{I}(\mathbb{E}(Y_1 - Y_0 \mid W = w) > 0)$` is the optimal rule.
This is the subpopulation that would be treated by the optimal rule. This
subpopulation avoids any cancellation for the `$\mathbb{E}Y_{d_0} -
\mathbb{E}Y_0$` parameter, i.e., there are no individual in that subset `$S$`
for which the treatment effect of the optimal rule is not better than control.
In addition, the optimal rule on this subpopulation is just a static
intervention `$A=1$`, so that, for this subgroup, it reduces to the ATE. This
subset includes all people where treatment is good, so it is maximally large set
which is also important when thinking about  a subpopulation: for example, a
drug company wants a maximal market for its drug.

One could also aim for a subset `$S$`, where `$\mathbb{E}(Y_1 - Y_0 \mid W \in
S)$` is larger. That would involve defining the subpopulation as all `$w$` for
which the conditional treatment effect exceeds a minimal value `$c$`, giving a
set `$S_c$`. `$S_c$` shrinks as `$c$` increases, while its effect increases as
well. So, I could imagine that plotting `$c$` against `$\mathbb{E}(Y_1 - Y_0
\mid W \in S_c)$` is of interest to make a good trade off in effect size
and size of population.

Any of these optimal sets `$S_0$` depend on `$P_0$`, `$S_0 = S(P_0)$`, and, in
this case, are determined by `$\mathbb{E}(Y_1 - Y_0 \mid W = w)$`. One could
learn this conditional treatment effect with super learner or HAL, even provide
inference and obtain a corresponding estimator `$\hat{S}$` of this optimal set
`$S(P_0)$`. Asymptotically, this estimated set will converge to the optimal set.
We could come up with measures on how good `$\hat{S}$` approximates `$S(P_0)$`,
and provide inference with TMLE of these measures of performance. So that would
be a formal way to determine how good `$\hat{S}$` is, answering partly your
question about testing if a particular choice is any good.

One can also imagine an adaptive design that involves adaptive sampling from an
adaptively selected population. Thus, at time `$t$`, one learns the best
estimate `$\hat{S}_t$`, and then samples a cluster of subjects from that
subpopulation `$\hat{S}_t$`, follow them up, and iterate this process. I believe
there is an article by Antoine Chambaz and Alex Luedtke on this type of adaptive
sampling design. Here, the action that is adapted is not treatment, or who to
test, but who to sample. Very much analogous to our current work on adaptive
surveillance systems for, e.g., the COVID-19 pandamic, where we adaptively set
the sampling probabilities for sampling subjects that will be tested. The latter
is harder due to all the dependence, while in the former we would still think of
subjects as i.i.d. so that only dependence is due to sampling probabilities
being a function of past data on previously sampled subjects.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!

