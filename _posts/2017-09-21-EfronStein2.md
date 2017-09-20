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

