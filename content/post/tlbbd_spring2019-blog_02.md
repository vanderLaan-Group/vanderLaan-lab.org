+++
title = "TMLE versus the one-step estimator"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2019-05-10T19:23:00+00:00"
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
> Is there any theoretical guarantees about relative performances between TMLE
> and the one-step estimator in finite sample conditions?
>
> Thanks,
>
> H.R.B.

---

## Answer:

Hi H.R.B.,

Finite sample guarantees are very hard to obtain. One can obtain finite-sample
confidence intervals by, for example, not relying on a CLT but on finite-sample
inequalities for sample means (e.g., Bernstein's inequality). However, these
will naturally be conservative, by necessity, since otherwise they would not
give the finite-sample guarantee. Therefore, the utility of such finite-sample
confidence bands appears to be limited. Michael Rosenblum and I have an article
on a confidence interval that is asymptotically equivalent with what CLT
provides (thus exact asymptotic coverage) and provides a finite-sample guarantee
for a user-supplied simple parametric model. That approach appeared to be
reasonable without being overly conservative. We often have problems with
underestimating the uncertainty due to ignoring second order remainders or
underestimating the variance of the influence curve (e.g., by not using
a substitution estimator). So these issues are immensely important. Currently,
we have a recent article (Cai & van der Laan) in which we use the nonparametric
bootstrap with HAL-TMLE (i.e., using HAL for initial estimator) to obtain a
confidence interval that takes into account the second-order behavior, and
appears to have excellent finite-sample coverage even when the true functions
are getting more and more complex at fixed sample size.

However, you are asking about finite-sample guarantee of one estimator being
better than another. We can do the usual exact second-order expansion of both
TMLE and the one-step estimator. This shows that TMLE minus the truth equals the
empirical process applied to the efficient influence curve at the TMLE plus the
exact second-order remainder `$R_2()$` at the TMLE, while the one-step estimator
minus the truth equals the empirical process applied to the efficient influence
curve at the initial estimator plus the same exact second order remainder at the
initial estimator. So the only difference in these exact second order expansions
is that for the TMLE both the efficient influence curve and the second-order
remainder are evaluated at the TMLE, while for the  one-step they are evaluated
at the initial. This does give us insight. In particular, if there are serious
positivity issues, and the TMLE update is not done in a robust manner as with
C-TMLE, and only aiming to solve the efficient influence curve up till minimum
required bound (e.g., `$\frac{\sigma_n}{n^{1/2} log(n)}$` say, where
`$\sigma^2_n$` is sample variance of estimated efficient influence curve), then
the TMLE update could be worse than the initial and therefore also hurt the
second-order remainder and empirical process term. On the other hand, if the
target estimand does not have lack of positivity, then a little extra fitting
step applied to initial estimator (by running MLE on least favorable submodel
through initial) will only improve the fit and might therefore easily reduce
second-order remainder. So, achieving this goal of guaranteed improvement
relative to the one-step estimator teaches us that we should invest enormously
in carrying out an as robust TMLE update step as possible. This has been the
motivation of C-TMLE, including C-TMLE truncation selection for the intervention
mechanism, but also of penalizing the TMLE step so that it does not go for the
full MLE of `$\epsilon$`, but only aim to solve the efficient influence curve
equation approximately. In addition, we have been building in extra `$\epsilon$`
parameters in the least favorable submodel that are aimed at reducing the
second-order remainder, thereby giving a real benefit to TMLE relative to a
one-step that is not able to do these extra targeting steps (and also not able
to do targeting of the intervention mechanism `$g$` as naturally carried out
within TMLE framework, i.e., C-TMLE).

Finally, the above comparison ignores the fundamental difference that a TMLE is
a plug-in estimator and can therefore never go out of bounds (e.g., estimate
probability with negative number), while that is not true for one-step
estimator. So a single observation can destroy the one-step estimator to the
point of going outside parameter space, while that is not possible within the
TMLE framework.

To conclude, I believe thinking carefully about this question already can result
in finite-sample improvements for a TMLE implementation. It makes you very
worried about any outlier type thing going on in data and make the estimator
robust to these. In our past, current and future work this is precisely the
type of TMLE we are developing, one that is ready for all kinds of finite-sample
challenges and remains robust. Even though finite-sample guarantees of TMLE
outperforming a one-step are too much to ask, our past and ongoing developments
are precisely  aiming for an estimator that is never much worse at all ,while
often better, and often theoretically (asymptotically) superior.

Most of our advances have been motivated by finite-sample considerations, even
though they also result in asymptotic theoretical improvements: Super Learning,
HAL, C-TMLE, CV-TMLE, C-TMLE truncation selection. The beauty of the TMLE
template is that it is so general and flexible that any challenge can be
attacked. For example, recently, Bibaut et al. wrote a paper aiming to develop
a TMLE that beats the top proposals from the computer science literature. These
latter proposals are not worried about bias of order `$\frac{1}{\sqrt{n}}$` but
are only worried about MSE. That made us  realize that we should even put more
of a brake on the TMLE-update step so that it only reduces bias till
`$\frac{1}{\sqrt{n}}$` level, and indeed they succeeded in outperforming the
best computer science proposal: i.e., one sets the challenge, one aims to
understand why a competitor does so well for this challenge, and then one
tailors TMLE to solve the challenge.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!