---
layout: post
title: Linear Estimators
published: true
project: false
---

Consider the online linear optimization problem. At each round, the adversary chooses a loss function $$l_t \in \mathcal{F}$$ and the player picks $$x_t \in \mathcal{K}$$ from a convex set $$\mathcal{K}$$. Roughly speaking, there are two basic settings in this problem:

1. Full Information, where the the loss function $$l_t$$ is revealed.
2. Bandit, where only the cost $$l_t^\top x_t$$ is revealed.

Let $$A$$ be an online linear optimization algorithm in the full information setting. It accepts a sequence of loss vectors and produces the next point to play:

$$\mathcal{A}: \mathcal{F}^\star \to \mathcal{K} \text{ such that } A(l_1,l_2,\dots,l_{t}) = x_{t+1}$$

To obtain a Bandit Linear Optimization from $$A$$, we follow these steps:

1. A sampling scheme $$S$$ such that $$S(x)$$ defines a distribution on $$\mathcal{K}$$ which satisfies:

$$\mathbb{E}_{y \sim S(x)} [y]=x$$

2. An estimation scheme $$E$$ such that $$E(l^\top y, y,x)$$ is an unbiased estimator of $$l$$:

$$\mathbb{E}_{y \sim S(x)}[E(l^\top y, y,x)] = l$$

The Bandit algorithm can be constructed as below:

1. Play $$y_t \sim S(x_t)$$ and observe $$l_t^\top y_t$$
2. Construct linear estimator $$\tilde{l}_t = E(l_t^\top y_t, y_t,x_t)$$
3. Update $$x_{t+1} = A(\tilde{l}_1,\tilde{l}_2,\dots,\tilde{l}_{t})$$

## Importance sampling estimator:

The expected loss of playing $$y \sim S(x)$$ is $$\mathbb{E}[l^\top y] = l^\top x$$. So, $$l = \nabla l^\top x = \nabla \mathbb{E}[l^\top y]$$. Now we can use the importance sampling trick to obtain an unbiased estimator for $$l$$. Let $$p(y;x)$$ define the probability density $$S(x)$$. We have:

$$\begin{align*}\nabla l^\top x &= \mathbb{E}[l^\top y] = \int_\mathcal{K} (l^\top y) p(y;x) dy\\
\implies l &=  \int_\mathcal{K} (l^\top y) \nabla p(y;x) dy\\
\implies l &= \int_\mathcal{K} (l^\top y) \frac{\nabla p(y;x)}{p(y;x)} p(y;x) dy\\
l &= \int_\mathcal{K} (l^\top y) \nabla \log p(y;x) p(y;x) dy\\
l&= \mathbb{E}\left[(l^\top y) \nabla \log p(y;x) \right]
\end{align*}$$

So, we can use this as an unbiased linear estimator when we play $$y \sim S(x)$$:

$$\tilde{l} = (l^\top y) \nabla \log p(y;x)$$


## Flaxman Kalai McMahan Estimator

Let $$\mathbb{B} = \{x:\|x\|_2\leq1\}$$ and $$\mathbb{S} = \{x:\|x\|_2 = 1\}$$, otherwise known as the ball and the sphere. Let $$u \sim \text{Uniform}(\mathbb{S})$$ and $$v \sim \text{Uniform}(\mathbb{B})$$. We have:

$$l^\top x = \mathbf{E}_{v \in \mathbb{B}}[l^\top (x+\delta v)]$$

Using Stoke's Theorem, we have:

$$l = \nabla \mathbb{E}_{v \in \mathbb{B}}[l^\top (x+\delta v)] = \frac{n}{\delta}\mathbb{E}_{u \in \mathbb{S}}[l^\top(x+\delta u) u]$$


Pick vector $$u$$ uniformly from $$\mathbb{S}$$ and play $$y = x + \delta u$$. Using the above result, we have:

$$\tilde{l} =  \frac{n}{\delta}(l^\top y) u $$

Going one step further, we can use a positive definite matrix $$A$$ insted of $\delta$ as follows:

$$l^\top x = \mathbf{E}_{v \in \mathbb{B}}[l^\top (x+Av)]$$

Stoke's Theorem gives:

$$l = \nabla \mathbb{E}_{v \in \mathbb{B}}[l^\top (x+A v)] = n\mathbb{E}_{u \in \mathbb{S}}[l^\top(x+A u) A^{-1} u]$$

Pick vector $$u$$ uniformly from $$\mathbb{S}$$ and play $$y = x + A u$$. Using the above result, we have:

$$\tilde{l} =  n(l^\top y) A^{-1}u $$

## Dani Hayes Kakade One Point Linear Estimator

Let $$\Sigma = \mathbb{E}[y y^\top]$$. We need to choose $$S(x)$$ such that $$\Sigma$$ is full rank for all $$x \in \mathcal{K}$$. This ensures $$\Sigma^{-1}$$ exists. We have

$$l = \mathbb{E}[\Sigma^{-1} y (y^\top l)]$$

Using this, we have the estimator:

$$\tilde{l} = \Sigma^{-1} y (y^\top l)$$
