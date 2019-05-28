+++
title = "Prediction intervals using the TMLE framework"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2019-05-11T16:24:00+00:00"
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

<u>**Question:**</u>

> Hi Mark,
>
> We are curious about how to use TMLE and influence curves for estimation and
> inference when the target parameter is a conditional expectation, rather than
> a scalar.
>
> Specifically, suppose I have a data structure `$O = (W, Y) \sim P_0$`, and
> sample `$n$` times i.i.d. from `$P_0$`. We are interested in estimating the
> functional `$\Psi(P_0) = \mathbb{E}[Y \mid W]$`. This seems like a perfect
> place to use Super Learning to estimate the target parameter, and Super
> Learning is indeed often used for these kinds of prediction problems.
>
> From Super Learner, we can get a good estimate of `$\mathbb{E}[Y \mid W]$`,
> and it seems like there is no reason to use a TMLE to update this fit.
>
> However, for each individual (with covariate `$W_i$`), we would also like to
> obtain a notion of a confidence interval and/or a prediction interval. Is
> there a way to use influence functions or TMLE to obtain these intervals? It
> seems like this could tie closely to the simultaneous confidence intervals we
> discussed earlier in the semester.
>
> J.R., D.C., and M.M.

---


<u>**Answer:**</u>

Hi J.R., D.C., and M.M.,

You are interested in providing statistical inference for
`$\Psi_0(w) = \mathbb{E}(Y \mid W = w)$` based on observing `$n$` i.i.d. copies
of a random variable `$(W, Y)$` with a nonparametric model for the probability
distribution. You mention that one could use Super Learning (SL) to estimate the
regression `$\mathbb{E}(Y \mid W)$` and that such an approach might yield a good
estimator but lacks inference. I would argue that a Super Learner would be a
non-targeted estimator for this particular target `$\mathbb{E}(Y \mid W = w)$`,
since SL is optimizing a loss-based dissimilarity, often represented by a square
of an `$L^2$`-norm of the candidate function minus the true function
`$\mathbb{E}(Y \mid W)$` in `$W$`. This means that it is optimizing some average
of squared `$w$`-specific errors across all `$w$` in the support of `$W$`.

Our strategy we have presented in chapter 25 of the new targeted learning book
is the following. Let's say `$W$` is `$d$`-dimensional. We would first
approximate `$\psi_0(w)$` with
`$\psi_b(w) = \int_{x} b^{-d} K\left(\frac{(x-w)}{b}\right) \psi_0(x) dx$`, for
some kernel `$K$` and bandwidth `$b$`. This is just an example of a
`$b$`-specific approximation of this non-pathwise differentiable target
parameter. Other strategies could be considered such as approximating
`$\psi_0(w)$` with a family of pathwise differentiable approximate target
parameters.

`$\psi_b(w)$` is now a pathwise differentiable target parameter. Therefore, we
can develop a CV-TMLE of this `$b$`-specific target parameter. It would rely on
estimating `$\mathbb{E}(Y \mid W)$` over a local neighborhood of `$w$`. One
could use a Super Learner with a local loss function that only involves the fit
over that neighborhood. In that manner, the candidate estimators could still
involve extrapolation, but the evaluation of performance over validation sample
only evaluates how good the fit is in the local neighborhood. So this is already
a much more targeted Super Learner. We use CV-TMLE instead of TMLE, since as
`$b$` converges to zero the efficient influence function of `$\psi_b(w)$`
becomes unbounded, so that we don't want to have to rely on a Donsker class
condition. Instead we only have to deal with an empirical mean over validation
samples, where conditionally on training sample, it will essentially be a sum of
i.i.d. observations (up till a univariate `$\epsilon_n$` that is easily
handled), thereby allowing us to obtain a CLT under a variance converging
condition only.

Therefore, one can then establish that
`$(nb^d)^{1/2}(\psi_{\text{CV-TMLE}, b}(w) - \psi_b(w))$` converges to a normal
distribution with mean zero and an asymptotic variance driven by the variance of
the normalized efficient influence curve (normalized so its variance actually
converges), where we can let `$b$` converge to zero as fast as a determined rate
(which requires investigating the second-order remainder, making sure it is
smaller order than the leading empirical sum term). Subsequently, we then have
to determine a selector of `$b_n$` that does a good job minimizing MSE
w.r.t. the actual `$\psi(w)$`. We have developed such a method based on
tracking, as `$b$` goes from large to small, the change in standard error of the
efficient influence curve and change in TMLE, and when that reaches a balance,
then we have found our desired `$b$`. We can prove this optimizes the MSE at a
rate that would be optimal if one would know the underlying smoothness (where we
use orthogonal kernel `$K$`), even though we don't this smoothness of the true
function.

By slightly undersmoothing this data adaptive choice (`$\log(n)$` factor), then
we obtain a normal limit distribution for
`$(nb_n^d)^{1/2}(\psi_{\text{CV-TMLE}, b_n} - \psi(w))$` with mean zero and same
asymptotic variance as above. So, now we obtain a valid asymptotic normal based
confidence interval.

The reason why we succeed in establishing the desired asymptotic convergence in
distribution is because we use CV-TMLE of the `$b$`-specific approximation, and
the convergence in distribution of the CV-TMLE holds uniformly along sequences
`$b_n$` that do not converge too fast to zero as a function of sample size.

One might also simply decide to be satisfied with inference for `$\psi_b(w)$`
itself, which still has a good interpretation. Even though this appears to be
a promising excellent approach for `$W$` being not too high-dimensional; for
high-dimensional `$W$`, I think the second-order remainders will dominate. I
think that determining a data adaptive approximation of `$\psi(w)$` first (e.g.,
select a data adaptive large parametric model, or HAL-fit, and treat that as a
working model on which the function `$\psi(\cdot)$` is projected and evaluated
at `$w$`), and then being satisfied with the resulting `$\psi_b$` target
estimand, is then a sensible approach.

You also ask about the construction of an interval `$(a_n(w),b_n(w))$` so that
`$\mathbb{P}(Y \in (a_n(w), b_n(w)) \mid W = w)$` converges to `$0.95$`. This is
clearly a very different problem. For example, this interval will never shrink
to zero width. Just talking from the top of my head, I could see that we might
focus on estimating a CDF `$F_w$` defined by `$y \to P(Y \leq y \mid W = w)$`,
i.e., the conditional CDF of `$Y$`. Its quantiles would then provide the desired
prediction interval.

Now, as above we might approximate `$F_w$` by a smoothed CDF `$F_{w, b}$`, e.g.,
using the kernel smooth above.

For a fixed `$b$`, we can then develop a CV-TMLE of the whole function
`$F_{w, b}$` using our universal least favorable model for multivariate target
parameters. For each fixed `$b$`, the CV-TMLE minus the CDF `$F_w$`, normalized,
would converge to a Gaussian process. This would already provide us with valid
prediction intervals based on `$F_{w, b}$`. We can then also determine along
what sequences `$b_n--0$` we can generalize this weak convergence proof, and
then among this class of possible sequences, develop a data adaptive selector
`$b_n$` that optimizes MSE for our target. Lots of details to be worked out,
but, clearly, one sees again that approximating the nonpathwise differentiable
CDF `$F_w$` by a family of pathwise differentiable CDFs `$F_{w,b}$`, developing
CV-TMLE of these infinite-dimensional `$F_{w,b}$`, and doing our usual proof for
CV-TMLE, etc., provides the formal theory for this approach.

You are referring to simultaneous confidence intervals. Clearly, the weak
convergence of the CV-TMLE of the whole CDF concerns the same weak convergence
result we would need for constructing a simultaneous confidence interval. So,
there is definitely a relation. There is also a functional delta-method argument
that can be used to establish that convergence of the CV-TMLE of the CDFs also
implies the weak convergence of its quantiles, as needed for our prediction
interval.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
