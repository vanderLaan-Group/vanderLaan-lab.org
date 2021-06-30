+++
title = "Applying targeted learning to improve global health equity"
author = "Mark van der Laan"
description = ""
tags = [
    "resources",
    "statistics",
    "targeted learning",
    "Q&A"
]
date = "2021-05-02T11:15:00+00:00"
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
> A pressing issue in the field of global public health is equitable ownership
> of data and results in terms of both authorship and representation. In some
> aspects, targeted learning improves equity by bolstering our ability to
> efficiently draw causal inferences from global health data. On the other
> hand, targeted learning and other advanced analytic methods can decrease
> equity by increasing data extraction from low- and middle-income countries by
> researchers from high-income countries and further obscuring the research
> process through complexity. What steps can and should we take to ensure that
> biostatistical methods are applied in a way to maximize equity? Throughout the
> semester, we have covered a few examples of applications of these methods
> using minimal specification adjustment (e.g., ["`sl3` Microwave Dinner
> implementation"](https://tlverse.org/tlverse-handbook/sl3.html#sl3-microwave-dinner-implementation)
> and ["Easy-Bake
> `tmle3`"](https://tlverse.org/tlverse-handbook/tmle3.html#easy-bake-example-tmle3-for-ate)),
> but would you discourage the applications of these methods without an
> understanding of the underlying statistical theory?
>
> Best,
>
> Z.B.

---

## Answer:

Hi Z.B.,

Thank you for the interesting question.

Yes, it is a fair point that data science has a huge impact on society and the
world as a whole and is already causing enormous equity issues, e.g., potential
censorship and/or manipulation of whole societies (as in [Orwell's
_1984_](https://en.wikipedia.org/wiki/Nineteen_Eighty-Four)), beyond other
issues. Targeted learning requires transparent formulation of the estimation
problem to be solved, and then lets the science and an _a priori_ defined
machine carry out the analysis. In this way, the conclusions are objective and
not driven by human or other biases. This could be used to solve equity problems
itself, such as fairness constraints in prediction or causal effects of actions
on equity-type outcomes. This is already happening, and certainly represents an
important class of problems that are receiving attention, ones in which TMLE
should play an important role. I would think that with increasingly open access
to data and the more we can utilize data from low- and middle-income countries
(transparently and out-in-the-open), the more one will require _a
priori_-specified reproducible methods, and the better these equity issues can
be addressed so that one can shed light on important developments.

In addition, it is important that people spend time and resources to study these
issues, define them objectively, and show what the data can tell us with such
transparent reproducible methods.

I certainly share your concern.

Best Wishes,

Mark

__P.S.__, remember to write in to our blog at `vanderlaan (DOT) blog [AT]
berkeley (DOT) edu`. Interesting questions will be answered on our blog!
