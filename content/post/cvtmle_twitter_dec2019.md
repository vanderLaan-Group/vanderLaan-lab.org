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

<u>**Question:**</u>

> [@mark_vdlaan](https://twitter.com/mark_vdlaan) Is there an applied
> researcher's guide to choosing between double machine learning and TMLE +
> cross-fitting? PS: Thanks for making these methods and resources so easily
> accessible!

---

<u>**Answer:**</u>

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

With this background I now proceed with discussing the three classes of
efficient estimators OSE, EEE and TMLE and provide some historical context, and
how DML fits into this.

Historically, the first efficient estimator was the so called one-step estimator
using sample splitting (1970-1980, Levin, Pfanzagl, Klaassen, see references in
BKRW, etc). For example, Klaassen (1986) shows that the one-step estimator using
sample splitting  is efficient under minimal conditions. The one-step estimator
(and its non-sample splitting version) is the efficient estimator presented in
the comprehensive book Bickel, Klaassen, Ritov, Wellner (1997) on the topic of
efficient estimation in semiparametric models. This one-step estimator is
defined as a plug-in initial estimator of the target parameter plus the
empirical mean of the efficient influence curve (EIC) at this same initial
estimator. The sample splitting version is defined as the average across
a number of sample splits in training and validation sample (say V-fold), of the
initial plug-in estimator based on training sample plus the empirical mean over
the validation sample of the EIC at this same initial plug-in estimator (fitted
on training sample). A little later (1980-), as empirical process theory became
a field, the sample splitting based one-step estimator was often replaced by the
regular one-step estimator that relied on the Donsker class condition (i.e., the
sample splitting will come at a small price if the Donsker class condition
nicely holds, but one should make sure the initial estimator is not an overfit).

In the 1990's the corresponding estimating equation-based estimators (EEE)
approach was rigorously developed by Robins and collaborators, in which one
construct efficient estimators as solutions of   the efficient influence curve
(EIC) estimating equation: i.e., one sets empirical mean of EIC(psi,nuisance)
equal to zero, estimating the nuisance function in EIC with an initial
estimator, and solves for target parameter psi.  This relies on the efficient
influence curve having an estimating function representation: i.e, EIC(P)
depends on data distribution P through the target parameter Psi(P) and
a nuisance parameter. If such a representation of EIC(P)=EIC(psi,nuisance)
exists, the one-step estimator equals the first step of the Newton-Raphson
algorithm for solving the estimating equation. Fundamental work on EEE was done
by Robins and Rontitzky (1992-), and collaborators, in the context of censored
data and causal inference models, involving clever representations and
derivations of the efficient influence curve (e.g, Augmented IPCW representation
of the EIC). A comprehensive review and treatment of the general efficient
estimating equation method (not only applied to censored or causal inference
models), including its theory is presented in the book van der Laan, Robins
(2001).

Regarding comparing OSE with EEE, the OSE is always well defined, while the EEE
requires 1) the estimating function representation; 2) that a solution of the
EIC-equation exists, and 3) if multiple solutions exists that we know how to
select among them. On the other hand, the asymptotics of OSE relies on the
initial estimator, making it potentially less robust than EEE: for example, if
the estimation problem is DR, but the initial estimator is not DR, then the OSE
is generally not DR either, even though the EEE will be DR. Completely analogue
to the OSE one can use a sample splitting analogue of the EEE by defining the
estimating equation as an average across the sample splits of the empirical mean
over validation sample of the EIC in which nuisance parameters are estimated
based on training sample: in this manner, the first step of Newton-Raphson for
solving this sample splitting based estimating equation equals the one-step
estimator based on sample splitting.  In van der Laan, Robins (2001) the focus
was on the non-sample splitting based EEE, and our theorems throughout the book
(and the general theorems in chapter 2) thereby rely on a Donsker class
condition. Specifically, we show that if one uses machine learning based
estimators whose realizations are cadlag functions with finite sectional
variation norm, then this Donsker class condition holds, thereby still allowing
for highly flexible machine learning, including HAL-MLE.

An important point here is that, any gradient of the pathwise derivative of the
target parameter can be used to construct an asymptotically linear estimator
with influence curve that gradient, and by using the unique canonical gradient,
it will be asymptotically efficient. Indeed, in Robins and Rotnitzky's work,
they always refer to the class of all estimating functions orthogonal to the
nuisance tangent space, which is equivalent with the class of all gradients of
the pathwise derivative of the target parameter.  Indeed, Robins et al. show
that the class of gradients can be computed by determining the class of all
functions of the unit data that are orthogonal (in covariance operator) to all
nuisance scores (scores of paths for which the pathwise derivative of the target
parameter equals zero). In problems in which the efficient influence curve is
hard to compute, one might decide to go for an inefficient OSE or EEE implied by
an easier to compute gradient (see van der Laan, Robins, 2001). In an estimation
problem with the double robustness structure, any of such inefficient OSE or EEE
based on an inefficient gradient will still be double robust (i.e., the
robustness implied by second order remainder R_2(P,P0) applies to any gradient,
since what matters is that it is orthogonal to the nuisance scores, not that it
has minimal variance).

A problem both EEE and OSE (sample splitting based or not) suffer from is that
they are not plug-in estimators (i.e. one applies the target parameter mapping
to an estimated data distribution in the model), so that they might not satisfy
crucial global constraints in the statistical model. For example, if the
canonical gradient is highly variable or sample size is small, it can easily
happen that the OSE and EEE end up estimating a probability with a number larger
than 1 or smaller than zero. Moreover, by not respecting these important global
statistical constraints in the model, the OSE and EEE lack robustness (i.e., one
observation can throw it off badly),  contrary to an MLE which always fully
respects the global constraints of the model.  Of course, global constraints are
asymptotically irrelevant, but are generally very important in finite samples.

By contrast, TMLE produces an efficient _plug-in_ estimator, performing updates
in the model space so as to avoid any risk of violating bounds on the density of
data and on the parameter space. Specifically, the targeted maximum likelihood
estimator takes an initial estimator of the density of the data, constructs
a parametric model through this initial estimator with score spanning the
canonical gradient at the initial estimator, and fits the unknown parameter(s)
in this so called least favorable parametric model (LFM) with MLE. The resulting
targeted density estimator is then plugged in the target parameter mapping to
obtain the TMLE of the target parameter. The least favorable model through the
initial estimator can often be chosen to be  a standard parametric model using
as off-set the initial estimator: for example, a the targeting step in the TMLE
of the ATE can be carried out with a logistic regression using a clever
covariate, and as off-set the initial estimator. In more recent work (van der
Laan, Gruber, 2015), we propose a so called universal least favorable submodel
that guarantees that the targeting step is maximally robust and that the TMLE
exactly solves the EIC equation in one-step, while with a local LFM it will
solve the EIC equation  up till an approximation error that is asymptotically
negligible if the initial estimator achieves the n-1/4 rate of convergence
(iteration of the TMLE can be used to solve the EIC equation  to arbitrary
precision).

In general, one does not need to always estimate the whole data density, but
instead, one specifies the required functional of the data density the target
estimand relies upon; determines a loss function and least favorable model
through the initial estimator of this functional (i.e. generalized score spans
the canonical gradient); computes the MLE of the unknown parameters of the least
favorable parametric model; and plugs the targeted estimator of the functional
into the target parameter mapping. This plug-in estimator is now called targeted
minimum loss estimator since it does not require the loss to be the
log-likelihood loss, and allows one to focus only on the relevant functional of
the data distribution.

In many problems the target parameter depends on   a collection of functionals
(i.e., nuisance functions) of the data distribution. In that case, one can
separately determine a least favorable submodel for each of these functionals by
requiring its score (w.r.t., a specified loss) to span the corresponding
component of the efficient influence curve. In this manner, one can target each
nuisance function separately, or sequentially (e.g., in such a way that
a single step TMLE already exactly solves the EIC equation, see ltmle()). Such
TMLE solve the empirical mean of each component of the EIC, and thus solve, in
particular, the EIC equation.


As with any of the three efficient estimation methods, the TMLE can also be
carried out with sample splitting, which we termed CV-TMLE _cross-validated
TMLE (2010, Zheng, vdL). The CV-TMLE uses an initial estimator on the training
sample, carries out the TMLE step on the validation sample, and defines the
CV-TMLE as the average across the sample splits of the resulting plug-in TMLE.
In fact, the TMLE update step can also be pooled across the validation samples.
Just as, the sample splitting versions of OSE, EEE, the CV-TMLE is
asymptotically efficient without a need to assume the Donsker class condition,
thereby allowing for overfitted initial estimators of the nuisance functions (as
long as the second order remainder R2(hatP,P0) is asymptotically negligible).

Importantly, the TMLE, just as an MLE, is a plug-in estimator and thereby fully
respects and incorporates the global constraints of the statistical model and
target parameter mapping. A  nice example of how TMLE fully utilizes constraints
is the rare outcome TMLE (Balzer et al.) in which both the initial estimator of
the prediction function and the targeted version respect that the predictions
are known to be small, making it much better perform in finite samples. Contrary
to EEE, TMLE does not rely on the EIC to be an estimating function, and  is
always well defined: there is no concern for multiple solutions or no solution
since one just has to compute a simple MLE, and, even if that MLE allows
multiple solutions of its score equation, the empirical risk provides
a criterion to choose among them. We also note that TMLE is the only estimator
that actually generalizes the MLE: if the MLE is well defined and used as
initial estimator, than the TMLE equals the MLE (targeting step will select zero
fluctuation).

In addition, the TMLE can augment the least favorable parametric fluctuation
model with additional fluctuation parameters that generate additional estimating
equations the TMLE will solve (beyond the EIC equation): this is not possible
within the OSE or EEE framework, since they have to commit to one particular
gradient (e..g, see work van der Laan, 15, Benkeser, Carone, demonstrating this
nicely). These additional fluctuation parameters can then be chosen so that the
TMLE obtains additional statistical properties beyond asymptotic efficiency. In
this manner, we have proposed general TMLE that  1) is guaranteed more efficient
than a user supplied estimator (even if one of nuisance parameters is
misspecified); 2) is not only double robust w.r.t consistency, but also
preserves asymptotic linearity under misspecification  of one of nuisance
parameters, and thereby provides double robust inference; 3) higher order TMLE
_higher-order TMLEs_  that reduce the second order remainder (incorporating the
so called higher order efficient influence function). What's more, there are
many additional nuances to the TMLE framework that are again unique to TMLE
relative to OSE or EEE, including collaborative TMLE (C-TMLE) for the targeted
estimation of the orthogonal nuisance function (e.g, treatment and censoring
mechanism) in the least favorable submodel; targeted fluctuations based on
universal least favorable submodels that target multidimensional or even
infinite dimensional target parameters (e.g. treatment specific survival curve).

It should also be noted that from the very start, any of our applications of EEE
and TMLE have always used machine learning (see e.g. many of the articles 1990-
cited on EEE in van der Laan, Robins, 2001, and on TMLE in  van der Laan, Rose,
2011, 2018), and thereby resulted in double robust machine learning based
estimators when applied to double robust estimation problems.

Finally, let's discuss how the recent DML fits into this rich previous
literature on OSE, EEE, and TMLE. DML falls in the category of estimating
equation-based estimators (EEE), which constructs estimators as solutions of
a gradient estimating equation, resulting in the efficient influence curve
(EIC) estimating equation if one selects the canonical gradient of the pathwise
derivative.  DML represents a subclass of the EEE approach, but uses
a different terminology than Robins, Rotnitzky (and thus van der Laan, Robins,
2001), enforces  the sample splitting, and the authors of DML established
various interesting and important theoretical results for a variety of
estimation problems, beyond practical implementations. A first example of
different terminology is the name cross-fitting, which had been called sample
splitting before (or cross-validation in CV-TMLE), but precisely represents the
sample splitting used early on in OSE and in CV-TMLE. Another example of
different terminology is the term Neyman-orthogonality of estimating functions,
while in Robins and Rotnitzky's work one defines the class of estimating
functions in terms of the  orthogonal complement of the nuisance tangent space,
thereby guaranteeing that the estimating functions at their true parameter
values are orthogonal to any nuisance score (e.g., see  chapter 1, van der Laan,
Robins). Equivalently, the class of estimating functions is simply provided by
the class of gradients of the target parameter, where any gradient is
automatically orthogonal to the nuisance tangent space. Thus in the DML work the
term "orthogonal to the nuisance tangent space" in Robins work is replaced by
Neyman-orthogonality. This orthogonality of the estimating function to the
nuisance tangent space guarantees that the estimation of the nuisance parameter
only results in second order contribution to the EEE, and often results in so
called robustness of the unbiasedness of the estimating function under
misspecification of the nuisance parameter (and thus also of the corresponding
EEE, see Chapter 1 van der Laan, Robins, 2001, for formal results). Finally, it
is my understanding that, contrary to (as in EEE) determining the class of
gradients and specifically the canonical gradient of the target parameter, in
DML they often start out with a particular estimating function that has
a nuisance parameter, and then subtract its projection on the nuisance tangent
space. This is precisely how Robins and Rotnitzky obtained the class of
augmented inverse probability of censoring weighted (AIPCW) estimating
functions, subtracting from an IPCW estimating functions its projection on the
tangent space of the censoring mechanism (often only assuming coarsening at
random to make it maximally efficient), and they showed that it represents the
class of all estimating estimating functions/all gradients. Either way,
orthogonalizing a given estimating function w.r.t. nuisance parameter maps the
estimating function into a particular element of the orthogonal complement in
the nuisance tangent space for the statistical model under consideration (i.e,
one of the gradients of pathwise derivative of target parameter, up till
normalizing matrix/constant which does not matter for an estimating equation).
Depending on the starting estimating function, the orthogonalized version ends
up being a gradient or canonical gradient (up till a normalizing
constant/matrix).  Therefore, the DRML approach is precisely a subset of the
general EEE approach originally developed by Robins and Rotnitzky: in
particular, in CAR censored data models the general AIPCW representation theorem
of Robins and Rotnitzky for the class of all estimating functions (i.e.,
orthogonal complement of nuisance tangent space) also characterizes the initial
IPCW estimating function whose orthognalized version is  the canonical gradient
and thereby the efficient influence curve (see Chapter 1, Section 1.5 in van der
Laan, Robins, 2001).

There is much more that could be elaborated upon here, but we have covered some
of the points most relevant to distinguishing between the TMLE, EEE, OSE, and the DML
frameworks. A former PhD student of mine, [Iván Díaz](https://www.idiaz.xyz/)
(now faculty at Weill Cornell Biostatistics), has recently written [a review
article](https://academic.oup.com/biostatistics/advance-article/doi/10.1093/biostatistics/kxz042/5631845)
discussing the two approaches TMLE and DML in some detail.

Best Wishes,

Mark and [Nima](https://nimahejazi.org)

__P.S.__, remember to write in to this blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu` or @-mention
[`mark_vdlaan`](https://twitter.com/mark_vdlaan) on Twitter. Interesting
questions will be answered on this blog!
