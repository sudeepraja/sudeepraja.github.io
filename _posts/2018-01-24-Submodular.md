---
layout: post
title: Submodularity and the Lovász extension
published: true
project: false
---

This blog is primarily about an easier way of understanding the Lovasz extension of submodular functions.

### Submodularity

Submodularity is a property of set functions which comes up very often in several areas of computer science. Many problems in combinatorial optimization and machine learning have submodular structure. Submodularity has two equivalent definitions.

Let $$[n]=\{1,2,..,n\}$$. The set of all subsets of $$[n]$$ is $$2^{[n]}$$. A set function $$f:2^{[n]} \to \mathbb{R}$$ is submodular if for all sets $$S,T \subset [n]$$ such that $$T \subset S$$, and for all elements $$i \in [n]$$, we have:

$$f(T \cup \{i\}) - f(T) \geq f(S \cup \{i\}) - f(S)$$

This is called as the diminishing returns property. Equivalently, a function $$f:2^{[n]} \to \mathbb{R}$$ is submodular if for all sets $$S,T \subset [n]$$, we have:

$$f(S) + f(T) \geq f(S \cup T) + f(S \cap T)$$

Some commonly encountered submodular functions are:

 1. Linear monotone functions : finding min/max spanning trees.
 2. Coverage functions : finding graph/hypergraph covers.
 3. Cut functions : finding min/max cuts of a graph. 
 4. Entropy function.

Let $$H_n=\{0,1\}^n$$ and $$K_n=[0,1]^n$$.  We can represent a set $$S \subset [n]$$ using its characteristic vector $$X_S \in H_n$$, which is defined $$X_S(i)=1$$ if $$i \in S$$ and $$0$$ otherwise. For the sake of convenience, we overload the definition of submodular functions as $$f:H_n \to \mathbb{R}$$.

It is commonly said that Submodularity is the "discrete counterpart of convexity". I feel this is misleading. Submodularity has interesting connections to both convex and concave functions. There are at least three extensions which construct a continuous function on $$K_n$$ given a submodular function.

 1. Convex Closure
 2. Concave Closure
 3. Multilinear Extension

We will only talk about the Convex closure. Although a convex closure can be constructed for any set function, not just submodular. For submodular functions, the convex closure is the same as the Lovász extension which is defined below.

### Lovász extension

The Lovász extension is a very useful construction that can be used for Submodular minimization. The Lovász extension of a submodular function is always convex and can be minimized efficiently. The minima of the Lovász extension can be used to find the minima of the submodular function.

Given a submodular function $$f:H_n \to \mathbb{R}$$, the Lovasz Extension is the function $$\hat{f}:K_n \to \mathbb{R}$$ defined as

$$\hat{f}(x) = \sum_{i=0}^n \lambda_i f(X_{S_i})$$

where $$\emptyset = S_0 \subset S_1\subset S_2...\subset S_n=[n]$$ is a chain such that $$\sum_{i=0}^n \lambda_i X_{S_i} =x$$ and $$\sum_{i=0}^n \lambda_i = 1$$.

This definition does not tell us how to find the chain or the coefficients $$\lambda_i$$.

In  the hypercube $$H_n$$, there are $$n!$$ unique shortest paths from $$0_n$$ to $$1_n$$, where $$0_n$$ and $$1_n$$ are the vectors of all 0s and 1s respectively. Let $$P=[0_n=X_0,X_1,..,1_n=X_n]$$ denote a path. Let $$C_P$$ be the convex hull of the points on the path $$P$$. There exist $$n!$$ such convex hull, one for each path. Moreover, these convex hulls partition $$K_n$$ into $$n!$$ equal parts.

Given a point $$x \in K_n$$, it is possible to determine the convex hull $$C_P$$ and path $$P=[0_n=X_0,X_1,..,1_n=X_n]$$ such that $$x \in C_P$$. We can find the coefficients $$\lambda_i$$ such that $$x = \sum_{i=0}^n \lambda_i X_i$$, where $$\sum_{i=0}^n \lambda_i=1$$ and $$\lambda_i\geq 0$$. The Lovasz Extension $$\hat{f}$$ at $$x$$ is defined to be

$$\hat{f}(x) = \sum_{i=0}^p \lambda_i f(X_i)$$

Let $$x$$ be the vector $$(x_1,x_2,..,x_n)$$. Let $\pi:[n] \to [n]$ be the sorting permutation of $$x_1,x_2,..,x_n$$. That means if $$\pi(i)=j$$, then $$x_j$$ is the $$i$$th largest element in the vector $$x$$. So $$1\geq x_{\pi(1)} \geq x_{\pi(2)} \geq ..\geq x_{\pi(n)} \geq 0$$. Let $$x_{\pi(0)}=1$$ and $$x_{\pi(n+1)} = 0$$. The $$\lambda$$'s are the gaps between $$x_{\pi(0)}, x_{\pi(1)} , x_{\pi(2)} , .., x_{\pi(n)} , x_{\pi(n+1)}$$.

$$\lambda_i = x_{\pi(i)}-x_{\pi(i+1)}$$

Let $$X_0 = 0_n$$. Then, $$X_1 = X_0 + e_{\pi(1)}$$, $$X_2 = X_1 + e_{\pi(2)}$$ and so on. Here $$e_{\pi(i)}$$ is the vector with a 1 at position $$\pi(i)$$ and all other elements are 0. In general, we have:

$$X_i = X_{i-1} + e_{\pi(i)}$$

The summation, $$\sum_{i=0}^n \lambda_i X_i$$ can be re-written as:

$$\begin{align}
\sum_{i=0}^n \lambda_i X_i &= (1-x_{\pi(1)})X_0+\sum_{i=1}^n (x_{\pi(i)}-x_{\pi(i+1)}) (X_{i-1}+e_{\pi(i)})\\ &= \sum_{i=1}^n e_{\pi(i)} (\sum_{j=i}^n x_{\pi(j)}-x_{\pi(j+1)})\\ &= \sum_{i=1}^n e_{\pi(i)} x_{\pi(i)}\\ &= x
\end{align}$$

