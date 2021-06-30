+++
title = "Stochastic treatment regimes in practice"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2021-05-01T15:54:00+00:00"
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
> We were discussing practical implementations of stochastic treatment regimes
> and came up with the following questions we would like to hear your thoughts
> about.
>
> __Question 1 (Practical positivity):__
> Is there a recommended procedure for deciding truncation threshold with
> respect to shifts in the framework of stochastic treatment regimes? Further,
> what might be an acceptable proportion of thresholded observations (or the
> distribution of `$g_{\delta}/g$`) such that we are comfortable estimating
> a shift magnitude? In this sense, one defines a reasonable grid to estimate
> shift values or a marginal structural model (MSM).
>
> __Question 2 (Other form of stochastic treatment):__
> We are interested in what stochastic treatment regime is of most interest to
> your group to derive estimation for and program into
> [`tmle3shift`](https://github.com/tlverse/tmle3shift), following the work
> you've done for an additive shift. Practically, are there situations where it
> might be optimal to have a different function implemented in `tmle3shift`,
> such as a multiplicative scaling or nonlinear functions of `$A$`?  Can you
> data-adaptively select a regime that maximizes the causal parameter?
>
> __Question 3 (Combining stochastic and optimal individualized treatment):__
> Somewhat playing off of the end of our last question, if you are interested in
> estimating optimal continuous treatment values for different individuals or
> subgroups of interest from the population, can you leverage the estimation of
> the impact of different shift values (or functions) on your subgroups of
> interest?
>
>
> Best,
>
> A.M. and S.F.

---

## Answer:

Hi A.M. and S.F.,

Thank you for your excellent questions.

__Question 1:__
There  are two ways to think about this. If one is aiming for
`$\mathbb{E}Y_{g^{\star}}$` for a particular `$g^{\star}$`, then truncation of
`$g^{\star}/g$` by some constant `$M$` tells us that the TMLE for this target
parameter is indexed by `$M$`, and that it represents a tuning parameter for
this `$M$`-specific TMLE, `$psi_{n,M}$`. In that case, one is concerned with
selecting `$M$` to optimize the MSE of `$\psi_{n, M}$` with respect to
`$\psi_0$` under the constraint that the bias should be smaller than the
standard error by a factor of approximately `$1 / \max(10, \log(n))$`, which
ensures that statistical inference (i.e., confidence interval construction) is
not affected by the bias. For that type of problem, we have been using a variant
of Lepski's method -- see, for example, chapter 25 of the 2018 book [_Targeted
Learning in Data Science_](https://www.springer.com/us/book/9783319653037).

This comes down to looking for a plateau of `$\psi_{n,M}$` in `$M$`, where the
plateau corresponds with the smallest `$M$` at which `$d/dM \psi_{n,M}$` is of
the same order (e.g., 2 times) as `$d/dM \sigma_{n,M}$`, where `$\sigma_{n,M}$`
is the standard error of the TMLE `$\psi_{n,M}$`. That is, the plateau can be
thought of as appearing before that value of `$M$` at which the change in the
point estimate that occurs as a result of increasing `$M$` outweighs the
increase in its standard error. It also corresponds with looking for the first
local optimum of the lower or upper bound of a confidence interval of
`$\psi_{n,M}$`. This is a powerful method for tuning `$M$`. One can estimate the
variance of `$\psi_{n,M}$` with the efficient influence curve (EIC) of TMLE
using this to truncate `$M$`, where we note that the EIC will have larger and
larger variance as `$M$` increases.

On the other hand, one might also simply view the selection of the shift
`$\delta$`, and thereby `$M$`, as a way to define a stochastic intervention and
accept that we care about the mean outcome under that selected stochastic
intervention. In  that case, this becomes an example of a data-adaptive target
parameter, but it was only based on the likelihood of `$g$` (making it
outcome-blind), so that no CV-TMLE is needed to correct for overfitting.

When making this selection, one still has to keep in mind a certain `$M$` to
bound `$g^{\star}_{\delta}/g < M$`. Then, your question concerns what a good way
would be to select `$M$`. I don't think we have a very satisfying answer yet on
that question, and I think we should think more about that. The question comes
down to what kind of criterion makes sense.

As one increases `$M$`, one will obtain wider confidence intervals, so one
somehow needs to decide what kind of variance is still acceptable, i.e., is
there enough support in the data we would want about our target parameter? Maybe
a good strategy is to report the results for all `$M$`, so that the reader can
honestly evaluate what the impact of `$M$` really is. We could then think of
providing a simultaneous confidence band to correct for having seen all of these
results. Since the TMLEs `$\psi_{n,M}$` are very correlated, the adjustment will
not be too costly, and, of course, we can make the width proportional to the
standard error so that the confidence intervals for large `$M$` are not
dominating the other ones.

__Question 2:__
Yes, we definitely need more interesting shifts or more general choices of
`$d(A,W)$`, including percentage increases relative to `$A$`. We really need
a function that can take as input any conditional distribution `$g^{\star}(A
\mid W)$`, i.e., any stochastic intervention, with the program then computing
TMLEs of `$\mathbb{E} Y_{g^{\star}}$`. This then includes shift interventions,
but the user can think of their own choices too. The TMLE applies to any choice
`$g^{\star}$`, so this is not hard. The user could also specify a family
`$g^{\star}_{\delta}$` and indicate tuning parameters of this intevention that
should be optimized for the sake of `$\mathbb{E} Y_{g^{\star}_{\delta}}$` or
that should be pushed to the maximal allowed level of the ratio of the
post-intervention and observed treatment densities, `$g^{\star}/g$`.

__Question 3:__
Yes, we could optimize `$\mathbb{E} Y_{g^{\star}_{\delta}}$` as a function of
the shift parameter or optimize the upper bound of the confidence interval for
this parameter so that it also takes into account the increased standard error.
I prefer the latter. This applies to any user-supplied parametric family or
nonparametric family of candidate stochastic interventions. Optimal rules for
continuous-valued treatments is an important topic itself and gets into working
with non-pathwise differentiable target parameters. We have some work on it with
[Alex Luedtke](https://www.alexluedtke.com/), also [Michael
Kosorok](https://mkosorok.web.unc.edu/) and others, but more to do for sure.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
