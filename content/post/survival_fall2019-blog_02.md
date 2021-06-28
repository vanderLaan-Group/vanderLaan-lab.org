+++
title = "Two-stage sampling and survival analysis"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2019-12-30T13:30:00+00:00"
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

## Question:

> Hi Mark,
>
> We are wondering under your framework, how to deal with a situation when only
> right-censored data has a full set of covariates, while the covariates for the
> non-right-censored data are largely missing. To be specific, we want to find
> the relation between peoples' matching property and their marriage durations.
> However, our datasets only have the matching property information for samples
> who are married (e.g., partners' age, characteristics, income, etc.), while
> these same covariates for sample who are unmarried are either incompatible or
> completely missing.
>
> Very appreciative for your response!
>
> ZH and SW

---

## Answer:

Hi ZH and SW,

We can define the full-data of interest as `$X = (W_1, W_2, A, T)$`, where we
let `$W_1$` be covariates that are always observed, `$W_2$` are those subject to
missingness, `$A$` is a binary treatment, `$T$` is a survival time. Looks like
that in your study, at some chronological time, we have a cross-sectional sample
of subjects, and, if they are still married, then we measure `$W_1, W_2, A$` and
know they are still married. So, we have right-censored data on their time until
divorce `$T$`, where the censoring time `$C$` is time from the wedding to
current point in time. If they are divorced, we have ony `$T$` and instead
measure covariates, _but_ for divorced subjects we obtain only `$W_1, A$` but
not `$W_2$`. Your observed data on a unit can be represented as
`$O = (W_1, \Delta_{W_2}, \Delta_{W_2} W_2, \tilde{T} = min(T, C), \Delta =
\mathbb{I}(T \leq C))$`, which is thus a function of `$(X, \Delta, C)$`.

Let's assume that the hazard of censoring, given full data `$X$`, is only a
function `$\lambda_C(c \mid A, W_1)$` of the always-observed variables
`$W1, A$`. Let's also assume that `$A$` is randomized conditional on `$W_1,
W_2$`. Suppose we view our observed data structure as a sequential coarsening
at random data structure, where the first censoring is right-censoring, and,
subsequently, we add missingness of `$W_2$` on top of this. So, the first layer
of censored data has as full data `$X = (W_1, W_2, A, T)$` and censored
data `$O_1 = (W_1, W_2, A, min(T, C), \Delta = \mathbb{I}(T \leq C))$`.

We can identify the distribution of `$X$` from the distribution of `$O_1$` due
to censoring satisfying coarsening at random and `$\mathbb{P}(C > c_{max} \mid
W_1, W_2, A)$` strictly positive. We now treat `$O_1$` as full-data and have
`$O = (W_1, \Delta_{W_2}, \Delta_{W_2} W_2, A, min(T, C),
\Delta = \mathbb{I}(T \leq C))$` as a missing data structure, where the
missingness indicator `$\Delta_{W_2}$` represents the missingness variable. We
assume that `$\Delta_{W_2}$` given `$O_1$` only depends on `$W_1, A, min(T,C),
\Delta$`, which appears to be true. However, we also need a positivity
assumption: in your case, we have that `$W_2$` is never measured if the failure
time is observed: `$\mathbb{P}(\Delta_{W_2} = 1 \mid W_1, A, \tilde{T},
\Delta = 1) = 0$`. This means that the probability of observing the full-data
`$X$` equals zero. This is a nasty situation.

So, here is an alternative approach, defining the full data in a less ambitious
manner. Suppose that we observe the censoring time `$C$` as the time from the
wedding to the current time. Let `$t_0$` be an arbitrary time and consider the
outcome of interest as `$Y = \mathbb{I}(T > t_0)$`, the indicator of marriage
lasting longer than `$t_0$` years. Now, we can define `$t \Delta_{Y} =
\mathbb{I}(C > t_0)$` and `$\Delta_{W_2}$` as the two missingness indicators
for `$Y$` and `$W_2$`, respectively. In this case, even among the uncensored
observations, i.e. `$C > t_0$`, there will be subjects with `$\Delta = 1$` and
`$\Delta = 0$`, so that we actually have subjects for which `$W_2$` is
measured.

Suppose now that our target quantity is `$\mathbb{E} Y_1 = \mathbb{P}(T_1 >
t_0)$`. Our observed data for this `$Y$` can now be formulated as `$O(t_0) =
(W_1, A, \Delta_{W_2}, \Delta_{W_2} W_2, \Delta_{Y}, \Delta_{Y} Y)$`. The
full-data is now `$X = (W_1, W_2, A, Y)$` and `$O(t_0)$` observed data.
Coarsening at random would now hold if `$\mathbb{P}(\Delta_{W_2} = 1,
\Delta_{Y} = 1 \mid X) = \mathbb{P}(\Delta_{Y} = 1 \mid W_1, A)
\mathbb{P}(\delta_{W_2} = 1 \mid \Delta_{Y} = 1, W_1, A)$`.

So we would assume both censoring and missingness of `$W_2$` are explained by
`$W_1$` and `$A$`, while also allowing `$\Delta_{W_2}$` to depend on
`$\delta_{Y}$`, which seems appropriate given your setting. It might now also be
reasonable to assume that `$\mathbb{P}(\Delta_{W_2} = 1, \Delta_{Y} = 1 \mid
X) > 0$` a.e., i.e., for each realization of the full-data, there is a positive
probability that we observe the full-data, since `$\delta_{Y} = 1$` still
allows subjects that are still married, thereby allowing `$W_2$` to be observed.

For example, we could now identify `$\mathbb{E}Y_1$` as follows: `$\mathbb{E}
Y_1 = \mathbb{E} \mathbb{E}(Y \mid W_1, W_2, A = 1) = \mathbb{E} \mathbb{E}(Y
\mid W_1, W_2, A = 1, \Delta_{Y} = 1, \Delta_{W_2} = 1$` or by using an IPTW
function: `$\mathbb{E}Y_1 = \mathbb{E} Y \mathbb{I}(A = 1, \Delta_{W_2} = 1,
\Delta_{Y} = 1) / (\mathbb{P}(A = 1 \mid W_1) \mathbb{P}(\Delta_{W_2} \mid
\Delta_{Y} = 1, A, W_1) \mathbb{P}(\Delta_{Y} = 1 \mid W_1, A))$`.

As a simple estimator of `$\mathbb{E}Y_1$` we could apply the TMLE of `$EY_1$`
based on data `$(W_1, W_2, A, Y)$`, thus adjusting for both `$W_1, W_2$`. This
TMLE would involve estimation of `$\mathbb{P}(A\ mid W_1, W_2)$` and estimation
of `$\mathbb{E}(Y \mid A, W_1, W_2)$`, targeting the latter estimate with
`$A / \mathbb{P}(A \mid W_1, W_2)$` and averaging w.r.t. the weighted empirical
distribution of `$W_1, W_2$`) with weights given by
`$\mathbb{I}(\Delta_{W_2} = 1, \Delta_{Y} = 1) / \mathbb{P}(\Delta_{Y} = 1 \mid
W_1, A) \mathbb{P}(\Delta_{W_2} = 1 \mid \Delta_{Y} = 1, W_1, A)$`. Importantly,
note that these weights are used in the estimation of `$g(A \mid W_1, W_2)$`,
`$\mathbb{E}(Y \mid A, W_1, W_2)$`, and even when taking the empirical mean
over `$W_1, W_2$`.

This is actually the IPCW-full-data TMLE for two-stage sampling designs (e.g.,
[Rose & van der Laan,
2011](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3083136/)), in which the
first stage yields data `$(W_1, W_2, A, \Delta_{Y}, \Delta_{Y} Y)$`, but then
we add missingness on top of this data structure, resulting in `$W_2$` being
missing for a subset of the subjects, where the missingness indicator
`$\Delta_{W_2}$` is allowed to depend on `$W_1, A, \Delta_{Y}, \Delta_{Y} Y$`,
i.e., on the always observed part of the data.

We also show how to make this IPCW-full-data TMLE for two-stage sampling designs
fully efficient, and thereby double robust, by targeting the missingness
mechanism `$\mathbb{P}(\Delta_{W_2} = 1 \mid W_1, A, \Delta_{Y}, \Delta{Y} Y)$`.
There is also [recent work by Nima Hejazi, David Benkeser, Peter Gilbert, et
al.](https://arxiv.org/abs/2003.13771), in which this approach is implemented,
refined, evaluated and further theoretically analyzed based on incorporation of
the highly adaptive lasso.

The trick which appears to save us here is that we define a full-data object at
`$t_0$`. We could even apply this approach for a collection of time points
`$t_0$`. Clearly, the larger we choose `$t_0$`, the fewer observations we have
with `$\Delta_{Y}=1$`. In the multiple timepoint case, one could then use a
final weighted isotonic regression (inverse weighting by the variance estimator
of each `$t_0$`-specific TMLE) on the `$t_0$`-specific TMLEs for
`$\mathbb{P}(T_1 > t_0)$` as a function of `$t_0$` to obtain a valid survival
function. Since we have the influence curve of the TMLE for each `$t_0$`,
simultaneous inference based on the multivariate normal limit distribution is
then available.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!