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
> After the lectures on tmle3shift and tmle3mediate, we are wondering if
> a different procedure for mediation could work. Consider a data-generating
> system for $O = W, A, Z, Y$, where W represents three binary covariates, A is
> a binary exposure of interest, Z are three binary mediators and Y is
> a continuous outcome. Fitting a SuperLearner for $E(Y|A,W)$ (ignoring Z) and
> then getting predictions through this fit for $E(Y|A = 1, W)$ and $E(Y|A = 0,
> W)$, $E(Y|A = 1, W$ - $E(Y|A = 0, W)$ represents the total effect ATE. Call
> this model $\mathcal{M}_{total}$. Now consider another model fit for $E(Y | A,
> Z, W)$ and conduct the same procedure. The model now controls for the mediator
> and when we deterministically set A = 1 and A = 0 we break the connection with
> the mediator and therefore can calculate the direct effect, call this model
> $\mathcal{M}_{NDE}$. The natural indirect effect can be calculated as
> $\mathcal{M}_{total} - \mathcal{M}_{NDE}$. This works in simulations (using
> simulated data for tmle3mediate). Of course, it's kind of bullshit because we
> have tmle3mediate which can calculate the NDE/NIE for a binary exposure - this
> two model approach is just borrowed from the glm method ages ago but uses
> SuperLearner instead. However, there are some interesting scenarios where this
> might be useful for example when there are multiple As. So consider we have $A
> = A_1, A_2, A_3$, all binary. The same procedure could be done by setting all
> As to 1 and 0 and calculating the NDE and NIE in this way. Could this serve as
> a non-parametric approach to mediation analysis for mixtures? Of course,
> a bootstrap procedure would need to be done to derive confidence intervals.
> Similarly, now consider A is continuous, the same procedure could be conducted
> but now shifting A by some $\delta$. For example, the NDE and NIE can be
> calculated for all chemicals of a joint exposure if the chemical exposures
> were all shifted by 10\%. This seems more intuitive, especially for
> researchers than shifting the propensity, as with tmle3shift. Now, taken one
> step further, given the unique properties of highly adaptive lasso (which we
> are still trying to understand), is it possible to do a similar procedure but
> with basis functions? Meaning that a HAL is fit to the data $E(Y|A,Z,W)$. Is
> it possible to then make predictions through the model to calculate the NDE
> and NIE by shifting certain basis functions? If so, given the unique
> properties of HAL (can be used for statistical inference without TMLE (?))
> could one get around the nonparametric bootstrap?
>
> Best,
>
> D.M. and J.B.

---

## Answer:

Hi D.M. and J.B.,

Thank you for the interesting questions.

You are talking about using a super-learner of E(Y|A,W) and E(Y|A,Z,W)
to obtain plug-in estimators of the total ATE IDE and NDE, possibly
controlled direct effects instead of Natural Direct Effects which are
essentially averages of controlled direct effects:

NDE=E(sum_z (Y_{1z}-Y_{0z})g(z|A=0,W) ).

You consider the case that A is binary, a vector of binaries, or even
vector of continuous exposures.

It is important to stick to the road map and a SL based plug-in is
generally not supported by theory so that inference is a problem and it
is also generally overly biased making it unlikely that a bootstrap can
get the inference right (estimating variance is one thing but estimating
bias is a whole other level). So it is all about defining the target
estimands of interest, the statistical model, and then you can use or
develop a TMLE and possibly an undersmoothed HAL, without TMLE.

This also makes sure we obtain compatible estimators so that the ATE,
NDE and IDE are all compatible with a single fit of the data density.
This helps finite sample behavior.

And yes we can plug in an HAL fit into e.g. a shift intervention
parameter E_P int_a E(Y|A=a,W)g_{delta}(a|W) da . When you use HAL you
can use bootstrap based inference as well as influence curve based
inference. The latter is fast and the earlier might have better coverage
in finite samples.

Regarding causal effects and direct effects of multidimensional vectors
of exposures, I think stochastic interventions are the way to go. For
example, in work with Chris Kennedy we proposed a conditional
distribution of the vector A of exposures given W and given that a score
S(A) equals a value, which happens to correspond with the static
intervention on S(A). So in general I like to think of staying close to
the conditional distribution of A, given W, and for that purpose
replacing it by a shift or adding an extra conditioning are all sensible
ways to intervene. In that way, we keep positivity. Your multivariate
shift sounds very interpretable so would be good to check out a TMLE. My
guess it will be better to stick to univariate interventions such as
interventions on a data adaptively learned summary measure of A and then
providing the corresponding stochastic intervention interpretation for A
as a whole. In that way, we know that the essential estimation problem
is equivalent with a learning the causal effect of a single continuous
exposure. Making it two dimensional is still okay but it might run out
of hand quickly and become very unstable if we carry out truly high
dimensional interventions. In the end it is all about g*/g but now with
multivariate conditional distributions of A.

I think of it more as, yes we want to understand the overall dose
response curve of the vector A, but we could do that by targeting one
specific feature of this curve at the time and use TMLE, across many
features. For example, intervening on a score S(A), across many possible
scores S, might teach us a lot about the overall dose response curve for
multivariate interventions on the whole A.

Nowadays we can do a one-step TMLE targeting all these smooth target
features making it all compatible with a single targeted fit of E(Y|A,W)
or E(Y|W,A,Z).

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
