+++
title = "Applications of TMLE in infectious disease research"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2019-05-11T14:35:00+00:00"
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
> Thanks for teaching this class. It's been an amazing experience.
> I have a few questions related to my own research.
>
> In infectious disease studies, modeling attempts to create models that
> estimate protection conferred from vaccination or previous history of
> infection (natural immunity). Let's say you were studying a specific disease
> and given a longitudinal dataset where participants in the study naturally
> experienced repeated infections. Researchers think that there are a number of
> variables that could confound the relationship relationship between exposure
> of a pathogen strain and "risk" of contracting the pathogen again, but many in
> the field wonder if there are unmeasured exogenous nodes that are captured by
> the identifier of the person. They believe there is unmeasured nodes because
> in different populations, estimates of protection change drastically,
> suggesting there may be an underlying individual susceptibility to each
> person.
>
> Traditionally, I think people would use a GLM (Poisson distribution) to
> predict subsequent infection based off of previous number of infections,
> controlling for covariates or even the identifier of the child. What are the
> problems with that approach?
>
> Using TMLE framework and the roadmap, how would you create a model, target
> parameter, and estimate that maps the influence of the previous number of
> infections on the "risk" of getting a subsequent infection controlling for a
> persons underlying susceptibility (which is a parameter we don't know anything
> about)?
>
> Furthermore, do you have any general, thoughts, advice, and or concerns for
> infectious disease modeling? Have there been application of TMLE in this
> field?
>
> D.W.

---

## Answer:

Hi D.W.,

I address your question in a few different parts below:

> Traditionally I think people would use a GLM (Poisson distribution) to predict
> subsequent infection based off of previous number of infections, controlling
> for covariates or even the identifier of the child. What are the problems with
> that?

If the goal is to predict the probability of subsequent infection based on past
infections, baseline covariates, then why use a parametric model. In other
words, we can use super-learner and improve the prediction performance. If the
goal is not prediction, then we need to formulate the target estimand that
represents the answer to question of interest. As we know if the interest is in
some kind of coefficient in this model, then there will be a nonparametric
defined target estimand that represents the same answer but without relying on
this parametric model. Regarding adjusting for the child itself, I wonder what
the precise story is. If one really adjusts for the child itself, then one will
need a time series of data within the child so that one observes many time
specific records that provide information about question such as short term
effect of past infection on next infection or so. In that case, one needs
asymptotics in number of time points. Indeed we are working on causal inference
for single time series precisely because of same reason that each person could
be different in ways that cannot be captured by measured covariates.

Your example does not sound as a time series problem, so then it must be the
case that adjustment for the child is probably meant to represent the
introduction of a random effect with some specified distribution. It is hard
to argue that such a model fit would represent the child specific model fit,
since clearly the random effect assumption is not representative of reality.
Such random effect models still imply regression models for the outcome given
the observed covariates (integrate out random effect, given covariates), so
we are back to my point above of why assume such a parametric form for this
conditional probability of infection as a function of the history of the
child.

These random effects are often accompanied with nice stories such as that it
represents the underlying susceptibility of the child, but that should be taken
with an enormous grain of salt. One would be better of to measure the genetics
or immune response of the child and aim to include that in set of baseline
covariates and time dependent covariates, since that should represent the
underlying susceptability for infection. One is then back to the super-learner
problem and one might also learn how important the genomics/genetics is for
this future probability of infection.

> Using TMLE notation and the roadmap, how would you create a model, target
> parameter, and estimate that maps the influence of previous number of
> infections on the "risk" of getting a subsequent infection controlling for
> a persons underlying susceptibility (which is a parameter we don't know
> anything about)?

If the goal is the effect of previous number of infections on a future
infection, controlling for baseline history (pre-infections), or the effect of
a multiple time point infection profile on future infection, where we now treat
the infection profile as a multiple time point exposure, and we desire to know
the counterfactual infection rate under a chosen multiple time point
intervention. In the latter case, this involves time-dependent confounding, and
this fits a causal estimation problem addressed by `R` package `ltmle()`, for
example. If one wants to view the total number of previous infections as
a discrete ordered exposure, then we can define a target estimand as the
identification result for the infection rate under a shift-intervention on the
total number of infections: i.e., how does the infection rate change if we add
one infection to the child's past infection profile. However, if there are
time-dependent covariates in between different infections, then these are going
to be time-dependent confounders, so that one will need to go for causal
inference for multiple time point exposure profiles with intermediate
time-dependent confounders.

Again, if one feels that there are unmeasured confounders represented by an
underlying susceptibility, then one might carry out the above analysis and
views it as the best effect estimate and inference one can obtain with the
available measured confounders. That might already be insightful, but one will
know that it will not be causal and one could discuss how much the unmeasured
confounding would have to shift the target estimand to make the p value non
significant or the confidence interval include zero effect. However, we would
also advise in that analysis, at the interpretation of the results, to start
measuring these unmeasured confounders, instead of fooling each other with
a random effect!

> Furthermore, do you have any general, thoughts, advice, and or concerns for
> infectious disease modeling? Have there been application of TMLE in this
> field?

Yes, this is an area of research open for the application of the general
roadmap of causal inference and targeted learning. We have research on causal
inference for networks of interconnected individuals, where we use TMLE to
estimate the mean outcome for the community of individuals under a particular
intervention on the exposure/treatment nodes (such as assigning vaccination),
where we assume that we have network information in the sense that we know
a maximal set of friends that influence the data generating for the individual
in question at the next time point. This is certainly getting into causal
inference models for epidemics, and it uses nonparametric structural equation
models, defines causal queries formally, defines target estimand based on the
G-computation formula, and then develops a TMLE, involving deep functional CLTs
for its analysis due to all the interdependence among individuals.

We have certainly exchanged our thoughts at places, but not devoted the time
and effort yet: for example, a former student and post-doc Oleg Sofrygin started
work with John Marshall's lab, using mathematical models for modeling
malaria-type epidemics. The overall approach is to first replace the
mathematical models by nonparametric structural equation models as much as
possible. Replace their coefficients of interest by nonparametrically defined
causal quantities such as impact of an intervention on overall infection rate.
Obtain identification, develop TMLE, and its inference. In other words, the
analogue of what we have done for causal inference for networks, i.e., carefully
following the roadmap for causal inference and targeted learning.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!