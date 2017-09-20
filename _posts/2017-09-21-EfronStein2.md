---
layout: post
title: A Family of Efron-Stein Like Lower bounds for Variance
published: true
project: false
---
This is a follow up to my previous post: [An Efron-Stein Like Lower bound for Variance](https://sudeepraja.github.io/EfronStein/), where I derived the following inequality.

For any function $$f:\mathbb{R}^n\to\mathbb{R}$$ and iid random variables $$x_1,..,x_n$$, 

$$\mathbb{Var}[f] \geq \frac{1}{n-1}  \sum_{i=1}^n\mathbb{Var}_{(-i)}[\mathbb{E}_{(i)}[f]]$$

Here $$\mathbb{E}_{(-i)}$$ denotes the expectation taken with respect to all variables except $$x_i$$. $$\mathbb{E}_{(i)}$$ denotes the expectation taken over only $$x_i$$. Let $$\mathbb{Var}_{(-i)}$$ and $$\mathbb{Var}_{(i)}$$ also be defined similarly.

After writing the the blog, I posted it on [r/probabilitytheory](https://www.reddit.com/r/probabilitytheory/). A fellow redditor helped me out and told me to mail [Prof. Sourav Chatterjee](https://statweb.stanford.edu/~souravc/) at Stanford. Although Prof. Chatterjee did say that he had not seen this inequality before, he pointed out a major flaw. The variances on the right side of the inequality will probably be just as hard to understand as the variance on the left, since we are just integrating out one variable in each term. He also pointed out that the inequality has a recursive structure and it can be unfolded further to obtain a family of inequalities.

This is the original inequality, lets call this EFI1 (Efron-Stein Inverse 1):

$$\mathbb{Var}[f] \geq \frac{1}{n-1}  \sum_{i=1}^n\mathbb{Var}_{(-i)}[\mathbb{E}_{(i)}[f]]$$

We can expand each of the $$\mathbb{Var}_{(-i)}[\mathbb{E}_{(i)}[f]]$$  on the right using EFI1 again. We get EFI2:

$$
\begin{align}
\mathbb{Var}[f] &\geq \frac{1}{n-1}  \sum_{i=1}^n\mathbb{Var}_{(-i)}[\mathbb{E}_{(i)}[f]]\\
&\geq \frac{1}{n-1}  \sum_{i \in [n]} \frac{1}{n-2} \sum_{j \in [n]-\{i\}} \mathbb{Var}_{(-i,-j)}[\mathbb{E}_{(i,j)}[f]]\\
&= \frac{1 \cdot 2}{(n-1)(n-2)} \sum_{(i,j) \in \binom{n}{2}} \mathbb{Var}_{(-i,-j)}[\mathbb{E}_{(i,j)}[f]]
\end{align}
$$

Here $$(i,j) \in {{n}\choose{2}}$$ denotes that $$(i,j)$$ belongs to the set of distinct monotonic pairs formed from $$n$$. $$(-j)$$ has the usual meaning of removing $$x_j$$ variable from the set $$x_1,..x_n$$.

We can expand each of the $$\mathbb{Var}_{(-i,-j)}[\mathbb{E}_{(i,j)}[f]]$$  on the right using EFI1 again. We get EFI3:

$$\mathbb{Var}[f] \geq \frac{1 \cdot 2 \cdot 3}{(n-1)(n-2)(n-3)} \sum_{(i,j,k) \in \binom{n}{3}} \mathbb{Var}_{(-i,-j,-k)}[\mathbb{E}_{(i,j,k)}[f]]$$

If we keep on expanding this, the final inequality we obtain is EFI(n-1):

$$\mathbb{Var}[f] \geq \frac{(n-1)!}{(n-1)!} \sum_{(i,j,k,..) \in {{n}\choose{n-1}}} \mathbb{Var}_{(-i,-j,-k,...)}[\mathbb{E}_{(i,j,k,..)}[f]]$$

We can re-write EFI(n-1) as:

$$\boxed{ \mathbb{Var}[f] \geq \sum_{i=1}^n \mathbb{Var}_{(i)}[\mathbb{E}_{(-i)}[f]] }$$

Compare this with Efron-Stein:

$$\mathbb{Var}[f] \leq \sum_{i=1}^n \mathbb{E}_{(-i)}[\mathbb{Var}_{(i)}[f]]$$

The EFI(n-1) can be obtained by just moving $$\mathbb{Var}_{(i)}$$ inside and $$\mathbb{E}_{(-i)}$$ outside and changing $$\leq$$ to $$\geq$$. Here, the variances over a single variable, which addresses the first concern raised by Prof. Chatterjee.

Although EFI(n-1) is a well known lower bound, this proof is new and easier to understand. During this process, we have also uncovered an entire family of lower bounds, all of which are tight. 

My thanks to reddit user arjunkc and Prof. Chatterjee for pointing me in the right direction.
