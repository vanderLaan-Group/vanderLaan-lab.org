+++
title = "Finite-sample properties of TML estimators"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2018-01-11T17:19:00+00:00"
categories = [
    "Resources",
    "Statistics",
    "Targeted Learning",
    "Q&A"
]
+++

_This post is part of our Q&A series._

A question from a graduate student in our Fall 2017 offering of "Survival
Analysis and Causality" at Berkeley:

## Question:

> Hi Mark,
>
> This may be an ill-defined question, but I was wondering, in the usual `$O
> = (W, A, Y)$` set-up, while TMLE has superior asymptotic properties over
> competing estimators like, say, the G-computation plug-in estimator or the
> IPTW estimator, are there specific instances where it is also guaranteed to
> have superior finite sample properties as well?
>
> Thanks for your time and apologies if this doesn't make sense.
>
> Cheers,
>
> N.S.

---

## Answer:

Hi N.S.,

Great question.

Yes, TMLE using a [well constructed] Super Learner is asymptotically efficient,
[a property] which states that for large sample sizes it will outperform other
competitors or be at least as good.

The Super Learner uses cross-validation, and that is really tailored to do the
right thing on the finite sample in our hands, but the choice of library matters
obviously, e.g., to at least include the highly adaptive lasso. Given a good
Super Learner, the targeting step can easily mess things up in finite samples.
That is why we have developed methods to guarantee consistency at a rate faster
than the critical `$n^{-\frac{1}{4}}$` for (1) data-adaptive truncation of the
treatment mechanism; (2) CV-TMLE to make the targeting step maximally robust by
not being affected by overfitting of the Super Learner; (3) one-step TMLE using
a universal least favorable submodel that again robustifies the targeting step
by using minimal data fitting to achieve the goal of solving the efficient
influence curve equation; and then there is (4) the risk of selecting the wrong
variables in the propensity score, which motivated the C-TMLE.

In addition, extra targeting can give extra properties to the TMLE such as
preserving asymptotic linearity under misspecification of one of the nuisance
parameters. We really like to develop a TMLE that does this all in an automated
manner and we can certainly implement such a TMLE, and we should.

Many of these choices also affect the G-computation and IPTW (e.g., choice of
Super Learning, truncation, variable selection for propensity score etc.).

Going back to your question: Yes, our goal should and will always be to develop
an estimator and inference (!) that is superior in finite samples. IPTW and
G-computation are not even asymptotically linear in nonparametric models when
using machine learning (and are of course inconsistent if used in parametric
models). Even though these estimators are thereby inferior theoretically, one
has no finite sample guarantee that the TMLE gives a more accurate point
estimator than G-computation (i.e., just Super Learner without targeting) for a
particular data set or even in MSE. But there is no point on betting on a choice
of estimator which has no theoretical basis since one is then relying on being
lucky and one can be terribly off. Instead the goal is to keep improving the
finite sample performance and robustness of a theoretically validated and
optimal estimator, both wrt estimation and inference.

Finite sample theory will generally be limited (the results or bounds are often
much too conservative to be useful for practical guidance, i.e., in the sense of
useful oracle inequalities). However, simulations across lots of data-generating
distributions in the model can provide great guidance. Recently, we are running
simulations involving random sampling of data distributions to make these
simulations honest so that they provide honest evaluation of different
estimation procedures, discover weak spots of the estimator and its inference,
and thereby guidance towards improvements.

Best,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!