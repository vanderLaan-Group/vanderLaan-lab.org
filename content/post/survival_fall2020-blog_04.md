+++
title = "Using a data-adaptive target parameter and CV-TMLE in survival analysis"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2020-12-04T16:51:00+00:00"
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
> Within the field of industrial hygiene and occupational epidemiology there is
> interest in linking possible occupational exposures to deleterious health
> outcomes, most usually various cancers. Obviously in such a setting, it is
> nearly impossible without individual chemical biomarkers to have causal
> identifiability for a specific exposure (for example lead, pesticides,
> benzene, etc.) on some outcome. This is because it is impossible to control
> for other simultaneous exposures that may confound the relationship or may
> interact with other exposures to cause more severe outcomes. As such, rather
> than creating a job exposure matrix, giving a particular occupation a nominal
> risk score for a particular exposure (for example, a risk score 0-3 for lead
> exposure) it may be more interesting to look at the occupation itself. For a
> given outcome, say Leukemia, it may be of more interest to find the set of
> occupations that have the highest rates of Leukemia. This would then become a
> data-adaptive parameter such that occupational specific hazards would need to
> be calculated for each occupation category within a training data sample. The
> set of occupations that maximize the hazard difference can be captured as a
> rule. For example, in a training set it may be determined that high risk
> occupations are: teachers, librarians, manufacturers, and truckers compared to
> low risk occupations academic professionals, financial workers, and tech
> workers.  This rule can then be carried over to a validation set where the
> hazard difference can be calculated for this rule which compares the
> occupations. Investigation of how occupational exposure may occur can happen
> post-hoc. For example, given teachers exposure to products such as markers,
> paper, plastics in binders, etc. over years, this may lead to certain cancers
> due to bioaccumulation of toxic chemicals in these products. Most occupational
> exposure focuses on manufacturing/transportation workers but other occupations
> may be overlooked. This data-adaptive procedure could allow for a more
> exploratory approach, to "listen to the data" and then investigate what
> exposures may be occurring in high-risk categories post-hoc. From an
> application standpoint, this seems to require
> a). creating job categories that are not too granular,
> b). calculating the occupation specific hazard for each occupation
> c). finding the set of occupations such that the sum of the hazards in that
>     set maximize the difference
> d). creating a rule to indicate that set,
> e). calculating the risk for this rule in a validation set.
> The questions we have for this procedure are: is it appropriate in this
> setting to sum over hazards in order to identify the occupational set that has
> the largest risk difference compared to low-risk occupations? Given that we
> would likely use cross-validated survival TMLE (for right censored data with
> no time-varying covariates), can we derive proper statistical inference for
> such an approach?
>
> Best,
>
> D.M., S.F., and A.M.

---

## Answer:

Hi D.M., S.F., and A.M.,

Sounds like your data is of type `$O=(W, \text{min}(T, C), \Delta =
\mathbb{I}(T \leq C))$` with one of covariates W_1 being type of occupation. You
are interested in estimating occupation-specific risk `$\psi(k) = S(t_0 \mid
W_1 = k) = \mathbb{P}(T > t_0 \mid W_1 = k)$` for each occupation `$k$`. For
that we could use TMLE `$\psi_n(k)$` for each `$k$`. As a whole vector, the TMLE
`$\psi_n$` is asymptotically linear with a vector influence curve, allowing for
formal statistical inference based on working model `\psi_n \sim
\text{N}(\psi_0, \Sigma)` with `$\Sigma$` covariance matrix of the influence
curves. This allows you to construct a simultaneous confidence interval and
carry out multiple testing procedures controlling particular family-wise error
rates, for example. In this case, all these estimators would be based on
`$k$`-specific disjoint samples, so that the estimators `$\psi_n(k)$` are
independent across `$k$`, although one could use pooling when estimating the
conditional hazards of failure and censoring.

I am not completely clear why you prefer to not do this. It sounds that you like
to group various occupations in groups and do that in a data adaptive manner.
So, now you create presumably (based on training sample, given a sample split)
disjoint and complementary sets `$S_1, \ldots, S_J$` of occupations, and are
concerned with inference for `$\psi(S_j) = S(t_0 \mid W_1 \in S_j)$`. This would
be a vector of data dependent target parameters due to `$\hat{S} = (\hat{S}_1,
\ldots, \hat{S}_J)$` being a data-dependent partitioning. If `$\hat{S}$` is
a type of selector that optimizes contrast in risk, like `$\psi(S_1)$` is group
with maximal risk, etc., then inference treating these `$J$` parameters as _a
priori_ specified will be finite-sample biased. On the other hand, if you use
simultaneous inference based on all occupations as above, then all `$K$`
confidence intervals can be interpreted as true uniformly in all `$K$`
occupations, so that one is able to look at subgroups, etc., all still resulting
in statements with 95% confidence (i.e., across repeated sampling, 95% of times
your statements would be correct). In that case, the multiple testing adjustment
was already made across all occupations.

The advantage of the data-dependent clustering and using CV-TMLE with multi-fold
sample splitting is that it will obtain the power as if these (smaller number)
`$J$` target parameters where a priori specified, so much smaller multiple
testing adjustment.

The price you pay is that the target parameters CV-TMLE targets are an average
over sample splits of subset (selected on that training sample) specific risks,
where the first subset `$\hat{S}_1$` might be different from one training sample to
another. So you really only obtain inference for `$1/V \sum_v
\Psi(P_0)(\hat{S}_1(P_{n,v})$` for the data adaptively selected target subset
`$\hat{S}_1(P_{n,v})$`. If it happens to be very stable, then
`$\hat{S}_1(P_{n,v})$` might be similar across splits, in which case you will be
fine with the interpretation. Either way, it provides powerful inference for
that average across sample splits of split specific target parameters: rejection
of the null would imply at least one of the sample split-specific target
parameters is significant. That is the trade-off relative to doing simultaneous
inference across all occupations.

This CV-TMLE approach applies to any data adaptive partitioning `$\hat{S}$` into
subgroups of occupations, so, yes, based on summing the individual occupation
risks or whatever is fine. For example, you could create a dissimilarity matrix
`$d(i,j)$` of dimension `$K$`-by-`$K$`, where `$K$` is total number of
professions, and `$d(i,j) = \psi(i) - \psi(j)$` and apply a clustering algorithm
such as PAM (partitioning around medoids) specifying a desired number of
clusters. This would then create groups with similar risks. Average silhouette
width could be used to decide on number of clusters, as part of this data
adaptive partitioning method, `$\hat{S}$` aims to make the clusters as different
as possible.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!