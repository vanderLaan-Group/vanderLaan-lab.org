+++
title = "Data-adaptively learning strata-specific causal effects"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2021-05-02T12:29:00+00:00"
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
> I have a question about applying CV-TMLE to a current research project. I have
> a cross-sectional dataset from Bangladesh, where the outcome of interest is
> antenatal care use (binary), the exposure of interest is women's empowerment
> (continuous), and the baseline covariates include mother's age, child's age,
> mother and father's education, number of members in household, number of
> children under 15, household wealth, and maternal depression. Women's
> empowerment is a score generated from various questions around freedom of
> movement, control over assets, and household decision making. However, from an
> intervention and policy design standpoint, it is potentially more interesting
> to compare differences in levels of empowerment. For example, we may want to
> know if antenatal care use is different between women with "high" empowerment
> versus "low" empowerment. Or perhaps even three categories: very high,
> average, very low. You mentioned the possibility of using `varimpact()`, which
> I understand data-adaptively discretizes a continuous exposure and selects the
> highest contrast comparison between the two exposure levels using CV-TMLE.
> I am really curious to learn more about how exactly this works and what
> happens under the hood to select the highest contrast comparisons. Can you
> explain the steps and intuition behind them? Is it possible to discretize into
> more than two levels? Does the procedure output estimates of the average
> treatment effect(s) or would that have to be done separately?
>
> Best,
>
> S.W.

---

## Answer:

Hi S.W.,

Thank you for your nice example and excellent question.

Sounds like a good plan. Yes, [`varimpact()`](https://github.com/ck37/varimpact)
starts out with data-adaptively discretizes exposure `$A$` (but I believe it
does this outcome-blind so that we can keep it fixed after that, and can then
have well-defined categories for `$A$`), and proceeds as follows:
1. It computes a TMLE of `$\mathbb{E} Y_j$` for each category `$j$`, and then
2. it determines the two levels that maximize the contrast
   `$\mathbb{E} Y_j - \mathbb{E} Y_k$`.

This is just the algorithm that maps the data into the desired target parameter
`$\Psi_{n}(P) = \mathbb{E} Y_{j_{1n}} - \mathbb{E} Y_{j_{2n}}$`, where the
choices `$\{j_{1n}, j_{2n}\}$` are based on the data. If we now would just
compute the TMLE of this target parameter, learned on the whole data set, then
we would be using the data twice, causing, in this case, a great degree of bias,
since the contrast was chosen to optimize the effect of interest.

In such cases of data-adaptive target parameters, we use _CV-TMLE_. That is, we
use `$V$`-fold sample-splitting, run this algorithm on the training sample
`$P_{n, v}$`, thereby determining `$j_{1n,v}$` and `$j_{2n,v}$`; then, we
compute the TMLE of `$\Psi_{n, v}(P) = \mathbb{E}_P Y_{j_{1n, v}} -
\mathbb{E}_P Y_{j_{2n, v}}$` by getting the initial outcome regression
`$Q_{n, v}$` and treatment mechanism `$g_{n,v}$` from the training sample
`$P_{n, v}$`, but perform the TMLE update step only using the validation sample
`$P_{n, v}^1$`. This yields a TMLE `$P_{n, v}^{\star}$`, and thereby a plug-in
estimator `$\Psi_{n, v}(P_{n, v}^{\star})$`. Repeating this for
each of the `$V$` sample splits, we have `$V$` CV-TMLEs, over which we compute
the corresponding average `$1/V \sum_{v = 1}^V \Psi_{n, v}(P_{n, v}^{\star})$`.

This is an estimator of the data-adaptive target parameter `$1/V \sum_{v = 1}^V
\Psi_{n, v}(P_0)$`, i.e., an average over `$V$` data-adaptively selected causal
effects, defined as moving `$A$` from bin `$j_{1n, v}$` to bin `$j_{2n, v}$`. We
then also provide inference, such as confidence intervals for this data-adaptive
estimand. By utilizing the full sample size, the procedure is as powerful as if
no sample-splitting had been used (normally, people use one sample to learn the
question, and the second sample to obtain inference). This is the price one has to pay
when the question is not under the full control of the user, but it will end up being an
average of causal effects. In some cases, each training sample might generate the
same contrast (e.g., when the choice of contrast is very stable and clear), while,
in other cases, there might be variation in the selected contrasts. Either way,
if we reject the null, then we know that one of these contrasts was
significantly different from zero, so that is a good hypothesis test for an
interesting null hypothesis. The `varimpact()` package provides additional
output, like providing inference for `$\Psi_{n, v}(P_0)$` for each fold `$v$`
separately, using this `$v$`-specific CV-TMLE, but the sample size is now only
`$n/V$`, thereby limiting the power (but nonetheless nice to see which effect
estimates where driving the average). There is also a chapter on this in
the 2018 book [_Targeted Learning in Data
Science_](https://www.springer.com/us/book/9783319653037).

So, yes, the procedure can discretize in more than two levels of `$A$`. [Alan
Hubbard](https://publichealth.berkeley.edu/people/alan-hubbard/) knows the
details of this, and so does [Chris Kennedy](https://ck37.com/), and I presume
there is documentation as well. I imagine that can be controlled as well, and
for sure a small modification in the program would add that flexibility. In
addition, the program does indeed both generate the target parameter and the
CV-TMLE, so it stands alone.

It will be nice if you use this program, since these programs can be iteratively
tested and improved over time, as well. We should also add it to the [`tlverse`
toolbox](https://github.com/tlverse), if not done so already. [Jeremy
Coyle](https://github.com/jeremyrcoyle) is a good reference to communicate with
as well, if you are interested in questions of what
[`tlverse`](https://github.com/tlverse) offers for such data adaptive target
parameters. It already includes the [optimal dynamic treatment and its
CV-TMLE](https://github.com/tlverse/tmle3mopttx), so I suspect something very
close is there already. In particular, [Andrew
Mertens](https://scholar.google.com/citations?user=SpcXHmIB8NAC&hl=en) used the
[`tlverse`](https://github.com/tlverse) software to carry out a large
meta-analysis involving data-adaptive target parameters; I believe it involves
searching for contrasts/optimal rules, across many variables, treating each
variable as a treatment, and others as confounders.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
