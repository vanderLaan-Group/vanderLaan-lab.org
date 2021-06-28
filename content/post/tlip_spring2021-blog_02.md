+++
title = "Super Learning and Interaction Terms"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2021-05-01T15:13:00+00:00"
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
> I have a question about the step in the Super Learning framework where
> interaction terms can be added between certain covariates. Is there
> a principled way to decide what interactions terms should be added from the
> data alone, or do all interaction specifications have to be based on prior
> knowledge of the system in question? Because our cross-validation procedure
> helps prevent overfitting, it seems like there wouldn’t be a drawback to
> including many interaction terms, except for increased computational
> complexity. Is this the case, or is there a reason not to incorporate too many
> interaction terms? Thanks!
>
> Best,
>
> A.V.

---

## Answer:

Hi A.V.,

Thank you for the excellent question. Many algorithms build interactions such as
random forest, trees, MARS, HAL, etc. On the other hand, an algorithm such as
[`glmnet`](https://glmnet.stanford.edu/articles/glmnet.html) will run with the
covariates as main terms so you will have to augment that set if that is what is
needed. It is good practice to include learners like
[`glmnet`](https://glmnet.stanford.edu/articles/glmnet.html) with different sets
of variables, including interaction term sets. Penalized regression algorithms
like [`glmnet`](https://glmnet.stanford.edu/articles/glmnet.html) are robust,
and by giving them different sets of covariates to work with you can create
a powerful super learner. Another important consideration is that finding
interactions in data comes with noise, so that you help an algorithm by already
putting down interaction terms as main terms, since it is easier for the
algorithm to check if these main terms are important. So,
a [`glmnet`](https://glmnet.stanford.edu/articles/glmnet.html) with smart
selection of interactions might perform better than a random forest. Similarly,
the [highly adaptive lasso (HAL)](https://github.com/tlverse/hal9001) will do
better if you can give it already a nice subset of interaction terms to
consider, in that case defined by selecting knot-points. So pre-screening by
removing variables as well as by augmenting main terms with interactions can
significantly improve the super learner, and one can use different cutoffs so
that one is not betting on one particular type of screening/augmenting.

You could decide to augment the covariate columns with all possible
interactions. Indeed, that might make things computationally intensive. It is
not really true that this is statistically better than giving it a smaller set
of interactions. If you give it a smaller set, and these are covering the
important terms, the algorithm will do better than when it is given a huge list
of covariates. So, some smart thinking for selection of extra covariates such as
transformations of variables, ratios, etc., that make sense, can be a real help.
Fortunately, super learner allows one to try out all kinds of strategies by
including them as candidate learners, and, in this manner, one can avoid having
a single algorithm that takes forever to run.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!