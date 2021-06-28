+++
title = "CV-TMLE and double machine learning"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2019-12-24T12:43:00+00:00"
categories = [
    "Resources",
    "Statistics",
    "Targeted Learning",
    "Q&A"
]
+++

_This post is part of our Q&A series._

A question from Twitter on choosing between double machine learning and TMLE
with cross-validation: https://twitter.com/emaadmanzoor/status/1208924841316880385

## Question:

> [@mark_vdlaan](https://twitter.com/mark_vdlaan) Is there an applied
> researcher's guide to choosing between double machine learning and TMLE +
> cross-fitting? PS: Thanks for making these methods and resources so easily
> accessible!

---

## Answer:

Thanks for this interesting question. In the past several years, the interest in
these machine learning-based estimators has become more widespread, since they
allow for the statistical answer to a question to be framed in terms of
scientifically meaningful parameters (e.g., defined through causal inference),
incorporating machine learning in the estimation process, while providing formal
statistical inference.

In order to provide a comprehensive answer, it is suitable to first define the
different classes of asymptotically efficient estimators. To start with, to
refer to a class of estimators as double machine learning (DML) estimators
suggests that this class only applies to a particular subclass of estimation
problems that allow for double _robustness_, or, involves machine learning of
multiple nuisance functions. Here, we are reminded that this robustness refers
to preservation of the consistency of the estimator under inconsistent
estimation of one or more of its nuisance parameters. Different estimation
problems have different structures, ranging from no form of robustness (i.e.,
consistency of the estimator under misspecification of a nuisance function), to
double robustness, triple robustness, an array of configurations of robustness,
to complete robustness. Therefore, (wrongly) terming an estimator in terms of a
very specific form of robustness suggests that it is a very limited procedure
fully relying on this very specific type of estimation problem. In addition, in
many estimation problems, one only has to apply machine learning to a single
functional parameter.

Efficient estimators are generally defined and will be double robust if the
estimation problem falls in that category, and, in general, can be tailored to
be, or is naturally, as robust as the estimation problem allows. So, it is more
sensible to discuss different classes of efficient estimators -- as we will see,
the double machine learning approach falls in the category of estimating
equation-based estimators. Efficient estimators rely on estimating nuisance
parameters (functionals of the data distribution), and, if the statistical model
for the data distribution is large, this would naturally require machine
learning (also called data adaptive estimation). Thus, any of these efficient
estimators could be called machine learning-based estimators, and, in fact, they
have been applied using machine learning from very early on (circa 1980) in the
literature.

We will only consider target parameters (which we will denote with `$\psi$`)
that are so-called _pathwise differentiable_, making them potentially estimable
at `$\sqrt{n}$`-rate. For simplicity, let's focus on the case that we observe
`$n$` independent identically distributed copies of a random variable `$O$` with
true data distribution `$P_0$`. Note that all we really need is local asymptotic
normality of the log-likelihood; therefore, this also applies to time series and
other dependent data structures (e.g., see [_Efficient and Adaptive Estimation
for Semiparametric
Models_](https://books.google.com/books/about/Efficient_and_Adaptive_Estimation_for_Se.html?id=lSnTm6SC_SMC)
by Bickel, Klaassen, Ritov, and Wellner). So, what does it mean to state that an
estimator of a pathwise differentiable target parameter is _asymptotically
efficient_ at a given data distribution? An estimator is asymptotically
efficient if it can be approximated in first order as an empirical mean of the
so-called canonical gradient of the pathwise derivative of the target parameter.
To determine the canonical gradient, one views the estimand as a mapping from
all possible data distributions in the statistical model, say `$\mathcal{M}$`,
to the parameter space (typically, the real line `$\mathbb{R}$`) and determine
its pathwise derivative along a rich collection of paths through the data
distribution -- this pathwise derivative is uniquely characterized by its
canonical gradient. Owing to its use in the definition of efficient estimators,
the canonical gradient at a data distribution is also called the _efficient
influence curve_ (`$\text{EIC}$` or `$\text{EIC}(P)$` at a particular data
distribution `$P$`). Recall that an estimator is _asymptotically linear_ if it
can be approximated by an empirical mean of a particular function of the unit
data structure $O$; the influence curve is this same particular function.
Importantly, an efficient estimator is asymptotically the best (i.e., most
efficient) among the class of all regular asymptotically linear estimators --
or, in fact, among all regular estimators. What's more, an efficient estimator
is asymptotically normally distributed with asymptotic variance equal to the
variance of the efficient influence curve.

Given a statistical model `$\mathcal{M}$` (i.e., a set of possible probability
distributions of the unit-level data `$O$`) and that the target parameter
`$\psi$` is a mapping from the statistical model to the parameter space (e.g.,
the real line `$\mathbb{R}$`), the statistical estimation problem is now fully
defined, and one can proceed to compute the canonical gradient of the pathwise
derivative of the target parameter. Perhaps unsurprisingly, the construction of
an efficient estimator necessarily involves using the canonical gradient. The
three classes of such efficient estimators are composed of

1. the one-step estimator (OSE);
2. the estimating equation estimator (EEE); and
3. the targeted minimum loss estimator (TMLE).

The asymptotic efficiency of these estimators relies on a second order term to
be negligible, thereby typically requiring highly adaptive estimators of the
nuisance functions in the canonical gradient (those that will achieve the
convergence rate of `$n^{\frac{1}{4}}$`). This second order term is defined in
terms of the canonical gradient (i.e., `$R_2(P, P_0) = \Psi(P) - \Psi(P_0) +
\mathbb{E}_{P_0} \text{EIC}(P)$` in my work). Additionally, when using variants
of these estimators that do not make use sample splitting, one also then relies
on a Donsker class condition (from empirical process theory), while, if one uses
the sample splitting variants, then this Donsker class condition is not needed.
It is the form that this second order remainder `$R_2(P,P_0)$` takes that
determines the robustness structure of the estimation problem -- for example, in
double robust problems, the term typically involves products of differences
between nuisance function under `$P$` and true nuisance function (i.e., under
`$P_0$`).

Recall that the classical way to obtain an efficient estimator is via maximum
likelihood estimation. Unfortunately, when the statistical model is large, the
the maximum likelihood estimator (MLE) is generally not defined. To work around
this issue, one might consider regularizing the MLE by introducing a tuning
parameter such as a sieve-based MLE, involving data adaptive selection of a
submodel among a collection of submodels that approximate the actual statistical
model. If this tuning parameter in the regularized MLE is optimized w.r.t. the
density itself, the resulting regularized MLE (a plug-in estimator) of the
target parameter will still fail to be asymptotically linear. Despite this,
there remain interesting classes of regularized MLEs in which an undersmoothed
choice of tuning parameter will result in the construction of an efficient
estimator. In recent work, we established this result for the so-called Highly
Adaptive Lasso (HAL) MLE (minimum loss estimation), in which one computes an MLE
over a class of cadlag functions with a universal bound on the sectional
variation norm, selecting the sectional variation norm with an
undersmoothing-based selector. In any case, such regularized MLEs are not
targeted towards a particular estimand and could therefore be considered less
powerful than one of the efficient estimators that utilize the canonical
gradient for a particular estimand. There is more to say about this, including
great work in the literature on this topic by Shen and Newey, among others, but
that lies beyond the scope of this comment.

We mention the HAL-MLE above since it is very relevant for efficient estimation.
Just recently, [Bibaut and vdL (2019)](https://arxiv.org/abs/1907.09244) showed
that the HAL-MLE converges to the true functional parameter at a rate as fast as
`$n^{-\frac{1}{3}}$` up to a `$\log(n)$` factor. Thus, using this HAL-MLE to
estimate the nuisance functions in the OSE, EEE, or TMLE would guarantee that
these estimators are indeed asymptotically efficient. In addition, the HAL-MLE
automatically satisfies the Donsker class condition as well, implying that the
non-sample split variants of the OSE, EEE, and TMLE using will be asymptotically
efficient when constructed via the HAL-MLE; the only assumption embedded in this
approach is that the true nuisance functions are cadlag and have finite
sectional variation norm (with the `$\text{EIC}$` being a bounded function).

With this background, we are now well-positioned to proceed with discussing the
three classes of efficient estimators (OSE, EEE, and TMLE). Along the way, we'll
provide some historical context and discuss how DML fits into this landscape.

Historically, the first efficient estimator was the so called one-step estimator
using sample splitting (the work of Levin, Pfanzagl, and Klaassen circa
1970-1980, see references in the book by Bickel, Klaassen, Ritove, and
Wellner). For example, Klaassen (1986) showed that the one-step estimator using
sample splitting is efficient under minimal conditions. The one-step estimator
(and its non-sample splitting version) is the efficient estimator presented in
the comprehensive book on efficient estimation in semiparametric models
([_Efficient and Adaptive Estimation for Semiparametric
Models_](https://books.google.com/books/about/Efficient_and_Adaptive_Estimation_for_Se.html?id=lSnTm6SC_SMC))
by Bickel, Klaassen, Ritov, and Wellner (1997). This one-step estimator is
defined as a plug-in _initial_ estimator of the target parameter, plus the
empirical mean of the EIC at this same initial estimator. The sample splitting
variant of this estimator is defined as the average across a number of sample
splits in training and validation sample (say, V-fold), of the initial plug-in
estimator based on a training sample plus the empirical mean over the validation
sample of the EIC at this same initial plug-in estimator (fitted on the training
sample). A little later (circa 1980 and onward), as empirical process theory
emerged as a field, the sample splitting based one-step estimator was often
replaced by the "regular" (non-sample slitting) one-step estimator that relied
on the Donsker class condition (i.e., the sample splitting will come at a small
price if the Donsker class condition nicely holds, but one should make sure that
the initial estimator is not overfitted).

In the 1990s, the corresponding approach of estimating equation-based estimators
(EEE) was rigorously developed by Robins and collaborators, in which one
constructs efficient estimators as solutions of the EIC estimating equation,
-- i.e., one sets empirical mean of `$\text{EIC}(\psi, \eta)$` (for some nuisance
parameter vector `$\eta$`) equal to zero -- by estimating the nuisance function
`$\eta$` in the EIC with an initial estimator and solving for the target
parameter `$\psi$`. This relies on the EIC admitting an estimating function
representation, i.e., having `$\text{EIC}(P)$` depend on the data distribution
`$P$` through the target parameter `$\Psi(P)$` and a nuisance parameter
`$\eta$`. If such a representation of `$\text{EIC}(P) = \text{EIC}(\psi, \eta)$`
exists, the one-step estimator is equivalent to the first step of the
Newton-Raphson algorithm for solving the estimating equation. Fundamental work
on EEE was done by Robins and Rontitzky (circa 1992 and onward), and
collaborators, in the context of censored data and causal inference models,
involving clever representations and derivations of the EIC (e.g., the
_augmented IPCW_ representation of the EIC). A comprehensive review and
treatment of the general efficient estimating equation methodology -- going
beyond application to censored data and causal inference models, including its
theory -- is presented in the book [_Unified Methods for Censored Longitudinal
Data and Causality_](https://books.google.com/books/about/Unified_Methods_for_Censored_Longitudina.html?id=z4_-dXslTyYC)
by van der Laan and Robins (2001).

In contrast with EEE, the OSE is always well-defined while the EEE requires

1. the estimating function representation;
2. that a solution of the EIC estimating equation exists; and
3. if multiple solutions exists that we know how to select among them.

On the other hand, asymptotics of the OSE rely on the initial estimator, making
it potentially less robust than EEE. For example, if the estimation problem is
doubly robust (DR), but the initial estimator is not DR, then the OSE is
generally not DR either, even though the EEE will be DR in such a case.
Completely analogously to the OSE, one can use a sample splitting analogue of
the EEE by defining the estimating equation as an average across the sample
splits of the empirical mean over the validation sample of the EIC, in which
nuisance parameters are estimated based on the training sample. In this manner,
the first step of the Newton-Raphson algorithm for solving this sample
splitting-based estimating equation is equivalent to the one-step estimator
based on sample splitting. In the book by van der Laan and Robins (2001), focus
was placed on the non-sample splitting-based EEE; thus, our theorems throughout
that book (as well as the more general theorems in chapter 2) rely on a Donsker
class condition. Specifically, we showed that if one were to use machine
learning-based estimators whose realizations are cadlag functions with finite
sectional variation norm, then this Donsker class condition holds, allowing for
highly flexible machine learning (including the HAL-MLE) to be used.

Here, it is important to note that any gradient of the pathwise derivative of
the target parameter can be used to construct an asymptotically linear estimator
with influence curve equal to that gradient; moreover, by using the unique
canonical gradient, the constructed estimator will be asymptotically efficient.
Indeed, in Robins and Rotnitzky's work, they always refer to the class of all
estimating functions orthogonal to the nuisance tangent space, which is
equivalent with the class of all gradients of the pathwise derivative of the
target parameter. Robins et al. show that the class of gradients can be computed
by determining the class of all functions of the unit-level data that are
orthogonal (in terms of the covariance operator) to all nuisance scores (i.e.,
scores of paths for which the pathwise derivative of the target parameter equals
zero). In problems in which the EIC is difficult to compute, one might decide to
settle for an inefficient OSE or EEE implied by an easier-to-compute gradient
(see the book by van der Laan and Robins, 2001). In an estimation problem with
the double robustness structure, any of such inefficient OSE or EEE based on an
inefficient gradient will still be double robust (i.e., the robustness implied
by the second order remainder `$R_2(P, P_0)$` applies to any gradient, since
what matters is that it is orthogonal to the nuisance scores, not that it has
minimal variance).

A problem with both EEE and OSE (regardless of whether sample splitting is used)
is that these estimators suffer from the fact that they are not plug-in
estimators (i.e., one applies the target parameter mapping to an estimated data
distribution in the model); thus, they might not satisfy crucial global
constraints in the statistical model. For example, if the canonical gradient is
highly variable or if the sample size is small, it can easily happen that the
OSE and EEE end up estimating a probability with a number larger than one or
smaller than zero, thus not respecting the bounds on the parameter (i.e., that
probabilities lie between zero and one, inclusive). Moreover, by potentially
failing to respect these important global statistical constraints in the model,
the OSE and EEE lack robustness (e.g., just one observation could throw off the
estimator badly), in stark contrast to an MLE, which always necessarily fully
respects global constraints of the model. Of course, global constraints are
asymptotically irrelevant, yet are very important in finite samples.

By contrast, TMLE produces an efficient plug-in estimator, performing updates
in the model space so as to avoid any risk of violating bounds on the density of
the data and on the parameter space. Specifically, the targeted maximum
likelihood estimator takes an initial estimator of the density of the data,
constructs a parametric model through this initial estimator with score spanning
the canonical gradient at the initial estimator, and fits the unknown
parameter(s) in this so-called least favorable parametric model (LFM) via MLE.
The resultant targeted density estimator is then plugged in to the target
parameter mapping to obtain the TMLE of the target parameter. The LFM through
the initial estimator can often be chosen to be a standard parametric model
treating as offset the initial estimator. For example, the targeting step in the
TMLE of the ATE can be carried out with a logistic regression using a clever
covariate, using as offset the initial estimator. In more recent work ([vdL
and Gruber, 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4912007/)), we
propose a so-called _universal_ least favorable submodel, which guarantees both
that the targeting step is maximally robust and that the TMLE exactly solves the
EIC estimating equation in a single step. By contrast, the originally proposed
_local_ LFM will solve the EIC equation up to an approximation error that is
asymptotically negligible if the initial estimator achieves the
`$n^{-\frac{1}{4}}$` rate of convergence; however, in this case, iteration of
the TMLE can be used to solve the EIC equation to an arbitrary level of
precision.

In general, one does not need to always estimate the whole data density --
instead, one specifies the required functional of the data density the target
estimand relies upon, determines a loss function and LFM through the initial
estimator of this functional (i.e., since the generalized score spans the
canonical gradient), computes the MLE of the unknown parameters of the least
favorable parametric model, and finally plugs the targeted estimator of the
functional into the target parameter mapping. This plug-in estimator is now
called the _targeted minimum loss estimator_ (TMLE), since it does not require
the loss to be the log-likelihood loss and allows one to focus only on the
relevant functional of the data distribution.

In many problems, the target parameter depends on a collection of functionals
(i.e., nuisance functions) of the data distribution. In that case, one can
separately determine a least favorable submodel for each of these functionals by
requiring its score (w.r.t. a specified loss) to span the corresponding
component of the efficient influence curve. In this manner, one can target each
nuisance function separately or sequentially (e.g., in such a way that a
single-step TMLE already exactly solves the EIC equation). Such a TMLE solves
the empirical mean of each component of the EIC, thus solving the EIC equation.

As with any of the three efficient estimation methods, the TMLE can also be
carried out with sample splitting, a variant which we termed _cross-validated
TMLE_ (CV-TMLE) in [Zheng and vdL
(2011)](https://link.springer.com/chapter/10.1007/978-1-4419-9782-1_27). The
CV-TMLE uses an initial estimator fit on the training sample, carries out the
TMLE updating step on the validation sample, and defines the CV-TMLE as the
average across all of the sample splits of the resultant plug-in TMLE. In fact,
the TMLE update step can also be pooled across the validation samples. Just as
the sample-splitting variants of OSE and EEE, the CV-TMLE is asymptotically
efficient without a need to assume the Donsker class condition, thereby allowing
for overfitted initial estimators of the nuisance functions, so long as
long as the second order remainder `$R_2(\hat{P}, P_0)$` is asymptotically
negligible.

Importantly, the TMLE is a plug-in estimator and thereby fully respects and
incorporates the global constraints of both the statistical model and target
parameter mapping, just like an MLE. A nice example of how TMLE fully utilizes
constraints is given by the [rare outcome
TMLE](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5436729/) (Balzer et al.,
2016), in which both the initial estimator of the prediction function and the
targeted version respect that the predictions are known to be small, greatly
enhancing its performance in finite samples. In contrast to EEE, TMLE does not
rely on the EIC being an estimating function; thus, it is always well defined --
that is, there are no concerns about the existence of multiple solutions or the
nonexistence of a solution. Since one need only compute a simple MLE, even if
that MLE were to allow multiple solutions of its score equation, the empirical
risk provides a criterion for choosing amongst such solutions. We also note that
the TMLE is the only estimator that actually generalizes the MLE -- if the MLE
is well-defined and used as initial estimator, then the TMLE is exactly
equivalent to the MLE (i.e., the targeting step will select zero fluctuation).

In addition, the TMLE procedure is able to augment the least favorable
parametric fluctuation model with additional fluctuation parameters that
generate additional estimating equations for the TMLE to then solve (that is,
beyond the EIC equation). This is simply not possible within the OSE or EEE
frameworks, since these have to commit to one particular gradient (e.g., see
[vdL, 2014](https://www.degruyter.com/view/j/ijb.2014.10.issue-1/ijb-2012-0038/ijb-2012-0038.xml) and
[Benkeser et al.,
2017](https://academic.oup.com/biomet/article/104/4/863/4554445) demonstrating
this nicely). These additional fluctuation parameters can then be chosen so that
the TMLE obtains additional statistical properties beyond asymptotic efficiency.
In this manner, we have proposed a very general TMLE procedure that

1. is guaranteed to be more efficient than a user-supplied estimator, even if
   one of the nuisance parameters is misspecified;
2. is not only double robust w.r.t. consistency but also preserves asymptotic
   linearity under misspecification of one or more of the nuisance parameters,
   thereby provideing double robust inference;
3. and may be used to construct _higher-order TMLEs_ that reduce the second
   order remainder by incorporating the so-called higher-order efficient
   influence curve.

What's more, there are many additional nuances to the TMLE framework that again
make TMLE unique relative to OSE or EEE, including (to name but a few)
collaborative TMLE (C-TMLE) for the targeted estimation of orthogonal nuisance
functions (e.g., treatment and censoring mechanisms) in the least favorable
submodel and targeted fluctuations based on universal least favorable submodels
that target multidimensional or even infinite-dimensional target parameters
(e.g., treatment-specific survival curves).

It should also be noted that from the very beginning, any of our applications of
both EEE and TMLE have always accommodated machine learning; see, e.g., many of
the articles, circa 1990 and onward, on EEE cited in the book by vdL and Robins
(2001) as well as those on TMLE in the books by vdL and Rose ([_Targeted
Learning_ (2011)](https://link.springer.com/book/10.1007/978-1-4419-9782-1)
and [_Targeted Learning in Data Science_
(2018)](https://link.springer.com/book/10.1007/978-3-319-65304-4)). By allowing
for the use of machine learning, these approaches directly allow for double
robust machine learning-based estimators, when they are applied to double robust
estimation problems.

Finally, we can discuss how the recent DML methodology fits into this rich
previous literature on OSE, EEE, and TMLE. DML falls in the category of the
estimating equation-based estimators, which construct estimators as solutions of
a gradient-based estimating equation, resulting in the EIC estimating equation
if one selects the canonical gradient of the pathwise derivative. DML represents
a subclass of the EEE approach but uses a different terminology than that of
Robins and Rotnitzky (and, thus, different too from that of vdL and Robins,
2001): it enforces sample-splitting and establishes various interesting and
important theoretical results for a variety of estimation problems, going beyond
practical implementations. A first example of the different terminology is the
name _cross-fitting_, which had been called sample-splitting before (or simply
cross-validation in CV-TMLE). Despite the change in name, cross-fitting
precisely represents the sample-splitting used early on in OSE and in CV-TMLE.
Another example of different terminology is the usage of the _Neyman
orthogonality_ of estimating functions; in Robins and Rotnitzky's work, one
defines the class of estimating functions in terms of the orthogonal complement
of the nuisance tangent space, thereby guaranteeing that the estimating
functions at their true parameter values are orthogonal to any nuisance score
(e.g., see  chapter 1 of the book by vdL and Robins). Equivalently, the class of
estimating functions is simply provided by the class of gradients of the target
parameter, where any gradient is automatically orthogonal to the nuisance
tangent space. Thus, in the DML line of work, the term "orthogonal to the
nuisance tangent space" (from the work of Robins) is replaced by Neyman
orthogonality. This orthogonality of the estimating function to the nuisance
tangent space guarantees that the estimation of the nuisance parameter only
results in second order contributions to the EEE, often resulting in so-called
robustness (as in unbiasedness) of the estimating function under
misspecification of the nuisance parameter and, thus, also of the corresponding
EEE (again, see chapter 1 of the book by vdL and Robins for formal results).
Lastly, it is my understanding that, contrary to determining the class of
gradients and specifically the canonical gradient of the target parameter (as is
done in EEE), in DML, one often starts out with a particular estimating function
that has a nuisance parameter and then subtract off its projection on the
nuisance tangent space. This is precisely how Robins and Rotnitzky obtained the
class of augmented inverse probability of censoring weighted (AIPCW) estimating
functions, i.e., by subtracting off from an IPCW estimating function its
projection on the tangent space of the censoring mechanism, often only requiring
the assumption of coarsening at random to obtain maximal efficiency.
Nonetheless, orthogonalizing a given estimating function w.r.t. a nuisance
parameter maps the estimating function into a particular element of the
orthogonal complement in the nuisance tangent space for the statistical model
under consideration (i.e., one of the gradients of the pathwise derivative of
the target parameter, up to a normalizing matrix/constant which does not matter
for an estimating equation). Depending on the starting estimating function, the
orthogonalized version ends up being a gradient or canonical gradient, up to a
normalizing constant/matrix. Therefore, the DML approach is precisely a subset
of the general EEE approach originally developed by Robins and Rotnitzky -- in
particular, in coarsening at random censored data models, the general AIPCW
representation theorem of Robins and Rotnitzky for the class of all estimating
functions (i.e., those in the orthogonal complement of the nuisance tangent
space) also characterizes the initial IPCW estimating function whose
orthognalized variant is the canonical gradient and, thus, the EIC (see chapter
1, section 1.5, in the book by vdL and Robins).

There is much more that could be elaborated upon here, but we have covered some
of the points most relevant to distinguishing between the TMLE, EEE, OSE, and
the DML frameworks. A former PhD student of mine, [Iván
Díaz](https://www.idiaz.xyz/) (now faculty at Weill Cornell Biostatistics), has
recently written [a review
article](https://academic.oup.com/biostatistics/advance-article/doi/10.1093/biostatistics/kxz042/5631845)
discussing the two approaches of TMLE and DML in some detail as well.

Best Wishes,

Mark and [Nima](https://nimahejazi.org)

__P.S.__, remember to write in to this blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu` or @-mention
[`mark_vdlaan`](https://twitter.com/mark_vdlaan) on Twitter. Interesting
questions will be answered on this blog!