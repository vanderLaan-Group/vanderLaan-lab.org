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
> __Question 1, Practical positivity:__
> Is there a recommended procedure for deciding truncation threshold with
> respect to shifts in the stochastic treatment regimes framework?  Further,
> what might be an acceptable proportion of thresholded observations (or
> distribution of g_delta / g) such that we are comfortable estimating a shift
> magnitude?  In this sense one defines a reasonable grid to estimate shift
> values or an MSM.
>
> __Question 2, Other form of stochastic treatment:__
> We are interested in what stochastic treatment regime is of most interest to
> your group to derive estimation for and program into tmle3shift following the
> work you’ve done for an additive shift.  Practically, are there situations
> where it might be optimal to have a different function implemented in
> tmle3shift, such as a multiplicative scaling or nonlinear function of a?  Can
> you data-adaptively select a regime that maximizes the causal parameter?
>
> __Question 3, Combining stochastic and optimal individualized treatment:__
> Somewhat playing off of the end of our last question, if you are interested in
> estimating optimal continuous treatment values for different individuals or
> subgroups of interest from the population, can you leverage the estimation of
> impact of different shift values (or functions) on your subgroups of interest?
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
There  are two ways to think about this. If one is aiming for EY_{g*} for
a particular g*, then truncation of g*/g by some constant M tells us that the
TMLE for this target parameter is indexed by M and it represents a tuning
parameter for this M-specific TMLE, psi_{n,M}. In that case, one is concerned
with  selecting M to optimize MSE of psi_{n,M} w.r.t. psi_0 under constraint
that the bias should be smaller than the standard error by a factor like
1/max(10,log n) so that inference is not affected by the bias. For that type of
problem we have been using so called Lepski's method, see for example chapter 25
in 2018 Targeted Learning book. This comes down to looking for a plateau in
M--psi_n(M) where plateau corresponds with the smallest M at which the d/dM
psi_{n,M} is of same order (e.g. 2 times) as d/dM sigma_{n,M} (before that M the
change in estimate observed by increasing M outweights the increase in standard
error), where sigma_{n,M} is standard error of the TMLE psi_{n,M}. It also
corresponds with looking for first local optimum of the lower or upper bound of
a confidence interval based on psi_{n,M}. This is a very nice powerful method
for tuning M. One can estimate the variance of psi_{n,M} with the influence
curve of TMLE using this truncation M, where we note that the EIC will have
larger and larger variance as M increases.

On the other hand, one might also simply view the selection of delta and thereby
M as a way to define a stochastic intervention and accept that we care about the
mean outcome under that selected stochastic intervention. In  that case it is an
example of data adaptive target parameter but it was only based on the
likelihood of g so that no CV-TMLE is needed to correct for this.

When making this selection one still has in mind a certain M to bound
`$g^{\star}_{delta}/g<M$`. Then your question concerns what is a good way to select M.
I dont think we have a very satisfying answer  yet on that question and I think
we should think more about that. The question is what kind of criterion makes
sense.

As one increases M one will obtain wider confidence intervals so one somehow
needs to decide what kind of variance is still acceptable, i.e is there
a minimal support by the data we want about our target parameter. Maybe a good
strategy is to report the results for all M so that the reader can honestly
evaluate what impact of M is. We could then think of providing a simultaneous
confidence interval to correct for having seen all these results. Since the
TMLEs psi_{n,M} are very correlated the adjustment will not be bad and of course
we can make the width proportional to the standard error so that the large
M confidence intervals are not dominating the other ones.

__Question 2:__
Yes, we definitely need more interesting shifts or more generally choices of
d(A,W). Including percentage increases relative to A. We really need a function
that can take as input any conditional distribution g*(A|W), i.e. any stochastic
intervention, and the program then computes TMLEs of EY_{g*}.  This then
includes shift interventions but the user can think of their own choices. The
TMLE applies to any choice g* so this is not hard. The user could also specify
a family g*_{delta} and indicate tuning parameters of this intevention that
should be optimized for sake of EY_{g*_{delta}} or that should be pushed to
maximal allowed level of g*/g.

__Question 3:__
Yes we could optimize EY_{g*_{delta}} as a function of the shift parameter or
optimize the uppeor bound of the confidence interval for this parameter so that
it also takes into accountthe increased standard error. I prefer the latter.
This applies to any user supplied parametric family or nonparametric family of
candidate stochastic interventions. Optimal rules for continuous treatments is
an important topic itself and gets into non pathwise differentiable target
parameters. We have some work on it with Alex Luedtke, also Kosorok etc,  but
more to do for sure.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
