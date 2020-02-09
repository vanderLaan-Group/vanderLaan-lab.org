+++
title = "Longitudinal Causal Model under Obscured Time-Ordering of Variables"
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

> Suppose we have a longitudinal data structure where information about the intervention 
> and time-varying covariate is collected simultaneously, and their temporal ordering is 
> obscured. For instance, data is collected at monthly health checkups, where `$A(t)$` 
> is the subject's healthy eating habits in the past month, and `$L(t)$` is the 
> occurrence of heartburn in the past month. 

> Is there a recommended way to move forward with data like this in terms of defining a 
> causal model (e.g., `$A(t)$` before `$L(t)$` vs. `$L(t)$` before `$A(t)$` vs. `$L(t)$` 
> and `$A(t)$` depend only on the observed past) and/or to incorporate sensitivity
> analysis?

> Thanks,
> D.C. & M.M.

---

<u>**Answer:**</u>

Hi D.C. & M.M.,

We wish to code the data as a longitudinal time ordered data structure 
`$L(0),A(0),\ldots,L(K),A(K),Y$`, where we need that `$L(k)$` occurs before `$A(k)$`, 
or, at least, we need to know that `$L(k)$` is not affected by `$A(k)$`. If data is 
discretized in monthly intervals, then, we might define `$L(k)$` as the relevant 
extractions of time-dependent covariates and events measured in month `$k$`, while 
`$A(k)$` is defined as the treatment summary over month `$k+1$`. In this way, the 
time-ordering is respected. However, note that the sequential randomization assumption 
assumes that `$A(k)$` is only affected by history in previous `$k-1$` months, not month 
`$k$` itself. Therefore, respecting the time ordering comes at the price of having 
reduced some information, potentially causing some bias due to confounding one cannot 
adjust for. Therefore, it could be important to make the time intervals small enough so
that this type of bias is not a practical issue. Romain Neugebauer has written an `R` 
package that takes as input standard files (such as `SAS` files) and maps them into the l
longitudinal format of the `ltmle` package or Oleg Sofrygin's longitudinal package 
`stremr`, respecting this time ordering issue, based on user supplied choices. 

It is not a bad idea to carry out the analysis for finer and finer time intervals to 
evaluate the sensitivity of the inference towards these artificial discretization 
choices. However, there is a tension in the sense that too many time intervals might
make the current TMLE less stable, due to it being based on sequential regression which 
requires having various measurement at each time point, while too few intervals makes 
the TMLE more stable but can cause bias due to ignoring important time-dependent 
covariate information in the fit of the treatment and censoring mechanisms. 

This need for artificial discretization of the actual data has motivated us to consider 
TMLE for longitudinal data for arbitrarily fine time intervals, including continuous 
time, so that for any particular time point, it might only involve measuring very few
subjects (or zero subjects). This is based on work with Helene Rijtgaard. 

Best Wishes, 
Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!