+++
title = "Adaptive algorithm selection via the Super Learner"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2019-05-11T13:54:00+00:00"
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
> A couple questions I have are about super learning and the strength of the
> learners as well as potentially adaptively choosing learners. Is there any
> advantage, theoretical or practical, of having a large library of weaker
> learners over a small library of stronger learners?
>
> Furthermore, given an initial library is there any way to adaptively choose
> new learners?
>
> N.A.

---

## Answer:

Hi N.A.,

_To address your first question:_ Is there any advantage, theoretical or
practical, of having a large library of weaker learners over a small library of
stronger learners?

To address this question I refer to the oracle inequality for the discrete super
learner that compares the loss-based dissimilarity w.r.t. the truth of the
cross-validated selected estimator with the oracle-selected estimator, among the
candidate estimators, where both these selectors make their decision based on
the algorithms trained on `$n(1-p)$` observations, where p is the proportion of
validation sample. This oracle inequality teaches us that the ratio of these
dissimilarities converges to `$1$` as long as the number of candidates in
library is polynomial in sample size (and none of candidate estimators achieves
the parametric rate of a correctly specified parametric model), which we refer
to as asymptotic equivalence of the cross-validation selector with the oracle
selector.

From that we learn that what matters for the discrete super learner is that the
we want a library where the best possible choice is as good as it gets.

Let's now consider a Super Learner that uses a real meta learning step the set
of candidate estimators is all weighted combinations in the library of
estimators. The same result applies but now the number of estimators in the
library can only grow slowly with sample size (since the weighted combinations
already represents a rich set of estimators).

So what matters now is what is the best (oracle) possible choice among all
weighted combinations of the estimators in the specified library. If all the
estimators in the library are heavily correlated, then the weighted combinations
will not be such a stellar set of candidate estimators relative to the discrete
set of estimators in the library. However, if the estimators in the library are
somewhat uncorrelated, say in the sense that different choices might perform
strongly in different areas of the covariate space, then doing a fancy meta
learning step that aims to discover what algorithm to use for different subsets
in the covariate space might be a great improvement. If the  meta learning step
is defined by fitting a weighted average, then I could see that a set of weak
learners could generate very good performing weighted averages.

It is relevant to refer here to the thesis work of Stephanie Sapp, and
corresponding articles co-authored with Erin LeDell, on the subsemble algorithm.
In this work the candidate estimators are a particular algorithm applied to
a subset of the data, across a partitioning of the data set, so that the
weighted combinations are literally taking a weighted average of estimators
applied to independent and small complementary data sets. Since the weights are
not fixed _a priori_, but chosen with cross-validation, we observed that the
bias of such estimators gets heavily reduced during meta learning step, so that
the subsemble is not more biased than the bias of the algorithm applied to
whole data set (even though the machine learning algorithm in question is
heavily nonlinear).

The subsemble algorithm is literally an application of the Super Learner
procedure to a library of algorithms defined by a particular algorithm applied
to a subsets of data corresponding with a partitioning of the data set. These
subsets do not have to be random splits, but could even be based on clustering
etc. An interesting feature of subsemble is that it is also good for big data by
avoiding running an algorithm on large data sets (e.g., computing time of this
algorithm grows exponentially or so as sample size increases).

When we refer to weak learners we might also be referring to an algorithm that
represents an overfit of the data as each tree random forest is generating.
Again, a weighted average can deal with the large variance of each estimator, so
that such a super learner might perform well as well, even though the discrete
Super Learner would be terrible.

_Your second question:_ Furthermore, given an initial library is there any way
to adaptively choose new learners?

One can define  a discrete super learner where the candidate estimators are
themselves  super-learners based on using  a set of `$k$` estimators, for a
sequence of integer values for k. So one can now line up a very large collection
of candidate algorithms, order these algorithms, and the overarching discrete
Super Learner will now select the number `$k$` of algorithms (top `$k$` in list)
one should use in the Super Learner. Since the discrete Super Learner is very
robust it will do a good job in selecting `$k$`, so that one does not need to
worry much about the fact that, for large `$k$`, the `$k$`-specific Super
Learners might involve weighted combinations and suffer from overfitting the
cross-validated risk in the meta-learning step.

There is also nothing wrong with defining the `$k$`-specific "Super Learner"
involving a step that picks a particular `$k$` algorithms based on the data
itself. Of course, then it is not really a Super Learner since it involves a
pre-selection step, but, again, the overarching discrete super-learner will make
a good selection among all these different `$k$`-specific algorithms. The
overall idea is that if one wants to get very fancy with the meta-learning step,
maybe along a sequence of more and more aggressive meta-learning steps; then,
that is perfectly fine as long as we choose among all these Super Learners
indexed by a `$k$`-specific meta-learning step with the discrete super learner.
These are exactly the type of Super Learners Gilmer Valdes's group at UCSF, in
collaboration with us, is building using CART-type meta-learning steps so that
the resulting Super Learner chooses a possibly different estimator depending on
the covariate value. Dr. Valdes has a lot of clever ideas about how to make the
meta-learning step fancier (like using boosting instead of CART, etc.), but such
clever algorithms are delicate and are easily resulting in overfits. So by
having an overarching discrete Super Learner one can be as clever as one wants
to be in generating very fancy and interesting candidate meta-learning steps,
not having to worry about many of these resulting in overfits of the data, as
long as the best meta-learning choice performs well. In the same spirit, we
could define the `$k$`-specific Super Learner as the one that uses Lasso
regression in the meta-learning step with an L1-norm bound defined by a number
`$\lambda(k)$` where `$\lambda(k)$` is increasing in `$k$` from small-to-large
values. Note that in this case the meta-learning step always uses a very large
number of algorithms (say, maximal set), and by setting `$\lambda(k)$` it will
data adaptively select the best subset (since most coefficients will be set to
zero in a Lasso regression). Of course, as `$\lambda(k)$` increases one can
expect the meta-learning step to suffer from overfitting, but, again, the
overarching discrete Super Learner will select the correct `$k$` and thus
`$\lambda(k)$`. In this case, we don't need to order the list of all estimators
a priori, since the lasso will just create a sequence of subsets growing in size
(say, one at a time), not necessarily a nested sequence of subsets either. So,
maybe this latter type of meta-learning is most directly answering your
question.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!