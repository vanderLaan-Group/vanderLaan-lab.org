+++
title = "Estimating the sample average treatment effect under effect modification in a cluster randomized trial"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2019-12-29T17:30:00+00:00"
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

<u>**Question:**</u>

> Hi Mark,
>
> We were wondering about the application of TMLE and
> superlearner to cluster-randomized study designs, and the adoption of the
> sample average treatment effect (SATE) as an efficient estimator. From our
> understanding, although the SATE is not formally identifiable in a finite
> setting, it is nevertheless an efficient estimate due to its asymptotic
> behavior (TMLE for the population effect is asymptotically linear and has an
> asymptotically conservative variance estimator). What properties of the SATE
> make it preferable to the population average treatment effect, particularly
> in effect modification settings? What allows for valid causal inferences to
> be drawn from data adaptive parameters like the SATE?
>
> Best,
> A.A. and D.C.

---

<u>**Answer:**</u>


Hi A.A. and D.C.,

Let's say we observe `$n$`  observations `$O_i=(W_i,A_i,Y_i)$` representing
cluster specific data structures, and we assume the randomization assumption,
`$P(A=1|W,Y_0,Y_1)=P(A=1|W)$`. The sample average treatment effect is defined as
`$SATE=\frac{1}{n} \sum_i(Y_i(1)-Y_i(0))$`, which is different from the sample
average *conditional treatment effect* `$SACTE= \frac{1}{n} \sum_i
\mathbb{E}(Y_1-Y_0|W_i)$`, and the latter is again different from the
`$ATE=\mathbb{E}(Y_1-Y_0)$`.

The `$ATE$`  is an average across a distribution of `$W$`, which in a cluster
RCT would mean it is a population average across clusters from some population
of clusters. In many cluster RCTs, the sample of clusters is not sampled that
way at all, but represents a selected convenient sample. Therefore, in that
case, it might make more sense to define a parameter from the conditional
distribution `$(Y_i,A_i)$`, given `$W_i$`, across `$i=1, \ldots ,n$`, i.e. treating
the clusters as fixed, and `$(A,Y)$` within each cluster as random. This makes
the SACTE an interesting alternative target parameter, which can be viewed as
a parameter of the conditional distribution given `$W_1,\ldots,W_n$`, or, one can
view it as a data adaptive parameter depending on the empirical distribution of
`$W_1, \ldots, W_n$` if one is still willing to view `$W_i$` as a random sample
from some population.

If one is not even willing to think of `$Y_1-Y_0$` as a random sample from
a conditional distribution `$P(Y_1-Y_0|W)$`, but only wants to make inference
about the actual values `$Y_i(1)-Y_i(0), i=1, \ldots ,n$`, then one could view the
SATE as the target. So the choice of quantity (among ATE, SACTE, SATE) is driven
by till what degree we wish to generalize our findings to a bigger population.
In various applications I might argue that all three are of interest.

Let TMLE represent the regular TMLE of the ATE. Recall that TMLE-ATE `$\sim
P_n(D_W+D_Y) =\frac{1}{n} \sum_i  (D_W+D_Y)(O_i)$`, where `$D_W,D_Y$` are the
two score components making up the influence curve `$D_W+D_Y$` of the TMLE.

Note that SATE-ATE (just a sample mean of `$Y_1-Y_0$`minus true mean) is
asymptotically linear with influence curve
`$$Y_1-Y_0-\mathbb{E}(Y_1-Y_0)=Y_1-Y_0-\mathbb{E}(Y_1-Y_0|W) +
\mathbb{E}(Y_1-Y_0|W)-\mathbb{E}(Y_1-Y_0)$$`,
and this is an orthogonal composition in the sense that the correlations of the
two terms are zero. The second term equals `$D_W$`.

So, TMLE-SATE = TMLE-ATE + ATE-SATE `$\sim P_n D_Y-\frac{1}{n}
\sum_i (Y_1-Y_0)-\mathbb{E}(Y_1-Y_0|W)$`. Similarly, TMLE-SACTE
`$\sim P_n D_Y$`.

We conclude: TMLE-SACTE is asymptotically linear with an improved influence
curve `$D_Y$`, having subtracted out the `$D_W$` component. The TMLE-SATE is
asymptotically linear with a further improved influence curve `$D_Y-D_U$`, where
`$D_U=(Y_1-Y_0-\mathbb{E}(Y_1-Y_0|W)$`. The latter `$D_U$` is not really an
influence curve since `$Y_0, Y_1$` are not observed. Nonetheless, it tells us
the the TMLE-SATE is asymptotically linear with inflluence curve `$D_Y - D_U$`
and, showing that TMLE-SATE is more efficient than TMLE-SACT. For the sake of
inference, we simply use `$D_Y$` as a conservative influence curve.

I believe the general idea of this is the following. Consider a target
`$\mathbb{E}[X]$` for a full data random variable `$X$`, and suppose that the
observed data includes observing `$W$`. Suppose that we have a TMLE of
`$\mathbb{E}X$`. One could define `$\frac{1}{n} \sum_i X_i, \frac{1}{n} \sum_i
\mathbb{E}(X|W_i)$`, and analyze the TMLE- `$\frac{1}{n} \sum_i X_i$` exactly
same was as above.

For example, suppose that we have a general longitudinal data structure,
`$W_i=L_i(0),A(0),\ldots, L(K),A(K),Y$`, and we define `$\mathbb{E}Y_d$` as
a mean outcome under a multiple time point dynamic treatment. We have a TMLE of
`$\mathbb{E}Y_d$`, such as the one implemented in `ltmle()`. We might desire
inference for `$\frac{1}{n} \sum_i Y_{d,i}$`, or `$\frac{1}{n}\sum_i
\mathbb{E}(Y_d|W_i)$`. We have `$\Psi_{\text{TMLE}} - \frac{1}{n}\sum_i
\mathbb{E}Y_{d,i} = \Psi_{\text{TMLE}} - \mathbb{E}Y_d-[\frac{1}{n} \sum_i
Y_{d,i}-\mathbb{E}(Y_d|W_i)]-[\frac{1}{n} \sum_i
\mathbb{E}(Y_d|W_i)-\mathbb{E}Y_d)]$`. The latter represents the `$D_W$`
component of the influence curve of the `$\Psi_{\text{TMLE}}-\mathbb{E}Y_d$`.
The other component is a non-identifiable influence curve that subtracts out
another component. So, we obtain conservative inference for `$\frac{1}{n} \sum_i
\mathbb{E}Y_{d,i}$` by using the influence curve of
`$\Psi_{\text{TMLE}}-\mathbb{E}Y_d$` without the `$D_W$` component of its
influence curve.

Regarding effect modification, if we have a discrete variable `$V$`, then
a stratified TMLE applied to data with `$V_i=v$` would obtain inference for
`$\frac{1}{n} \sum_i (Y_i(1)-Y_i(0))$` within strata `$V_i=v$`, for each `$v$`.
To obtain inference for this `$v$`-specific SATE, one can use the conservative
influence curve.

If one now wants to obtain inference for a difference of two `$v$`-specific
SATEs, then the TMLE of this difference will still be asymptotically linear with
the difference of the two `$v$`-specific non-identifiable influence curves. It
is now less clear if ignoring the difference of the two non-identifiable
components of their respective influence curves would still result in
conservative inference. It would be worthwhile to research this. Since we have
valid conservative inference for the `$v$`-specific SATE for each `$v$`, we
could also decide to build a test based on comparing the two marginal confidence
intervals (overlap), but this would by necessity be more conservative. If this
inference for a contrast of `$v$`-specific SATEs happens to be problematic, then
that might be an argument to instead focus on the effect modification parameter
(contrast of `$v$`-specific SACTE).

`$\frac{1}{n} \sum_i \mathbb{E}(Y_1-Y_0|W_i,V_i=1)-\frac{1}{n} \sum_i
\mathbb{E}(Y_1-Y_0|W_i,V_i=0)$` instead since for this we have an identified
influence curve.

If `$V$` is continuous, one might use a working MSM `$m_{\beta}(v)$` for
`$\frac{1}{n} \sum_i \mathbb{E}(Y_1-Y_0|W_i,V_i=v)$` as a function of `$v$`.
One can then use the TMLE of the beta in this working MSM (as implemented in
`ltmle` e.g.). This would again correspond with using an influence curve that
would remove a `$D_W$` component of the regular influence curve of the TMLE of
`$\beta$`.

So my basic answer to your question is that inference for the SATE based on the
TMLE of the ATE can be generalized to general longitudinal data structures, and,
one should be able to also generalize it to treatment effect modification by
a discrete or continuous effect modifier `$V$`.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
