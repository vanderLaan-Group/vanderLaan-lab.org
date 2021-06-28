+++
title = "Feature Engineering with Large Datasets"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2021-05-01T15:28:00+00:00"
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
> In the article about why we need a statistical revolution at the beginning of
> the [`tlverse` book](https://tlverse.org/tlverse-handbook/), you discuss the
> "Art" of statistics, and describe a scenario where confounders for a logistic
> regression are chosen and characterized in such a way to yield a statistically
> significant result, potentially after multiple iterations that produce an
> estimate that is not significant. You argue that super learning is
> a statistical approach that can learn from the data, and that machine learning
> algorithms can use the data to decide on the confounders that should enter our
> model. Our question is, how do we reconcile this goal with the data cleaning
> and feature engineering that often needs to occur, particularly in big
> datasets, prior to using super learner. Is there an "honest" way to use
> subject matter expertise and process data without undermining the ultimate
> goal of letting the data tell us which covariates are important and/or
> contribute significantly to predicting our outcome of interest?

> As an example, consider a prediction question that uses NHANES data. NHANES
> includes over 5000 potential features in a given 2-year cycle. One category of
> questions centers around oral health and dentition. There are 3 questions
> asked about each tooth in the survey respondent’s mouth. With 32 teeth in
> a human adult mouth, this means there are 96 features centered on capturing
> dental health. If data-adaptive estimators decide which features should enter
> our model, it's hard to believe any individual tooth (among the 96
> tooth-specific questions) would be more important than another. However, it
> seems possible that some aggregation/amalgamation of these 96 covariates might
> produce a single meaningful predictor. While this is an extreme example, there
> are many other clusters of questions asked in NHANES that could potentially be
> combined, and formatted in ways that may or may not affect prediction of our
> outcome of interest. This feature engineering/pre-processing step would occur
> prior to using screeners and piping them to a modeling algorithm.
>
> Best,
>
> L.T. and A.S.

---

## Answer:

Hi L.T. and A.S.,

Thank you for the excellent question and nice examples.

I think we need to define what feature engineering really means.

To start with, you can feature-engineer as much as you want, if it is not based
on the outcomes `$Y$`. So, one can use principal components analysis, and add
them as covariates; one can create summary measures from a set of variables; and
so on. In your example, it makes sense to augment the data set with such summary
measures -- just as sometimes doctors know that certain ratio of biomarkers are
particularly important and could then add them without throwing away the two
biomarkers themselves. This is all good practice for helping the super learner.
If, on the other hand, we want to create features based on the outcome, then
these algorithms need to be used in a pipeline for constructing a candidate
learner in the library of the super learner, so that cross-validation takes into
account the variability due to taking that additional step.

Again, very good practice to think about smart, informed ways of building
features based on outcomes as well. This could be used to augment the set of
covariates or to select a sparser subset of features that then gets inputted in
the ML algorithm. The beauty of the super learner is that it allows us to do all
that and still only improve the overall performance of the super learner. It is
also an effective way to build algorithms that run in a reasonable amount of
time since giving these algorithms too many variables might not only
statistically hurt them, but also hurt the computer running-time of the
algorithm.

Being careful about what should go into pipeline and what is non-informative is
very important. It is very easy to fool oneself into thinking that the
predictions are great, due to first screening based on `$Y$` and then running ML
or SL ignoring this, making the CV-risk look much better than it should.

In this manner, we still make sure that our total SL or total TMLE is _a priori_
specified and can be formally analyzed and its sampling distribution can be
estimated, so that inference is still possible. Outcome-blind data can be used
in any interactive (human-dependent manner), way.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!