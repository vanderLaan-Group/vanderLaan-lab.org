+++
title = "Leave-$p$-out Cross-validation"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2017-11-22T15:40:00+00:00"
categories = [
    "Resources",
    "Statistics",
    "Targeted Learning",
    "Q&A"
]
+++

_This post is part of our Q&A series._

A question from two graduate students in our Fall 2017 offering of "Survival
Analysis and Causality" at Berkeley:

<u>**Question:**</u>

> Hi Mark,
>
> [We] were wondering what the implications were for selecting leave one
> observation out versus leave one cluster out when performing cross-validation
> on a longitudinal data structure. We understand that computational constraints
> may render leave one observation out cross validation to be undesirable,
> however are we implicitly biasing our model selection by our choice in
> cross-validation technique?
>
> Best,
> T.C. and R.P.

---

<u>**Answer:**</u>

Hi T.C. and R.P.,

We have a theoretical oracle inequality for leave a proportion $p$ out. The
finite sample remainder $C(M_1, M_2, \delta) \text{log}\frac{K_n}{n \cdot p}$ in
this finite sample oracle inequality has this proportion $p$ in there so that
this particular inequality suggests that we do a worse job in approximating the
oracle selected estimator as $p$ gets small for a fixed $n$.

As in our original articles, we also pointed out that one can let $p = p(n)$
converge to zero slowly enough and still obtain asymptotic equivalence with the
oracle selector. Since the oracle selector gets better as $p$ gets smaller
(since it selects best estimator when trained on $n \cdot (1 - p)$ observations
and we like that to be $n$ observations), this shows that from an asymptotic
perspective one wants to let $p$ converge to zero as $n$ converges to infinity.

To put it another way, $p$ could be viewed as a tuning parameter, which would
then require proposing a method that selects the choice $p$ data adaptively.That
would be an interesting research topic, we never dived into. For example, one
could define a super-learner based on a $V$-fold cross-validation and a given
library. That super-learner is now indexed by a choice $V$ (or $p$ more
generally). We could now create a super-learner that uses as candidate
estimators these $V$-specific super-learners, which would then select $V$.
However, there is a circular argument since that outer super-learner will also
have to select a choice $V_0$ for its own cross-validated risk. Nonetheless, one
might see that the best performance is achieved by a $20$ fold super-learner
w.r.t. a $V_0 = 10$ fold evaluation. I could also imagine that simply checking
the CV-risk of each $V$-specific super-learner might suggest a good choice of
$V$: theoretically, i.e .when $n$ is very large, increasing $V$ should reduce
the true risk, but in finite samples, we might see that this monotonicity holds
for $V$ from $2$ until $12$ but after $12$ it becomes erratic, suggesting $12$
is a good choice.

The fact that there is a lack of theoretical results for leave-one-out
cross-validation, is itself not an argument against it. I believe it often
works. People such as the late Leo Breiman in our Statistics department have
suggested that the variance of the selector increases as $p$ gets small, and he
generally recommended (based on his extensive practical experience) $p = 0.1$.
So, based on purely statistical considerations, the optimal $p$ is probably not
going to be $\frac{1}{n}$ (i.e., leave-one-out cross-validation).

Given that one leaves a proportion $p$ out of $n$ for the validation sample, it
would be optimal to average over all possible splits in $n \cdot p$ validation
and $n \cdot (1 - p)$ training observations. However, one can show that using
$V$-fold (with $V = \frac{n}{p}$) is in first order as good, so that extra
averaging only results in second order improvements. Using single-split
cross-validation is significantly worse than $V$-fold. One average the
cross-validated performance across repeated $V$-folds sample splits (fixed $V$)
until one observes no meaningful difference anymore in the cross-validated risk
and the selector/super-learner. That is what we have used in important data
analyses to make sure we are not affected by variability due to the particular
sample splits chosen.

Overall, cross-validation does a good job in approximating the corresponding
oracle selector as evidence by the finite sample oracle inequality and based on
practical experience. So it is not an issue of bias, but it is doing what you
direct it to be doing. If you use $2$-fold, then it does a good job
approximating the best selector among estimators trained on only half of
observations, so that might not be what you want, in which case one should not
have chosen $2$-fold.

More recently we have developed online cross-validation results, where online is
a form of leave one out cross-validation, but in the context of an ordered
sequence of observations and the estimator is trained on the previous
observations. These results also suggest that leave one out is not necessarily
a bad idea. The online cross-validation is motivated by online learning, in
other words, motivated by computational considerations so that we can construct
a scalable super-learner that is online. Online cross-validation does probably
does worse statistically than $V$-fold for a good choice of $V$, even when one
would average across $V$ orderings of the observations. So computational
considerations are often a big player in the decision making.

Best,
Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!

