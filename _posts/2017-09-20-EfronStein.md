---
layout: post
title: An Efron-Stein Like Lower bound for Variance
published: true
project: false
---
In a homework of the [Machine Learning Theory](https://people.cs.umass.edu/~akshay/courses/cs690m/index.html) course that I am taking at UMass, we were asked to prove the Efron-Stein Inequality. It upper bounds the variance of arbitrary functions of iid random variables. While working on the proof, I found an intriguing inequality which has the structure of Efron-Stein, but is a lower bound.

For any function $$f:\mathbb{R}^n\to\mathbb{R}$$ and iid random variables $$x_1,..,x_n$$

$$\mathbb{Var}[f(x_1,..,x_n)] \leq \mathbb{E} \sum_{i=1}^n \mathbb{Var}[f(x_1,..,x_n) \mid x_1,..,x_{i-1},x_{i+1},..,x_n]$$

Fleshing it out more explicitly:

$$\mathbb{Var}[f] \leq  \sum_{i=1}^n \mathbb{E}_{x_1,..,x_{i-1},x_{i+1},..,x_n}[\mathbb{Var}_{x_i}[f \mid x_1,..,x_{i-1},x_{i+1},..,x_n]]$$

First, lets try to make the notation more informative and less cumbersome. Let $$\mathbb{E}_{(-i)}$$ denote the expectation taken with respect to all variables except $$x_i$$. $$\mathbb{E}_{(i)}$$ denotes the expectation taken over only $$x_i$$. Let $$\mathbb{Var}_{(-i)}$$ and $$\mathbb{Var}_{(i)}$$ also be defined similarly.

Rephrasing Efron-Stein with slight modification using this notation, we get

$$\mathbb{Var}[f] \leq \sum_{i=1}^n \mathbb{E}_{(-i)}[\mathbb{Var}_{(i)}[f]]$$

[Law of Total Variance](https://en.wikipedia.org/wiki/Law_of_total_variance): $$\mathbb{Var}[Y] = \mathbb{E}[\mathbb{Var}[Y\mid X]] + \mathbb{Var}[\mathbb{E}[Y\mid X]]$$

We can use the Law of Total Variance to express $$\mathbb{Var}[f]$$ in $$n$$ was.

$$\mathbb{Var}[f] = \mathbb{Var}_{(-i)}[\mathbb{E}_{(i)}[f]] + \mathbb{E}_{(-i)}[\mathbb{Var}_{(i)}[f]] \quad \text{ for all } i \in [n]$$

Taking the sum over all $$n$$ ways of expressing $$\mathbb{Var}[f]$$, we get


$$\begin{align}
n\mathbb{Var}[f] &= \sum_{i=1}^n\mathbb{Var}_{(-i)}[\mathbb{E}_{(i)}[f]] + \sum_{i=1}^n\mathbb{E}_{(-i)}[\mathbb{Var}_{(i)}[f]]\\
&\geq \sum_{i=1}^n\mathbb{Var}_{(-i)}[\mathbb{E}_{(i)}[f]] + \mathbb{Var}[f]
\end{align}$$

The inequality is obtained by applying Efron-Stein to the second summation. This give us the final result.

$$\boxed{\mathbb{Var}[f] \geq \frac{1}{n-1}  \sum_{i=1}^n\mathbb{Var}_{(-i)}[\mathbb{E}_{(i)}[f]]}$$

Re-writing using more commonly used notation.

$$\boxed{\mathbb{Var}[f] \geq \frac{1}{n-1}  \sum_{i=1}^n\mathbb{Var}_{x_1,..x_{i-1},x_{i+1},..x_n}[\mathbb{E}_{x_i}[f \mid x_1,..x_{i-1},x_{i+1},..x_n]]}$$

This inequality is quite similar to Efron-Stein. Just interchange $$\mathbb{Var}$$ with $$\mathbb{E}$$ on the left, change $$\leq$$ to $$\geq$$ and scale by $$1/(n-1)$$. 

This inequality is also tight. Consider $$f = \sum_{i=1}^n x_i$$ where $$x_i$$ are drawn iid from a distribution with mean $$\mu$$ and variance $$\sigma^2$$.  

On the LHS, $$\mathbb{Var}[f]=n\sigma^2$$

On the RHS,

$$\begin{align}
\mathbb{Var}_{(-i)}[\mathbb{E}_{(i)}[f]]&= \mathbb{Var}_{(-i)}[x_1+..x_{i-1}+\mu+x_{i+1}+..+x_n]\\
&= (n-1)\sigma^2\\
 \frac{1}{n-1}  \sum_{i=1}^n\mathbb{Var}_{(-i)}[\mathbb{E}_{(i)}[f]]&= \frac{1}{n-1} n(n-1)\sigma^2 = n\sigma^2
\end{align}$$

So far, I haven't seen this inequality in any text or paper. If no else has seen it and it is new, then $$\text{YAY!}$$ Otherwise $${}_\text{yay!}$$. Hopefully some statistician or researcher can tell me if its a known result. Or better yet, uses it for their work.

Follow up post containing a more elegant result:[A Family of Efron-Stein Like Lower bounds for Variance](https://sudeepraja.github.io/EfronStein2/).
