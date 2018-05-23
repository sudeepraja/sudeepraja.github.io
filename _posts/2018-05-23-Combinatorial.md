---
layout: post
title: Online Combinatorial Optimization using Hedge in Linear Time
published: true
project: false
---

In this post, we consider the Online Linear Optimization problem where the decision set is confined to the vertices of a Hypercube. Naively applying Hedge to this problem achieves optimal regret in $$T$$ but requires maintaining an exponential number of variables. We show that it is in fact possible to apply Hedge by using only linear number of variables.

This problem can be viewed as a repeated game between a player and an adversary. Let $$\mathcal{S}$$ be a set of $$n$$ items. At each step $$t=1,2,..,T$$, the player chooses a subset  of items $$S_t \subseteq S$$ and the adversary simultaneously chooses a loss (cost) for each item $$l_t^{i} \in [0,1]$$ for $$i \in \mathcal{S}$$. The player incurs the loss (pays the cost) of the items he has chosen, $$L_t(S_t) = \sum_{i \in S_t} l_t^{i}$$. The player also gets to know the loss of each item. The goal of the player is to choose sets $$S_1,S_2,..,S_T$$ such that regret  $$R_T$$ is minimized. Regret is defined as:

$$R_T = \sum_{t=1}^T L_t(S_t) - \min_{S^\star \subseteq \mathcal{S}} \sum_{t=1}^T L_t(S^\star)$$

We can treat each of the subsets of $$\mathcal{S}$$ as experts and apply [Hedge](https://sudeepraja.github.io/Hedge/). 

 1. Initialize $$w^{S}_{1}=1 \forall S \subseteq \mathcal{S}$$
 2. For $$t=1$$ to $$T$$:
	 3. Let $$Z_t = \sum_{S \subseteq \mathcal{S}} w^{S}_{t}$$and $$p^{S}_{t} = w^{S}_{t}/Z_T$$. Choose $$S_t=S$$ with probability $$p^{S}_{t}$$.
	 4. See the losses $$l^{i}_t \forall i \in \mathcal{S}$$ and incur loss $$L_t(S_t)$$
	 5. Update $$w^{S}_{t+1} = w^{(t)}_{S} \exp(-\eta L_t(S))$$ or equivalently $$w^{S}_{t+1} = \exp(-\eta \sum_{\tau = 1}^tL_\tau(S))$$

Running Hedge this way requires sampling from a distribution defined by $$2^n$$ parameters and later updating these parameters which is intractable. The expected regret of this algorithm is:

$$\mathbb{E}[R_T] \leq \frac{\eta}{2} \sum_{t=1}^T\sum_{S \subseteq \mathcal{S}}  p^{S}_{t} L_t(S)^2 + \frac{\log 2^n}{\eta}$$

Since $$l_t^{i} \in [0,1]$$, we have $$L_t(S)^2 \leq n^2$$. So, 

$$\mathbb{E}[R_T] \leq \frac{\eta n^2T}{2} +\frac{n \log 2}{\eta}$$

Optimizing over $$\eta$$, we get $$\eta  = \sqrt{\frac{2 \log 2}{nT}}$$ and the expected regret $$\mathbb{E}[R_T] \leq n^{3/2} \sqrt{2T\log 2}$$. 

We give an algorithm which does hedge, but uses only $$n$$ parameters. The decision set of this problem can be represented using the vertices of the hypercube $$\{0,1\}^n$$. The vertex corresponding to the set $$S$$ is its characteristic vector $$X(S)$$, which is defined as $$X(S)^{i}=1\{i \in S\}$$. We rephrase the problem according to this notation. At each step $$t=1,2,..,T$$, the player chooses a vertex $$X_t \in \{0,1\}^n$$ and the adversary simultaneously chooses a loss vector $$l_t \in [0,1]^n$$. The player incurs the loss $$L_t(X_t) = \langle l_t, X_t\rangle$$. The player also sees the loss vector $$l_t = [l_t^{1},l_t^{2},...,l_t^{n}]$$. The goal of the player is to choose $$X_1,X_2,..,X_T$$ such that regret  $$R_T$$ is minimized. Regret is defined as:

$$R_T = \sum_{t=1}^T L_t(X_t) - \min_{X^\star \in \{0,1\}^n} \sum_{t=1}^T L_t(X^\star)$$

The algorithm we propose is as follows:

 1. Initialize $$x^{i}_{1}=1/2 \quad \forall i \in [n]$$
 2. For $$t=1$$ to $$T$$:
	 3. Sample $$X_t^{i} \sim \text{Bernoulli}(x_t^i) \forall i \in [n]$$ and choose $$X_t = [X_t^{1},X_t^{2},...,X_t^{n}]$$
	 4. See the loss vector $$l_t$$ and incur loss $$\langle l_t,X_t \rangle$$
	 5. Update 

$$x^{i}_{t+1} = \frac{1}{1+\exp(\eta \sum_{\tau=1}^t l_\tau^i)} \quad \text{or} \quad x^{i}_{t+1} = \frac{x^{i}_{t}}{x^{i}_{t} + (1-x^{i}_{t})\exp(\eta l_t^i)}$$

**Claim:** If both hedge and our algorithm see the same losses and perform the same action from time $$\tau=1,2,..,t-1$$ (defined by the filtration $$\mathcal{F_{t-1}}$$), then the probability that hedge chooses $$S$$ at time $$t$$ is the same as the probability that our algorithm chooses $$X_S$$ at time $$t$$. 

$$\Pr(S_t = S \mid \mathcal{F_{t-1}}) = \Pr(X_t = X_S \mid \mathcal{F_{t-1}})$$

**Proof:** We have that:

$$\Pr(S_t = S|\mathcal{F_{t-1}}) = \frac{\exp(-\eta \sum_{\tau = 1}^{t-1}L_\tau(S))}{Z_t} = \frac{\exp(-\eta \sum_{\tau = 1}^{t-1}\langle l_\tau,X_S \rangle)}{Z_t}$$

Where $$Z_t = \sum_{S \in \mathcal{S}} \exp(-\eta \sum_{\tau = 1}^{t-1}\langle l_t,X_S \rangle)$$. Also,

$$\Pr(X_t = X_S|\mathcal{F_{t-1}}) = \prod_{i=1}^n (x_t^i)^{X_S^i} (1-x_t^i)^{1-X_S^i}$$

We have to find an update for $$x_{t}$$ such that $$\Pr(S_t = S \mid \mathcal{F_{t-1}}) = \Pr(X_t = X_S \mid \mathcal{F_{t-1}})$$:

$$Z_t = \frac{\exp(-\eta \sum_{\tau = 1}^{t-1}\langle l_\tau,X_S \rangle)}{\prod_{i=1}^n (x_t^i)^{X_S^i} (1-x_t^i)^{1-X_S^i}}$$

Since this is true for all $$S \subseteq \mathcal{S}$$, consider any $$A, B \subseteq \mathcal{S}$$ such that $$A$$ and $$B$$ differ in a single element $$k$$. Without loss of generality, assume $$A = B \cup \{k\}$$ and $$k \notin B$$, ie, $$X_A = X_B + X_{\{k\}}$$ and $$X_B^k=0$$. We will have:

$$\begin{align}
\frac{\exp(-\eta \sum_{\tau = 1}^{t-1}\langle l_t,X_A \rangle)}{\prod_{i=1}^n (x_t^i)^{X_A^i} (1-x_t^i)^{1-X_A^i}} &= \frac{\exp(-\eta \sum_{\tau = 1}^{t-1}\langle l_t,X_B \rangle)}{\prod_{i=1}^n (x_t^i)^{X_B^i} (1-x_t^i)^{1-X_B^i}}\\
\frac{\exp(-\eta \sum_{\tau = 1}^{t-1}l_\tau^k)}{x_t^k} &= \frac{1}{1-x_t^k}\\
\frac{1-x_t^k}{x_t^k} &= \frac{1}{x_t^k} -1 = \exp(\eta \sum_{\tau = 1}^{t-1}l_\tau^k)\\
x^{k}_{t+1} &= \frac{1}{1+\exp(\eta \sum_{\tau=1}^{t-1} l_\tau^k)}
\end{align}$$

Since this is the update we chose in our algorithm, it ensures that that the probabilities are same.

Alternativley, we can show by substitution that the probabilities are equal:

$$\begin{align}
\Pr(X_t=X_S|\mathcal{F}_{t-1}) &= \prod_{i=1}^n (x_t^i)^{X_S^i} (1-x_t^i)^{1-X_S^i}\\
&=\prod_{i=1}^n \bigg(\frac{1}{1+\exp(\eta \sum_{\tau=1}^t l_\tau^i)}\bigg)^{X_S^i} \bigg(\frac{\exp(\eta \sum_{\tau=1}^t l_\tau^i)}{1+\exp(\eta \sum_{\tau=1}^t l_\tau^i)}\bigg)^{1-X_S^i}\\
&=\prod_{i=1}^n \bigg(\frac{\exp(-\eta \sum_{\tau=1}^t l_\tau^i)}{1+\exp(-\eta \sum_{\tau=1}^t l_\tau^i)}\bigg)^{X_S^i} \bigg(\frac{1}{1+\exp(-\eta \sum_{\tau=1}^t l_\tau^i)}\bigg)^{1-X_S^i}\\
&=\prod_{i=1}^n \frac{(\exp(-\eta \sum_{\tau=1}^t l_\tau^i))^{X_S^i}}{1+\exp(-\eta \sum_{\tau=1}^t l_\tau^i)}
\end{align}$$

Because $X_S^i$ is binary, we have $$(\exp(-\eta \sum_{\tau=1}^t l_\tau^i))^{X_S^i} = \exp(-\eta \sum_{\tau=1}^t l_\tau^i X_S^i)$$

$$\begin{align}
\Pr(X_t=X_S|\mathcal{F}_{t-1}) &= \prod_{i=1}^n \frac{\exp(-\eta \sum_{\tau=1}^t l_\tau^i X_S^i)}{1+\exp(-\eta \sum_{\tau=1}^t l_\tau^i)}\\
&=\frac{\exp(-\eta \sum_{\tau=1}^t \langle l_\tau, X_S\rangle)}{\prod_{i=1}^n(1+\exp(-\eta \sum_{\tau=1}^t l_\tau^i))}
\end{align}$$

Observe the denominator. It is a product of $$n$$ terms, each consisting of 2 terms. When we expand this, we get an expression with $$2^n$$ terms, each corresponding to one of the vertices of the hypercube. In the $$i$$th term of the product, we need to choose either 1 or $$\exp(-\eta \sum_{\tau=1}^t l_\tau^i)$$. If we choose, then $$X^i = 0$$ and if we choose $$\exp(-\eta \sum_{\tau=1}^t l_\tau^i)$$, then $$X^i = 1$$. Hence, we have that:

$$\prod_{i=1}^n(1+\exp(-\eta \sum_{\tau=1}^t l_\tau^i)) = \sum_{X \in \{0,1\}^n} \exp(-\eta \sum_{\tau=1}^t \langle l_\tau,X\rangle)$$

Hence, we get:

$$\Pr(X_t=X_S|\mathcal{F}_{t-1}) = \frac{\exp(-\eta \sum_{\tau=1}^t \langle l_\tau, X_S\rangle)}{\sum_{X \in \{0,1\}^n} \exp(-\eta \sum_{\tau=1}^t \langle l_\tau,X\rangle)} = \Pr(S_t=S|\mathcal{F}_{t-1})$$

As our algorithm has the same probability as choosing a subset as Hedge, it suffers from the same expected regret $$\mathbb{E}[R_T] \leq n^{3/2} \sqrt{2T\log 2}$$. However, it only need to maintain and update $$n$$ parameters which makes it tractable.

**Note:** The optimal regret is $$O(\sqrt{nT})$$, which can be obtained by running Online Mirror Descent(OMD) using the Entropic regularizer. In general, OMD using Entropic regularization on the convex hull of points and discrete Hedge on vertices are different algorithms. Maybe I'll write more on these in future blogs.