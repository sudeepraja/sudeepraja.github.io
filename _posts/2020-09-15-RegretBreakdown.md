---
layout: post
title: Regret Breakdown
published: true
project: false
---


In this post, I document a few tricks that can be used to modify standard regret inequalities.

Consider the $$n$$-experts problem. At each round, the adversary chooses a loss vector $$l_t \in \mathcal{L}$$ and the player picks $$p_t \in \Delta(n)$$ and chooses an expert according to the distribution $$p_t$$. The expected regret of the player with respect to a fixed distribution $$p^\star$$ is:

$$R_T(p^\star) = \sum_{t=1}^T l_t^\top(p_t-p^\star)$$

If the distribution $$p_t$$ is chosen according to FTRL or OMD using a Legendre regularizer $$F$$, one could write the regret inequality:

$$\sum_{t=1}^T l_t^\top(p_t-p^\star) \leq \frac{F(p^\star)-F(p_1)}{\eta} + \eta \sum_{t=1}^T {l_t^2} ^\top m_{F,\mathcal{L}}(p_t)$$

Here $$m$$ is a vector valued function which depends on $$F$$ and the range of $$l_{t}$$, ie, $$\mathcal{L}$$.

## Changing comparator

Instead of computing regret with respect to some $$p^\star \in \Delta(n)$$, we compute regret with respect to $$p_\delta^\star$$ where:

$$p_\delta^\star = \frac{\delta}{n} + (1-\delta) p^\star$$

The regret can be written as:

$$ \begin{align*}
\sum_{t=1}^T l_t^\top(p_t-p^\star) &= \sum_{t=1}^T l_t^\top(p_t-p_\delta^\star) + \sum_{t=1}^T l_t^\top(p_\delta^\star-p^\star)\\
&\leq \left[\frac{F(p^\star_\delta)-F(p_1)}{\eta} + \eta \sum_{t=1}^T {l_t^2} ^\top m_{F,\mathcal{L}}(p_t)\right] + \sum_{t=1}^T l_t^\top(p_\delta^\star-p^\star)
\end{align*}
$$

This trick is particularly useful when $$F(p) \to \infty$$ when $$p$$ goes to the boundary of $$\Delta_n$$.

## Using an estimator

If we have Bandit or some other form of partial feedback, then we need an estimator $$\tilde{l}_t$$ that can be used in the place of $$l_t$$. This estimator may have some bias. So, we have to modify regret to take care of the bias:

$$
\begin{align*}
\sum_{t=1}^Tl_t^\top(p_t-p^\star) &= \sum_{t=1}^T \mathbb{E}_{p_t}[\tilde{l}_t^\top(p_t-p^\star)]  + \sum_{t=1}^T \left(l_t-\mathbb{E}_{p_t}[\tilde{l}_t]\right)^\top(p_t-p^\star)\\
&\leq \left[ \frac{F(p^\star)-F(p_1)}{\eta} + \eta \sum_{t=1}^T \mathbb{E}_{p_t}[{\tilde{l}_t^2}] ^\top m_{F,\mathcal{\tilde{L}}}(p_t) \right] + \sum_{t=1}^T \left(l_t-\mathbb{E}_{p_t}[\tilde{l}_t]\right)^\top(p_t-p^\star)
\end{align*}
$$

The estimator $$\tilde{l}_t \in \mathcal{\tilde{L}}$$, where $$\mathcal{\tilde{L}}$$ may not be the same as $$\mathcal{L}$$. So, $$m_{F,\mathcal{L}}$$ is replaced by $$m_{F,\mathcal{\tilde{L}}}$$.


## Changing sampling distribution

Instead of sampling from $$p_t$$ as suggested by the FTRL/OMD, we sample from $$q_t$$. The regret is:

$$ \begin{align*}
\sum_{t=1}^T l_t^\top(q_t-p^\star) &= \sum_{t=1}^T l_t^\top(p_t-p^\star) + \sum_{t=1}^T l_t^\top(q_t-p_t)\\
&\leq \left[\frac{F(p^\star)-F(p_1)}{\eta} + \eta \sum_{t=1}^T {l_t^2} ^\top m_{F,\mathcal{L}}(p_t)\right] + \sum_{t=1}^T l_t^\top(q_t-p_t)\\
\end{align*}
$$

This becomes useful in the context of estimators. It may be beneficial to sample from $$q_t$$ insted of $$p_t$$ to get bounded estimators.

Combining change of sampling distribution with unbiased estimators, we get the regret inequality:

$$ \begin{align*}
\sum_{t=1}^T l_t^\top(q_t-p^\star) &= \sum_{t=1}^T l_t^\top(p_t-p^\star) + \sum_{t=1}^T l_t^\top(q_t-p_t)\\
&= \sum_{t=1}^T \mathbb{E}_{q_t}[\tilde{l}_t^\top(p_t-p^\star)] + \sum_{t=1}^T l_t^\top(q_t-p_t)\\
&\leq \left[\frac{F(p^\star)-F(p_1)}{\eta} + \eta \sum_{t=1}^T \mathbb{E}_{q_t}[{\tilde{l}_t^2}] ^\top m_{F,\mathcal{\tilde{L}}}(p_t)\right] + \sum_{t=1}^T l_t^\top(q_t-p_t)
\end{align*}
$$

## Putting it all together
Here is the sequence of steps, some of which might be optional:

- Apply change of comparator:

$$
\begin{align*}
\sum_{t=1}^T l_t^\top(q_t-p^\star) = \sum_{t=1}^T l_t^\top(q_t-p_\delta^\star) + \sum_{t=1}^T l_t^\top(p^\star_\delta-p^\star)\\
\end{align*}
$$

- Change sampling distribution to $$p_t$$:

$$
\begin{align*}
& = \sum_{t=1}^T l_t^\top(p_t-p_\delta^\star) + \sum_{t=1}^T l_t^\top(q_t-p_t)+\sum_{t=1}^T l_t^\top(p^\star_\delta-p^\star)\\
\end{align*}
$$

- Use Estimator and account for bias:

$$
\begin{align*}
& = \sum_{t=1}^T \mathbb{E}_{q_t}[\tilde{l}_t^\top(p_t-p_\delta^\star)] + \sum_{t=1}^T (l_t-\mathbb{E}_{q_t}[\tilde{l}_t])^\top(p_t-p_\delta^\star) + \sum_{t=1}^T l_t^\top(q_t-p_t)+\sum_{t=1}^T l_t^\top(p^\star_\delta-p^\star)
\end{align*}
$$

The final inequality can be written as:

$$
\begin{align*}
&\leq \underbrace{\frac{F(p^\star_\delta)-F(p_1)}{\eta} + \eta \sum_{t=1}^T \mathbb{E}_{q_t}[{\tilde{l}_t^2}] ^\top m_{F,\mathcal{\tilde{L}}}(p_t)}_{\text{OMD/FTRL Regret inequality}} + \underbrace{\sum_{t=1}^T (l_t-\mathbb{E}_{q_t}[\tilde{l}_t])^\top(p_t-p_\delta^\star)}_{\text{Effect of biased estimator}}\\
& \quad + \underbrace{\sum_{t=1}^T l_t^\top(q_t-p_t)}_{\text{Sampling distribution change} }+ \underbrace{\sum_{t=1}^T l_t^\top(p^\star_\delta-p^\star)}_{\text{Comparator change}}\\
\end{align*}
$$
