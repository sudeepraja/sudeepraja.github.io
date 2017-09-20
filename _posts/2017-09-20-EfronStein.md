---
layout: post
title: An Efron-Stein Like Lower bound for Variance
published: true
project: false
---
In a homework of the [Machine Learning Theory](https://people.cs.umass.edu/~akshay/courses/cs690m/index.html) course that I am taking at UMass, we were asked to prove the Efron-Stein Inequality. It upper bounds the variance of arbitrary functions of iid random variables. While working on the proof, I found an intriguing inequality which has the structure of Efron-Stein, but is a lower bound.

For any function $$f:\mathbb{R}^n\to\mathbb{R}$$ and iid random variables $$x_1,..,x_n$$

$$\mathbb{Var}[f(x_1,..,x_n)] \leq \mathbb{E} \sum_{i=1}^n \mathbb{Var}[f(x_1,..,x_n) \mid x_1,..,x_{i-1},x_{i+1},..,x_n]$$

First, lets try to make the notation more informative and less cumbersome. Let $$\mathbb{E}_{(i,j)}$$ denote the expectation taken with respect to $$x_i,..,x_j$$, while conditioning on the remaining variables. $$\mathbb{E}_{(i)}$$ denotes the expectation taken over only $$x_i$$. Let $$\mathbb{Var}_{(i,j)}$$ also be defined similarly.

$$\mathbb{E}_{(i,j)}[f] = \mathbb{E}_{x_i,..x_j}[f \mid x_1,..x_{i-1},x_{j+1},..x_n]$$

$$\mathbb{Var}_{(i,j)}[f] = \mathbb{Var}_{x_i,..x_j}[f \mid x_1,..x_{i-1},x_{j+1},..x_n]$$

Rephrasing Efron-Stein with slight modification using this notation, we get

$$\mathbb{Var}_{(i,n)}[f] \leq \sum_{i=1}^n \mathbb{E}_{(1,i-1),(i+1,n)}[\mathbb{Var}_{(i)}[f]]$$

[Law of Total Variance](https://en.wikipedia.org/wiki/Law_of_total_variance): $$\mathbb{Var}[Y] = \mathbb{E}[\mathbb{Var}[Y|X]] + \mathbb{Var}[\mathbb{E}[Y|X]]$$

We can use the Law of Total Variance to express $$\mathbb{Var}_{(i,n)}[f]$$ in $$n$$ was.

$$\mathbb{Var}_{(i,n)}[f] = \mathbb{Var}_{(1,i-1),(i+1,n)}[\mathbb{E}_{(i)}[f]] + \mathbb{E}_{(1,i-1),(i+1,n)}[\mathbb{Var}_{(i)}[f]] \quad \text{ for all } i \in [n]$$

Taking the sum over all $$n$$ ways of expressing $$\mathbb{Var}_{(i,n)}[f]$$, we get


$$\begin{align}
n\mathbb{Var}_{(i,n)}[f] &= \sum_{i=1}^n\mathbb{Var}_{(1,i-1),(i+1,n)}[\mathbb{E}_{(i)}[f]] + \sum_{i=1}^n\mathbb{E}_{(1,i-1),(i+1,n)}[\mathbb{Var}_{(i)}[f]]\\
&\geq \sum_{i=1}^n\mathbb{Var}_{(1,i-1),(i+1,n)}[\mathbb{E}_{(i)}[f]] + \mathbb{Var}_{(i,n)}[f]
\end{align}$$

The inequality is obtained by applying Efron-Stein to the second summation. This give us the final result.

$$\boxed{\mathbb{Var}_{(i,n)}[f] \geq \frac{1}{n-1}  \sum_{i=1}^n\mathbb{Var}_{(1,i-1),(i+1,n)}[\mathbb{E}_{(i)}[f]]}$$

This inequality is quite similar to Efron-Stein. Just interchange $$\mathbb{Var}$$ with $$\mathbb{E}$$ on the left, change $$\leq$$ to $$\geq$$ and scale by $$1/(n-1)$$. 

This inequality is also tight. Consider $$f = \sum_{i=1}^n x_i$$ where $$x_i$$ are drawn iid from a distribution with mean $$\mu$$ and variance $$\sigma^2$$.  

On the LHS, $$\mathbb{Var}_{(i,n)}[f]=n\sigma^2$$

On the RHS,

$$\begin{align}
\mathbb{Var}_{(1,i-1),(i+1,n)}[\mathbb{E}_{(i)}[f]]&= \mathbb{Var}_{(1,i-1),(i+1,n)}[x_1+..x_{i-1}+\mu+x_{i+1}+..+x_n]\\
&= (n-1)\sigma^2\\
 \frac{1}{n-1}  \sum_{i=1}^n\mathbb{Var}_{(1,i-1),(i+1,n)}[\mathbb{E}_{(i)}[f]]&= \frac{1}{n-1} n(n-1)\sigma^2 = n\sigma^2
\end{align}$$

So far, I haven't seen this inequality in any text or paper. If no else has seen it and it is new, then $$\text{YAY!}$$ Otherwise $${}_\text{yay!}$$. Hopefully some statistician or researcher can tell me if its a known result. Or better yet, uses it for their work.
