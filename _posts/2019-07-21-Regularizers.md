---
layout: post
title: Regularizers for Online Mirror Descent
published: true
project: false
---

This is a reference blog post about useful regularizers for Online Mirror Descent. This is by no means an exhaustive list. I have only added regularizers that I have seen in papers.

1. Squared Euclidean Norm: This regularizer gives Online Gradient Descent

$$\frac{1}{2}\|x\|_2^2$$

2. Negative Entropy: On the probability simplex, this regularizer gives the Hedge algorithm.

$$\sum_{i=1}^n x_i \log x_i$$

3. Log barrier: Useful in deriving adaptive bandit bounds. First introduced in [1]

$$\sum_{i=1}^n \log \frac{1}{x_i}$$

4. Tsallis Entropy: This is defined for $$q\in (0,1)$$. When $$q=1/2$$, OMD with this entropy can be shown to match the regret lower bound of $$\sqrt{TK}$$ in the multi armed bandit problem. Also shown to achive best of both worlds kind of bounds [2] when $$q=1/2$$.

$$\frac{1- \sum_{i=1}^n x_i^q}{1-q} $$

5. Hyperbolic Entropy: Introduced in [3]. This regularizer inerpolates between OGD and Hedge based on $$\beta$$.

$$\sum_{i=1}^n \left(x_i \sinh^{-1}\left(\frac{x_i}{\beta}\right)- \sqrt{x_i^2+\beta^2}\right)$$

6. Negative Von-Neuman Entropy: When used on the spectrahedron, gives the Matrix Multiplicative weights algorithm [4].

$$\text{Tr}(X \log(X))$$

7. Log Determinant: Used in Bandit PCA [5].

$$\log \frac{1}{\det(X)}$$

8. Matrix Tsallis Entropy: Used in [6] for an online optimization proof of the Batson-Spielman-Srivastava  spectral sparsification result. $$q\in (0,1)$$

$$\frac{1-\text{Tr}(X^q)}{1-q}$$

9. Soft Exploration: Used in [7] for solving several open prblems. Introduced the idea of "soft" exploration as opposed to using "forced" exploration $$(1-\gamma)x_t + \frac{\gamma}{n}$$.

$$\sum_{i=1}^n x_i \log x_i - \gamma \sum_{i=1}^n \log x_i$$

## References
1. Foster, Dylan J., et al. "Learning in games: Robustness of fast convergence." Advances in Neural Information Processing Systems. 2016.
2. Zimmert, Julian, and Yevgeny Seldin. "An optimal algorithm for stochastic and adversarial bandits." arXiv preprint arXiv:1807.07623 (2018).
3. Ghai, Udaya, Elad Hazan, and Yoram Singer. "Exponentiated gradient meets gradient descent." arXiv preprint arXiv:1902.01903 (2019).
4. Arora, Sanjeev, Elad Hazan, and Satyen Kale. "The multiplicative weights update method: a meta-algorithm and applications." Theory of Computing 8.1 (2012): 121-164
5. Kotłowski, Wojciech and Gergely Neu. "Bandit Principal Component Analysis" Proceedings of Machine Learning Research vol 99:1–31, 2019 32nd Annual Conference on Learning Theory
6. Allen-Zhu, Zeyuan, Zhenyu Liao, and Lorenzo Orecchia. "Spectral sparsification and regret minimization beyond matrix multiplicative updates." Proceedings of the forty-seventh annual ACM symposium on Theory of computing. ACM, 2015
7. Bubeck, Sébastien, Michael B. Cohen, and Yuanzhi Li. "Sparsity, variance and curvature in multi-armed bandits." arXiv preprint arXiv:1711.01037 (2017).
