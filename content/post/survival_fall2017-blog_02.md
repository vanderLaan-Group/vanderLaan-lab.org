+++
title = "Competing risks and non-pathwise differentiable parameters"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2017-11-29T11:30:00+00:00"
categories = [
    "Resources",
    "Statistics",
    "Targeted Learning",
    "Q&A"
]
+++

_This post is part of our Q&A series._

A question from two graduate students in our Fall 2017 offering of "Survival
Analysis and Causality" at Berkeley:

## Question:

> Hi Mark,
>
> Below are [two] questions [we thought might interest you]. Looking forward to
> your thoughts on these!
>
> Best,
>
> S.D. and I.M.
>
> 1. Most competing risk analyses assume that the competing risks are
>    independent of one another. What would be your advice on handling the same
>    style of survival data when the occurrence of one of the competing events
>    is informative of the occurrence of the other? In other words, what is
>    a reasonable approach when the independence of competing risks is an
>    unreasonable assumption?
>
> 2. If we assume we have some `$\Psi$` as a target parameter mapping, how
>    should we proceed if `$\Psi$` is not pathwise differentiable at `$P_0$`?
>    What if it is pathwise differentiable, but the canonical gradient is large?

---

## Answer:

Hi S.D. and I.M.,

Good questions.

Regarding (1), this is something we discussed in class as well. So, one has two
time-to-event times `$T_1$` and `$T_2$` and if `$T_1$` happens before `$T_2$` we
do not see `$T_2$` and vica-versa, say. Suppose one cares about estimating the
effect of treatment on survival function of `$T_1$`. Then one could view `$T_2$`
as a right-censoring variable and assume CAR, and thereby that the probability
of `$T_2$` being equal to `$t$` given `$T_2 \geq t$`, `$T_1$`, and the observed
past, is only a function of the observed past. That is what you refer to as
making an independence assumption, but keep in mind it is a conditional
independence assumption so that the richer the observed history the more
realistic this assumption will be.

As I argued, often it is not that sensible to view `$T_2$` as a censoring time,
e.g., time until death. In that case, I prefer to view the full-data involving
tracking `$X_1(t) = \mathbb{I}(\text{min}(T_1, T_2) \leq t, T_1 < T_2)$` and
`$X_2(t) = \mathbb{I}(\text{min}(T_1, T_2) \leq t, T_2 < T_1)$` and look at both
of these outcome processes. As an example, one may evaluate the causal effect of
treatment on `$X_1(t_0)$` and also on `$X_2(t_0)$`. One could think of
`$(X_1(t_0), X_2(t_0))$` as a discrete variable with three possible values and
estimate the probability distribution of this discrete variable under different
treatment regimens and compare treatments accordingly.

It is not that different from having two clinical outcomes in an RCT and view
both as very important and the evaluation of a treatment will have to consider
both.

It is not just that the CAR assumption could be unreasonable, but even when may
be reasonable, one might still prefer to view $X_1$, $X_2$ as the full data
process and not one of them as a nuisance/censoring.

In regard to (2), I presume you mean a target parameter that is _generally not
pathwise differentiable_ at any distribution $P$ in your model. That is
different from a generally pathwise differentiable target parameter that is not
pathwise differentiable at some $P$ in your model. The latter one deals with by
understanding the model assumptions that make it pathwise differentiable, e.g.,
one might tneed a bound on the propensity score/treatment mechanism/censoring
mechanism. These positivity type assumptions then need to be evaluated and if
they do not hold in our data set, then we can move towards target parameters that
will be supported by the data, e.g think about realistic dynamic or stochastic
treatment rules versus static rules that are not supported by the data.

As you say, even when it is pathwise differentiable, the confidence intervals
might be very wide, so that this is not that different. Having a wide confidence
interval is itself not bad, it is what it is, but one might prefer to also
answer some questions that are well supported by the data. One approach I like
is to define a data adaptive target parameter that maps the data into a target
parameter that is well supported by the data. For example, one might learn a
stochastic or dynamic intervention that approximates the desired (e.g.) static
intervention but that is actually supported by the data. In that manner, one has
no price to pay in terms of multiple testing which would be the case if one
instead defines a large collection of target parameters _a priori_.

Statistical inference with non-pathwise differentiable target parameters such as
a dose response curve `$\mathbb{E}Y(a)$` for a continuous treatment dose `$a$`,
based on `$O = (W, A, Y)$`, nonparametric model, is challenging. We have
developed methods for that purpose (work with Aurelien Bibaut and a chapter in
the new upcoming Targeted Learning book). They approximate the curve by a smooth
approximation that is pathwise differentiable, estimate it with CV-TMLE, data
adaptively select amount of smoothing optimally trading off bias and variance,
and establishing that this CV-TMLE will still be asymptotically normal so that
inference is available based on the normal limit distribution (with lower rates
of convergence than `$\frac{1}{\sqrt{n}}$`). This is exciting  and important
work which will occupy us for years to come. Apparently, we can utilize all our
advances in the targeted learning of pathwise differentiable target parameters
for this purpose, we just target a smooth approximation, and tune that
approximation appropriately, while preserving the asymptotic normality with mean
zero (so that bias can be ignored in generating confidence interval...)

Best,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!