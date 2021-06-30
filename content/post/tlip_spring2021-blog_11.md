+++
title = "Causal mediation analysis for exposure mixtures"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2021-05-02T12:52:00+00:00"
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
> After the lectures on [`tmle3shift`](https://github.com/tlverse/tmle3shift)
> and [`tmle3mediate`](https://github.com/tlverse/tmle3mediate), we're wondering
> if a different procedure for mediation analysis could work. Consider a
> data-generating system for `$O = (W, A, Z, Y)$`, where `$W$` represents three
> binary covariates, `$A$` is a binary exposure of interest, `$Z$` are three
> binary mediators, and `$Y$` is a continuous outcome. Fitting a Super Learner
> for `$\mathbb{E}(Y \mid A,W)$` (ignoring `$Z$`), and then getting predictions
> through this fit for the counterfactuals `$\mathbb{E}(Y \mid A=1, W)$` and
> `$\mathbb{E}(Y \mid A=0, W)$`, we can construct
> `$\mathbb{E}(Y \mid A=1, W) - \mathbb{E}(Y \mid A=0, W)$`, which represents
> the total effect. Let's call this model `$\mathcal{M}_{\text{total}}$`. Now,
> consider another model fit for `$\mathbb{E}(Y \mid A, Z, W)$` and conduct the
> same procedure. The model now controls for the mediators, and, by
> deterministically setting `$A = 1$` and `$A = 0$`, we break the connection
> with the mediatorsand therefore can calculate the direct effect; call this
> model `$\mathcal{M}_{\text{NDE}}$`. The natural indirect effect can be
> calculated as `$\mathcal{M}_{\text{total}} - \mathcal{M}_{\text{NDE}}$`. This
> works in simulation experiments (using simulated data for `tmle3mediate`). Of
> course, it's kind of bullshit because we have `tmle3mediate`, which can
> calculate the NDE/NIE for a binary exposure; this two-model approach is just
> borrowed from the GLM-based methods developed ages ago, but it uses Super
> Learning instead. However, there are some interesting scenarios where this
> might be useful. For example, when there are multiple exposure. So, consider
> we have `$A = (A_1, A_2, A_3)$`, all binary. The same procedure could be
> followed by setting all `$A$`s to 1 and 0 and calculating the NDE and NIE in
> this way. Could this serve as a non-parametric approach to mediation analysis
> for mixtures? Of course, a bootstrap procedure would need to be done to derive
> confidence intervals. Similarly, now consider `$A$` is continuous; the same
> procedure could be conducted but now shifting `$A$` by some `$\delta$`. For
> example, the NDE and NIE can be calculated for all chemicals of a joint
> exposure if the chemical exposures were all shifted by 10%. This seems more
> intuitive, especially for researchers, than shifting the propensity, as with
> `tmle3shift`. Now, taken one step further, given the unique properties of
> highly adaptive lasso (HAL), is it possible to do a similar procedure but with
> the HAL basis functions? Meaning that a HAL is fit to the data `$\mathbb{E}(Y
> \mid A,Z,W)$`. Is it possible to then make predictions through the model to
> calculate the NDE and NIE by shifting certain basis functions? If so, given
> the unique properties of HAL (can be used for statistical inference without
> TMLE), could one get around the non-parametric bootstrap?
>
> Best,
>
> D.M. and J.B.

---

## Answer:

Hi D.M. and J.B.,

Thank you for the interesting questions.

You are talking about using a super learner of `$\mathbb{E}(Y \mid A, W)$` and
`$\mathbb{E}(Y \mid A, Z, W)$` to obtain plug-in estimators of the total effect
and its direct and indirect effect components, possibly controlled direct
effects (CDE) instead of natural direct effects (NDE), which are essentially
averages of controlled direct effects, i.e.,
`$NDE = \mathbb{E}(\sum_z (Y_{1,z} - Y_{0,z}) g(z \mid A = 0, W))$`.

You consider the case that `$A$` is binary, a vector of binaries, or even vector
of continuous exposures. It is important to stick to the roadmap. Super
learner-based plug-in estimators are generally not supported by theory, so
inference is a problem; moreover, these are also generally overly biased, making
it unlikely that a bootstrap can yield accurate inference (i.e., estimating
variance is one thing but estimating bias is a whole other beast). Following the
roadmap is all about defining the target estimands of interest and the
statistical model; then, you can use or develop a TMLE and possibly an
undersmoothed HAL, without TMLE.

This also makes sure we obtain compatible estimators so that the total effect,
and its direct and indirect effect components, are all compatible with a single
fit of the data density. This helps improve finite-sample behavior.

And, yes, we can plug in a HAL fit into, e.g., a shift intervention parameter
`$\mathbb{E}_P \int_a \mathbb{E}(Y \mid A=a, W) g_{\delta}(a \mid W) da$`. When
using HAL, both bootstrap-based inference and influence curve-based inference
are possible. The latter is fast while the former might have better coverage in
finite samples.

Regarding causal effects and direct effects of multidimensional vectors of
exposures, I think stochastic interventions are the way to go. For example, in
work with [Chris Kennedy](https://ck37.com/), we proposed a conditional
distribution of the vector `$A$` of exposures given `$W$`, using the fact that
a score `$S(A)$` equals a value, which happens to correspond with the static
intervention on `$S(A)$`. So, in general, I like to think of staying close to
the conditional distribution of `$A$`, given `$W$`, and, for that purpose,
replacing it by a shift or adding an extra conditioning are all sensible ways to
intervene. In this way, we keep positivity intact. Your multivariate shift
sounds very interpretable, so would be good to formulate a TMLE. My guess is
that it will be better to stick to univariate interventions, such as
interventions on a data-adaptively learned summary measure of `$A$`, and then
provide the corresponding stochastic intervention interpretation for `$A$` as
a whole. That way, we know that the essential statistical estimation problem is
equivalent with that of learning the causal effect of a single continuous
exposure. Making it two-dimensional is still okay, but it might run out of hand
quickly and become very unstable if we carry out truly high-dimensional
interventions. In the end, it is all about `$g^{\star}/g$`, but now with
multivariate conditional distributions of `$A$`.

I think of it more as, yes, we want to understand the overall dose response
curve of the vector `$A$`, but could we do that by targeting one specific
feature of this curve at a time, using TMLE across many features. For example,
intervening on a score `$S(A)$`, across many possible scores `$S$`, might teach
us a lot about the overall dose-response curve for multivariate interventions on
the whole `$A$`.

Nowadays, we can do a one-step TMLE targeting all these smooth target features,
making it all compatible with a single targeted fit of `$\mathbb{E}(Y \mid
A,W)$` or `$\mathbb{E}(Y \mid W,A,Z)$`.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
