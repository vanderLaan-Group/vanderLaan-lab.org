+++
title = "Causal effects for single-group policies"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2018-11-28T14:14:00+00:00"
categories = [
    "Resources",
    "Statistics",
    "Targeted Learning",
    "Q&A"
]
+++

_This post is part of our Q&A series._

A question from a graduate student in our Spring 2018 offering of "Targeted
Learning in Biomedical Big Data" at Berkeley:

<u>**Question:**</u>

> Hi Mark,
>
> I was thinking that if you addressed the question that [we] discussed in your
> office hours last week, a lot of economists would be interested in reading it.
>
> Feel free to edit the wording of the question however suits you best, but I
> was thinking: How can you formulate a causal parameter in a setting in which
> you have a policy that affects one group but not another based on observable
> characteristics and control for time trends in your model (i.e., subtracting
> out changes in the non-affected group as in differences-in-differences
> analysis)?
>
> Thanks,
> A.M.

---


<u>**Answer:**</u>

Hi A.M.,

I think the story was something like this: We have two groups of hospitals,
academic and regular hospitals. We are interested in evaluating a certain new
intervention that kicks in at time $t_1$, but it is only applied to the academic
hospitals.

For each hosptital $i$ in the sample of $n$ hosptials, we observe
$(W_i(0), \text{Type}_i, A_i(0) = 0, Y_i(0)), (W_i(1), \text{Type}_i, A_i(1),
Y_i(1)), i = 1, \ldots, n$.

Here $W(t)$ are characteristics of the hospital, presumably mostly time
independent, but can also include time-dependent factors, $A(t)$ denotes the
intervention ($0$ or $1$) at time $t$, and $Y(t)$ is the outcome on the hospital
at time $t = 0, 1$.

...it might be the case that at time $t=1$ all academic hospitals receive the
intervention, but it could also be the case that only a proportion of them
received treatment at time $t = 1$.

The goal is to estimate the causal impact of the intervention for the academic
hospitals. So one likes to understand what would be the mean outcome for the
academic hospitals if we would have given them treatment at time $t = 0$, versus
giving them control at time $t = 0$, and similarly at time $t = 1$. So let
$Y_i(t, a)$ be the counterfactual outcome for hospital $i$ at time $t$ and under
treatment $a \in (0, 1)$. So we would like to know $\mathbb{E}(Y(t, 1)
- \mathbb{E}Y(t, 0) \mid \text{Type = academic})$ for one of both $t = 0, 1$.

The challenge is that at time $t$, the academic hospitals either all get control
or all treatment.

A naive parameter would be $\mathbb{E}(Y(t = 1, 1) - Y(t = 0, 0) \mid
\text{Type = academic})$. This would be trivially estimated by the sample mean
of outcomes at time $t = 1$ minus sample mean of outcomes at time $t = 0$.
However, if $W(t_0)$ and $W(t_1)$ are different, then it would make more sense
to control for these  covariates and use as estimand
$$\int_w \mathbb{E}(Y \mid t = 1, W(1) = w, \text{Type = academic}) -
\mathbb{E}(Y \mid t = 0, W(0) = w, \text{Type = academic}) d\bar{P}_{ac}(w),$$
where $\bar{P}_{ac}$ is the pooled distribution of the pooled sample of $W_i(t)$
across the academic hospitals $i$, and $t$. The latter  would be estimated with
the TMLE of an ATE, but where time $t$ plays role of treatment.

If $W(t)$ is indeed time dependent and such a good unit specific covariate that
the impact of an environmental factor on $Y(t)$ is only through $W(t)$, then we
are in the situation of my technical report on causal inference for
community-based interventions, so that this ATE estimand actually identifies the
causal effect we care about. However, if that is not a realistic assumption,
then we have to deal with the concern that from time $t=0$ to time $t=1$, the
environment might have changed for the academic hospitals, so that the change we
are observing might not only be due to the treatment of interest, but other
factors that changed at that time (and that we can not control for by
controlling for $W(t)$).

One might now make the assumption that  the impact of the change of environment
for the academic hospitals is similar to the impact of change of environment for
the non-academic hospitals. Then, we want to subtract from the above estimand,
$\mathbb{E}(Y(t = 1, 0) \mid \text{Type=non-academic}) -
\mathbb{E}(Y(t = 0, 0) \mid \text{Type=non-academic}). Assuming that $W(t)$ is
changing as well, we want to control for these changes, and thereby we  aim to
identify this with the estimand:
$$\int_w \mathbb{E}(Y \mid t = 1, W(1) = w, \text{Type=non-academic}) -
\mathbb{E}(Y \mid t = 0, W(0) = w, \text{Type=non-academic})
d\bar{P}_{\text{non-academic}}(w),$$
where $d\bar{P}_{\text{non-academic}}$ is the pooled covariate distribution
across time $t = 0, 1$ for the non-academic hospitals.

So, based on the above reasoning, we would then propose the following estimand
as an approximation of the causal impact of the intervention on the academic
hospitals, which controls for changes in unit specific covariates over time and
changes in environmental factors over time:
\begin{align}
\Psi =& \mathbb{E}(Y(t = 1, 1) \mid \text{Type = academic}) \\
&- \mathbb{E}(Y(t = 0, 0) \mid \text{Type = academic}) \\
&- \mathbb{E}(Y(t = 1, 0) \mid \text{Type = non-academic}) \\
&+ \mathbb{E}(Y(t = 0, 0) \mid \text{Type = non-academic}),
\end{align}
where we gave the estimands for each of these $4$ terms above.

One can estimate this as a difference of two TMLEs of the ATE
$\mathbb{E}(Y_1 - Y_0)$ for $(W, A, Y)$ data structure, but where for the first
TMLE we only use the academic hospitals and where $A=0 (t=0)$ and $A=1 (t=1)$,
while the second TMLE only uses the non-academic hospitals and again
$A=0 (t=0)$ and $A=1 (t=1)$.

I could have imagined another type of example, in which as above we observe a
sample of hospitals at two time points, but, in this case, at both times $t=0$
and $t=1$, there is a proportion of them that receive treatment (and we ignore
type of hospital as in above example). However, one feels uncomfortable assuming
that $A(t)$ is conditionally independent of $Y(t)$, given $W(t)$ at either time
point $t=0,1$.

Instead, one might define the outcome at time $t=1$, as $Y=Y(t=1)-Y(t=0)$, and
then define the estimand as the
$\mathbb{E}_W (\mathbb{E}(Y \mid A=1,W) - \mathbb{E}(Y \mid A=0,W))$ for
$\mathbb{E}Y_1 - \mathbb{E}Y_0$.

This actually equals the G-computation estimand for
$\mathbb{E}Y(t = 1, a = 1) - \mathbb{E}Y(t = 0, a = 1) -
\mathbb{E}Y(t = 1, a = 0) + \mathbb{E}Y(t = 0, a = 0)$ for a joint treatment
$t,a$, as if we are interested in the causal effect modification of time $t$ on
effect of treatment $a$.

The idea being that one feels uncomfortable with assuming that $A(t=1)$ is
independent of $Y(t=1, a)$ given $W(t=1)$, but one is more comfortable with
assuming $A(t=1)$ is independent of $Y(t=1,a) - Y(t=0,a)$, given $W(t=1)$. More
generally, one wants to focus on estimating the ATE at time $t=1$, but one want
to use the baseline $Y(t=0)$ as an important confounder (so the data on the
hospital at time $t=0$ is used as baseline confounder of $A(t=1)$).

This is probably the motivation for the so called difference of differences
approach in econometrics (except often on logistic  scale). However, as I state
above, I believe the more general way to think about this is to view the time
$t=0$ data on hospital as baseline confounders for the treatment at time $t=1$,
recognizing that especially $Y(t=0)$ is a very crucial confounder that might
make the $A(t=1)$ conditionally randomized. We should also check this paper by
Weber and Petersen (JCI) on this topic.

Best Wishes,
Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!

