+++
title = "Estimating causal effects with instrumental variables in survival analysis"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2020-12-03T17:27:00+00:00"
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
> In survival analysis, what methods should we use to estimate counterfactuals
> and causal effect if the conditional independence assumption is violated? For
> instance, the instrumental variable method in econometrics and Mendelian
> randomization in biostatistics deal with the unmeasured confounding problem.
> How should we use TMLE to improve efficiency in this case?
>
> Best,
>
> H.J. and S.L.

---

## Answer:

Hi H.J. and S.L.,

All we need to do is to obtain the identification of the counterfactual survival
curve as a function of the observed data distribution under the assumption of
having an instrumental variable. For example, we observe
`$W, R, A, \min(T, C), \mathbb{I}(T \leq C)$`, where `$R$` is the instrument and
`$A$` the actual treatment. The literature provides such identification results.
We then have an estimand. For example, if `$A$` is binary and `$R$` is binary,
then under assumptions, we have the following.

`$\mathbb{P}(T_1 > t_0) - \mathbb{P}(T_0 > t_0) = [\mathbb{E}_W S(t_0 \mid
R = 1, W) - \mathbb{E}_W S(t_0 \mid R = 0, W)] / [\mathbb{E}_W \mathbb{E}(A
\mid R = 1, W) - \mathbb{E}_W \mathbb{E}(A\ mid R = 0, W)]$`. We can develop a
TMLE for that estimand. A TMLE of the numerator we already have. Similarly, the
denominator is just an average treatment effect (ATE), so we have a TMLE for
that as well. Going further, one can even make sure to use a single TMLE instead
of two separate TMLEs, so that the resulting plug-in estimator of this estimand
is compatible with a single data distribution. There is some work by my former
student Boriska Toth on TMLE with instrumental variables for the data structure
`$(W,R,A,Y)$`, and also by my former postdoc Kara Rudolph. In particular, in our
work with Kara, we have such a fully compatible TMLE.

Keep in mind, TMLE is a pure estimation problem defined by the statistical model
and target estimand. So, causal assumptions are only relevant to the point that
they generate different estimands. Once we have the estimand, the TMLE does not
care about these underlying assumptions, except if they affect the statistical
model.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!