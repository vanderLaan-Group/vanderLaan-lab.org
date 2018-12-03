+++
title = "Adaptive sequential designs and optimal treatments"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2018-11-29T12:43:00+00:00"
categories = [
    "Resources",
    "Statistics",
    "Targeted Learning",
    "Q&A"
]
+++

_This post is part of our Q&A series._

A question from graduate students in our Fall 2018 offering of "Special Topics
in Biostatistics -- Adaptive Designs" at Berkeley:

<u>**Question:**</u>

> Hi Mark,
>
> Our question concerns the benefit of using a sequential adaptive design
> when estimating the outcome under the optimal dynamic treatment rule (for a
> binary treatment). We propose doing so in a 2-stage framework, where in the
> first stage subjects are naively randomized to treatment, $Pr(A=1) = 0.5$. In
> the second stage subjects are randomized conditional on their covariates:
> $logit[Pr(A = 1 \mid W)] = \widehat{BLIP}(W)$ such that they have a higher
> probability of receiving treatment if the conditional treatment effect
> estimate ($\widehat{BLIP}$) from the first stage suggests they will benefit
> from treatment. Extensions could account for the standard error of the
> estimated $\widehat{BLIP}(W)$.
>
> Is there any theory showing the benefits of using an adaptive sequential
> design for this target causal parameter? The expected outcome for subjects in
> the trial will certainly be higher in this design than in one where treatment
> is always randomized naively -- which offers a benefit to participants in a
> trial.
>
> However, from an estimation and efficiency perspective, it's not clear to us
> that this design will always offer improvement over a traditional design. To
> take an extreme example, if subjects in the second stage were assigned
> deterministically to the estimated optimal treatment (without
> experimentation), then estimation would suffer.  It appears that at some point
> the reduction in experimentation should affect estimation and inference,
> potentially through positivity issues.
>
> Do you have thoughts on this?
>
> -J.R. and L.M.

---


<u>**Answer:**</u>

Hi J.R. and L.M.,

...

Best Wishes,
Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!






This is an excellent question. Interestingly I believe that these adaptive designs if well run do not only benefit the patients themselves by receiving beneficial treatment with higher probability if past data suggests evidence for this. In addition, I believe that it will also optimize information for estimation of the expectation of the counterfactual outcome under the current best estimate of the dynamic treatment (i.e. EY_{d*_n}). May be I should say, of the expectation of the counterfactual outcome under the current estimate of the stochastic intervention that approximates the optimal dynamic treatment as sample size increases (i.e. EY_{g*_n})


The reason is that if I am sampling my treatment from g*, then my design is optimal for purpose of estimation of EY_{g*}.

More generally, if my treatment mechanism is g* and g*/g_0 is a meaure of how well we will do for EY_{g*_0}, i.e. one does want to make the actual design as close as possible to the g*_0 intervention one wants to evaluate.

In this case, our g*=g*_n changes with n, presumably gradually. Nonetheless, under gradual change of g*_n as function of n, one would think that the data sampled i.e used treatment mechanism is much closer to drawing from g*_n than it is from drawing treatment independently with probability 0.5.

Here you see once again how important it is to make the adaptive design converge smoothly and wisely, that is, do not adapt based on noise, adapt slowly based on evidence. However even when making rough changes due to two stage trial design, i.e. first stage 0.5, and second stage based on perturbation of estimator of optimal rule, the treatment mechanism will still resemble more our estimate g*_n than using 0.5.

Since g*_n is approximating the estimated optimal rule d*_n, I would also argue that the adaptive design will get a more precise estimator of EY_{d*_n} as well than one would obtain with an 0.5 fixed design.

Formally, one compares the asymptotic variance 1/n sum_i P_0 D*_{g*_n}(Q_0,g_i)^2 , where D*_{g*_n}(Q_0,g_i) is efficient influence curve of EY_{g*_n}, of the TMLE of EY_{g*_n} under our adaptive design, with the asymptitic variance P_0 D*_{g*_n}(Q_0,g_b)^2 where g_b(1|W_i)=0.5 is the simple RCT fixed design. i.e. one compares asymptotic variances of the TMLE under the two designs and we know what they are.

So after having run the adaptive design, so that you have g*_i for all i, you an make a  formal comparison of these two asymptotic variances in your simulation (where you know Q_0), and that will demonstrate a relative efficiency of the two design for learning EY_{g*_n}. Similarly, for EY_{d*_n}.

Of course, in a simulation, you can actually repeat the adaptive design many times and obtain a sampling distribution of the TMLE around the data adaptive EY_{g*_n} for this adaptive design, and compare its spread with the sampling distribution of TMLE of EY_{g*_n} under the  0.5 fixed design .

You also learn from this that if you would respond to noise and thereby set g*_i equal to some noisy estimate, then the g*_i's are not representative of the final d*_n or g*_n, and it will not be good. Similarly, if one does not allow for experimentation, i.e. one samples A deterministically from current estimator d_i of optimal dynamic treatment, then things will be bad as well: in that case, we have huge positivity issues, that is,  D*_{d*_n}(Q_0,g_i) will now blow up since the factor d*_n/g_i in front of Y-bar{Q} in it, where g_i =d_i is deterministic, will blow up.

So clearly how one adapts g*_n is fundamental at both the level of eventually learn the optimal rule ,but also about how to generate much information for learning the mean outcome under the final estimate of the optimal rule (or its final stochastic approximation representing the adaptive design at last observation n).

The same story applies to the adaptive designs that adapt continuous in time as we covered in class.

