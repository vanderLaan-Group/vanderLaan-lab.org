+++
title = "Imputation and missing data in the TMLE framework"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2019-05-10T10:01:00+00:00"
categories = [
    "Resources",
    "Statistics",
    "Targeted Learning",
    "Q&A"
]
+++

_This post is part of our Q&A series._

A question from graduate students in our Spring 2019 offering of "Targeted
Learning in Biomedical Big Data" at Berkeley:

## Question:

> Hi Mark,
>
> For a longitudinal data set if we have missing data, we might want to impute
> the values with MICE imputation (multiple imputation with chain equations).
> Can we use TMLE together with multiple imputation? How can we combine the
> results of all the multiple imputed datasets into a final result and obtain
> valid inference?
>
> There are two sources of variability:
>
> 1. Each model's individual variability, and
> 2. No matter how many datasets are generated there won't be an infinite number
>   of datasets. (Theory says that if you generate an infinite number of
>   datasets then you can obtain an unbiased estimate).
>
> Papers that we found are simply applying multiple imputation and then running
> TMLE without addressing the issue of inference.
>
> -A.W.

---

## Answer:

Hi A.W.,

To start with we have to ask for what variables imputation is carried out, i.e.,
if the imputation is happening for a key variable such as treatment or outcome,
or if it is happening for a baseline or time-dependent covariate that might
predict censoring or treatment. For the latter case, it might be perfectly
reasonable to assume that the sequential randomization for both treatment and
censoring, conditional on the past (defined for what it is, thus, e.g.,
involving that a covariate is missing but we know it is missing) holds for the
actual observed covariate histories. For example, the decision to set treatment
was based on what was observed, and the doctor simply did not have access to
these variables that are reported missing (i.e., were not measured). In that
case, one should not view this as missing. We simply code the covariate in
question with two columns, one being an imputation indicator and the other the
true or imputed value. We think that imputing something meaningful such as
carried out with a imputation algorithm is not a bad idea, since it might help
the Super Learner to do a better job extrapolating, instead of having to deal
with covariates that have an artificial value when not observed.

If one feels strongly that a certain underlying covariate/time-dependent
covariate was used in treatment decisions or censoring, even when not observed,
then one could identify that covariate as a special one that is subject to
missingness. One would now rely on a G-computation formula assuming the
sequential randomization of the intervention nodes w.r.t. the history including
the underlying (possibly missing) special covariate. Now, one still has to deal
with the missingness of the special covariate, which has its own missingness
mechanism.

So then we are left with missingness of a treatment node, censoring node,
outcome node, possibly an effect modification node `$V$` that might be used to
define the target parameter (e.g., treatment effect modification by `$V$`), and
possibly a special (time-dependent) covariate that is truly needed for
identification.

One might first rely on the G-computation formula for the full-data that has no
missingness on these nodes, and represent the observed data as a missing data
structure on this full data with its own missingness mechanism (assuming CAR).
However, one can also include these missingness nodes as intervention nodes as
in causal inference, and talk about interventions on treatment, censoring but
also setting this missngness to non-missing. Or some of the missingness nodes
are included in definition of intervention nodes and we use a corresponding
G-computation formula for the post-intervention distribution while there is
still some additional missingness which maybe can be viewed as second stage
missingness applied after having seen the whole data structure (so now
missingness on that node can also depend on future). So this is is really a
modeling stage in the sense that we need to agree on a data generating model
that incorporates these non-testable assumptions, and corresponding
identification result.

Either way, after having done this, we have defined the statistical estimation
problem, model, target estimand, and we can develop the efficient influence
curve and TMLE. This TMLE would involve modeling this missingness mechanism,
just as it would involve modeling treatment and censoring mechanism. It probably
(if the model is nonparametric) it will involve inverse weighting by these
mechanisms in one way or another (e.g., in weights of loss or in clever
covariates). Keep in mind that a TMLE or in general an efficient estimator in
essence involves imputing missing or censored outcomes since the objects involve
prediction functions. For example, for `$(W, A, \Delta, \Delta Y)$`, the TMLE
involves fitting `$\mathbb{E}(Y \mid W, A, \Delta = 1)$`, and uses this model to
evaluate this prediction for all observations, including the ones for which
`$Y$` was missing. So that is making imputations of the missing `$Y_i$`, and
then it takes sample mean. Similarly, this is what happens with right-censoring,
one essentially estimates conditional means, given you have not been
right-censored, of a future counterfactual outcome.

The maximum likelihood type imputation methods often view all variables as
missing even when they are not w.r.t. target parameter as explained above.
Anyway, if one uses maximum likelihood based imputation of the true missing
variables to generate many full data sets, then the whole inference will still
rely on the model that was used for the imputation. So, one loses robustness. If
one applies TMLE to these imputed data sets, then the second step has its own
double robustness, but still the final estimator will depend on a parametric
imputation model. It might even be non compatible with the model use by TMLE,
i.e., one first assumes a parametric likelihood to carry out imputations, and
then one assumes  a nonparametric model for the resulting full-data set and
applies TMLE. SO this might get weird and clearly deviating from a sensible
roadmap of targeted learning.

I presume that there is a theory developed for these multiple imputed data set
method that states that if the imputation model is correct, and one has
available a full data estimator with influence curve and thereby variance
estimator, then one can obtain valid inference by combining the collection of
TMLEs and their variance estimators. So if that theory exists, then one can then
apply that allowing one to mix a model based imputation method with TMLE dealing
with remaining challenges (but not worried about the particular missingness
dealt with by the imputation method).

I have no problem with this, but I would like to make sure that the imputation
model is compatible with the model used by TMLE applied to imputed data set. So
one might keep the imputation model as nonparametric (i.e., only assume what is
known) as in TMLE. However, that would then require machine learning on the
model used to carry out the imputation. And now I am sure we are going to see
that the resulting estimator will only be asymptotically linear if we also
target the fit used for imputation, thereby truly moving to a full blown TMLE
again.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!