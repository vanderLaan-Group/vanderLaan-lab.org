+++
title = "Machine learning for conditional density estimation"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2021-05-02T10:58:00+00:00"
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
> I was curious in general about approaching problems that involve machine
> learning-based estimation of densities rather than scalar quantities (i.e.,
> regression), particularly for continuous variables. As a grounding example,
> for continuous treatments in the TMLE framework one needs to estimate `$P(A
> \mid W)$`, where `$A$` is a continuous random variable. My (perhaps naive)
> impression is that it is difficult to get reasonable estimates of such
> a probability with limited samples. Even when `$A$` is discrete, I could
> imagine that these probabilities are difficult to estimate with
> high-dimensional `$W$` and limited samples. Any insight into how different or
> not-so different your approach to density estimation is compared to regression
> would be really interesting!
>
> Best,
>
> H.N.

---

## Answer:

Hi H.N.,

Thank you for your important question.

Yes, we are very much involved in conditional density estimation. We have
various approaches. In a 2010 paper on longitudinal TMLE, I proposed
discretization of the continuous variables to parametrize the conditional
density in terms of its hazard `$p(A=a \mid A \geq a,W)$`, so that it becomes
a repeated measures pooled logistic regression problem, and thereby we can
utilize the whole world of machine learning algorithms and parametric models,
with super learning. We used this approach to construct a TMLE ([Stitelman et
al.](https://pubmed.ncbi.nlm.nih.gov/22992289/)) for multiple time point
intervention-specific mean outcomes based on general longitudinal data
structures. In a [2011 paper](https://pubmed.ncbi.nlm.nih.gov/22718677/) by
[Iván Díaz](https://www.idiaz.xyz/) and myself, we proposed estimation of the
conditional density with a similar approach also involving selecting of the bin
width with cross-validation. [Oleg
Sofrygin](https://divisionofresearch.kaiserpermanente.org/researchers/sofrygin-oleg)
also implemented this logistic regression approach as part of his
[`condensier`](https://github.com/osofr/condensier) `R` package on causal
inference, which was useful for network causal inference, in which we have to
estimate a conditional density of multiple summary measures of the treatment of
the friends and the person itself. In more recent work, we use the highly
adaptive lasso (HAL). For example, in work with [Helene
Rytgaard](https://publichealth.ku.dk/staff/?pure=en/persons/411409) on TMLE for
continuous-time survival analysis, we estimate intensities and conditional
hazards, and thus conditional densities, with HAL using the Poisson family.
[Nima Hejazi](https://nimahejazi.org) is currently writing an article where we
compare different HAL-based estimators of the conditional density `$g(A \mid
W)$` (using his [`haldensify` `R`
package](https://github.com/nhejazi/haldensify)) and its impact on efficient
estimators of the effect of shift interventions, including tuning of the `$L_1$`
norm of these HAL estimators for the purpose of the estimator of the target
parameter. So, the overall message is that conditional density estimation is not
much harder than going after the conditional mean. Formally, the parameter space
of conditional density is one dimension larger than that from the conditional
mean, and optimal rates of convergence are determined by the entropy/covering
number of the function class. When using HAL, the rate this implies (at most) an
extra `$\log(n)$` factor contirbution. On the other hand, when using inverse
weighting by conditional density in TMLE or IPTW for causal inference target
estimands, there is more concern for positivity violations so that this requires
care. This also motivates to start developing TMLE for conditional independence
models that assume that `$L(t)$`  only depends on the past through recent time
points (Markov type), which implies that the efficient influence curve will
involve much less inverse weighting, adding stability to these estimators, while
such conditional independence assumptions could still be reasonable given the
right definition of `$L(t)$`.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
