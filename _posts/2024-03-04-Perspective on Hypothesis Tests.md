---
layout: post
title: "Perspective on Hypothesis Tests"
toc: true
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
      displayMath: [['$$','$$'], ["\\[","\\]"]],
      inlineMath: [['$','$'], ["\\(","\\)"]],
      processEscapes: true
    },
    "HTML-CSS": { availableFonts: ["TeX"] }
  });
</script>

## Why use hypothesis tests?

Data scientists use hypothesis tests to validate assumptions made about a given set of data. The reason
they **need** hypothesis tests is because _data can decieve you_. Most people see a collection of data as
cold, hard facts --- "look at the numbers", we say with definitiveness. A data scientist understands the **random**
aspect of data, and knows that you can't always take data at its face value.

A hypothesis test helps distinguish between what's random and what's real.

## When should I use a hypothesis test?

Usually you'll find yourself in one of two scenarios:

First, if you ran an experiment, you'd use a hypothesis test to determine whether the effect of the treatment was due
to random chance or a true difference. In the business world, this is called an A/B test, which requires extra care in
setting up.

In most cases, your data didn't come from an experiment. This would be observational data, and you can
use a hypothesis test to better interpret the data. Most data in the business world is observational.
For example: if you have customer data and didn't run an experiment, then you only
observed the outcome of purchasing process. You only have information about _what_ ended up happening, and
can't say _why_ it happened. To make matters worse, the random aspect of data can often distort your view of "what ended up happening", and lead you to make conclusions that aren't really there. A hypothesis test can help you decide if an observation from your data is due to randomness or not.

## What is a hypothesis test?

I’d like to explain the key ideas behind hypothesis testing through an example I made up.

## Example: Hospitality

**Description**:

While reviewing their financial statements from the last quarter, a hotel chain discovered something new: long term bookings (LTB's) generated substantially more profit than short term bookings (STB's). Digging deeper, they discovered that the revenue difference could be explained by discrepancies in the cancellation rate. Specifically, the STB's had a cancellation rate that was 20 percentage points higher than the LTB's cancellation rate. The team speculates that people who book longer stays have plans that are more solidified, while short trips are more whimsical. For internal purposes, they identify stays longer than 14 days as “long term”.

The CEO wants to explore the link between length of stay and cancel rates. If shorter stays have significantly more cancellations, they would consider countermeasures to mitigate these losses. Implementing trip insurance and cancellation fees have been debated in the past. However, before they make any policy changes, they’d like to have confidence that the difference in cancellation rates is meaningful, and not just due to chance. In general, a strong and sound understanding of their business will help the executive team make smart strategic decisions.

**The Hypothesis Test**:

We want a model for how our data behaves. Like any scientist worth their salt, we need to formulate a **testable hypothesis**. In the language of hypothesis testing, we are interested in two classes of hypotheses: the null hypothesis and the alternative hypothesis.

You can think of the null hypothesis as the “cynical” perspective. In this scenario, the null hypothesis is that there is no true difference between cancellation rates of long and short term bookings, and the observed difference is due to pure random chance. This is the sense in which the null is "cynical" --- it dismisses the findings of our data as a mirage of randomness.

The alternative hypothesis, on the other hand, asserts that the observed difference is meaningful; in this case, that short term bookings cancel at a higher rate than long term bookings. In contrast with the null, the alternative hypothesis asserts that our findings really do exist.

Now that we have an intuition of what the two hypotheses are, we need to arrange them so that we can test them with our data.

The null says that there is no true difference between cancellation rates on average. Granted, the null accepts that there are _observed_ differences between the two --- that was what the financial performance review told us at the very start of this case study. But, according to the null model, this was just chance variation --- in the long run, the averages will end up being the same. With that in mind, we can formulate our null hypothesis as saying that "the average cancellation rate of STB's is the same as the average cancellation rate of LTB's". Using the variable $\bar{X}$ to denote the average, the null says

$$
\bar{X}_{\text{LTB}} = \bar{X}_{\text{STB}}
$$

On the other hand, the alternative hypothesis says that there is a true difference in the observed cancellation rates. That, on average, we would still expect to see a difference in the cancellation rates between long and short term stays. We can formulate the alternative hypothesis as saying that the average cancellation rate is higher for an STB than for an LTB.[^label1]

$$
\bar{X}_{\text{LTB}} < \bar{X}_{\text{STB}}
$$

[^label1]:
    The formulation of the alternative hypothesis --- "the average cancellation rate is **higher** for a short term stay" --- is purely a function of the problem setup. Oftentimes, we are testing for a _difference_ in two averages, without any prior knowledge of what direction that difference is in. If this was the case, the alternative would've looked like

    $$
    \bar{X}_{\text{LTB}} \neq \bar{X}_{\text{STB}}
    $$

    Recall that here, the starting point was noticeably less cancellations for long term stays, so we specifically want to test for the difference being a higher cancel rate for long term stays. This concept is known as one vs. two tailed tests. Read more [here](https://stats.oarc.ucla.edu/other/mult-pkg/faq/general/faq-what-are-the-differences-between-one-tailed-and-two-tailed-tests/).

**Comparing Hypotheses**:

Now that our hypotheses are defined, it's time to compare the two --- but first, a brief aside on why we're even allowed to do this:

When you have a large number of data points, as many modern enterprises do, statistical theory lets you make some nice assumptions about what that data means. One such assumption, known as the Law of Large Numbers (LLN), says that the average you calculate from your data gets closer to the true average if you use more data.[^label2]

[^label2]:
    Mathematically, we express the weak LLN as saying: for a sequence of independently and identically distributed random variables with finite mean and variance, $X_1, X_2,...$ the sample average, defined as $ \bar{X_n} = \frac{1}{n}(X_1 + ... + X_n) $, converges to the expected value: $\bar{X_n} \rightarrow^p \mu$ (which, since the variables were i.i.d., is the expected value of all the random variables in the sequence). The proof relies on Chebyshev's inequality, and is quick --- using the definition of sample variance, $Var(X_n) = \frac{\sigma^2_X}{n}$, Chebyshev's inequality says:
    $$ \begin{equation}P(|\bar{X} - \mu| \geq \epsilon) \leq \frac{Var(X_n)}{\epsilon^2} = \frac{\sigma^2_x}{n\epsilon^2} \end{equation} $$.
    Through the $n$ in the denominator, you can see that as the sample size grows infinitely large, the probability $P(|\bar{X} - \mu| \geq \epsilon)$ converges to zero and
    thus $\bar{X} \rightarrow^p \mu$

This is where the power of big data comes in.

Think about what the null model is saying: that the observed data is just due to chance, and does not reflect a statistically meaningful result. At first glance, this seems like a claim that’s impossible to refute – no matter how extreme the observed results are, the null model would always blow it off as coincidence!

The LLN is the cornerstone that makes hypothesis testing useful at all. Remember what the LLN says:

> as the sample size grows large, the average of our data sample gets closer to the true average.

This is the silver bullet against the "irrefutability" of the null model: the averages of the cancellation rates in our data sample are very closely approximating the true averages. The "true average" is a statistical concept[^label3] describing how the data was generated. In our example, the true average is the probability that determined whether a booking cancelled or not, and subsequently the data we collected was populated by the true average. It's quite abstract, and it really only exists in theory --- in practice, we'd never be able to measure the true average. The LLN tells us that in a large sample, the average we see in our data is closely approximating the true average, and a large difference in the observed averages suggests a meaningful difference in the true averages.[^label6]

[^label6]:
    This point is actually a bit more subtle, and relies on the Continuous Mapping Theorem, which states that continuous functions preserve limits even when the inputs are random variables. In general, if $X_n \rightarrow ^p \bar{X}$, then $g(X_n) \rightarrow g(\bar{X})$.Here, in our example, we're taking the difference of two random variable collections:

    $$
    g(\bar{X}_{LTB}, \bar{X}_{STB}) = \bar{X}_{LTB} - \bar{X}_{STB}
    $$

    By the law of large numbers, we know that the sample average $\bar{X}$ converge in probability to the true averages $\mu$. Thus, by the continuous mapping theorem, we can say that

    $$
    g(\bar{X}_{LTB}, \bar{X}_{STB}) \rightarrow^p g(\mu_{LTB}, \mu_{STB}) = \mu_{LTB} - \mu_{STB}
    $$

[^label3]: More formally known as the population parameter.

Let’s check-in: we have our null model, our alternative model, and an understanding of how our data relates to the two.

Our null model is that there is no difference between the cancel rates for long term stays and short term stays. Our alternative model is that there is a difference in cancel rates. From the description, the observed difference from our data was 20 percentage points.

We can convert the discrepancy between the null and alternative model into something known as a z-statistic: we divide the observed difference of 20 by the standard deviation, and that’s our z-statistic. At a high level, the z-statistic measures how far away our measured difference is from the difference that the null hypothesis expects. Again, the null model in this example is that there is 0 difference.

$$
\mathbb{z} = \frac{(\bar{X}_{alternative} - \bar{X}_{null})}{\sigma}
$$

In our example:

$$
z = \frac{\text{0.2}}{\text{standard deviation}}
$$

since the alternative average was 0.2, the null average was 0.0, and we would estimate the standard deviation from the data.[^label4]

[^label4]: Methods of variance estimation deserves a whole separate post.

From this definition, we can start to see what the hypothesis test is really doing. If the average we observe is very far from the average that the null expects, it suggests that **the null model is a bad model for the data**, and we should consider the alternative instead.

**The Results**:

Once we have the z-statistic, we are ready to test the hypotheses. Since we assume that we have a lot of data, we can use the normal approximation.[^label5] By providing the z-statistic computed from our data to the normal distribution, we get a probability in return. After applying some basic transformations, this probability is the result of our hypothesis test. Forget about the technical details for a moment, since any hypothesis test will ultimately lead you to this point, and assume that the probability you got as a result was 3.5%. What does this mean?

[^label5]:
    In practice, this is a non-trivial task that requires both statistical and domain knowledge. However, for many real-world
    processes, the Central Limit Theorem often leads to distributions that are normal.

By the construction of our hypothesis test, the result of 3.5% represents the probability that we observe data at least as extreme as what we got, assuming that the null hypothesis was true. In the context of the example:

The null hypothesis was that the cancellation rates for long and short term stays were the same.
Our data shows that long term stays have a cancellation rate that is 20 percentage points lower than the cancellation rate for short term stays
Our hypothesis test says that, if the null hypothesis is true, there is a 3.5% chance that we would observe that 20 percentage point difference, or any other difference larger than 20 points.

This is the result of hypothesis testing. You start by assuming that the null hypothesis is true. Since you’re going out of your way to run a test, you probably observed something that isn’t compatible with the null hypothesis – your observed difference. You use a hypothesis test to tell you how likely it is that you got that observed difference, assuming that the null hypothesis was true. Based on the likelihood of the observation, you'd decide whether you accept or reject the null. A higher likelihood of the data under the null would lead you to accept it, and a lower likelihood would lead you to reject it.

In most settings, 3.5% is such a small probability that you’d start to begin to doubt the null hypothesis. Because your null hypothesis is so unlikely given the data that you have, you reject the null hypothesis in favor of the alternative. In doing so, you conclude that the cancellation rate for long term bookings is lower than the cancellation rate for short term bookings, at a statistically significant level.

---

---

# **Technical Commentary**:
