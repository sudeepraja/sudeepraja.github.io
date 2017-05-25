---
layout: post
title: Linear Regression with and without Calculus
published: true
project: false
---

This post will be about simple linear regression. I will first use calculus to derive an expression. Then I derive same expression in a more intuitive way using linear algebra.

You have $$n$$ data point and observation pairs $$(a_1,b_1),(a_2,b_2),..,(a_n,b_n)$$ where $$a_i \in \mathbb{R}^m$$ and  $$b_i \in \mathbb{R}$$.  Let $$A_i \in \mathbb{R}^{m+1}$$ be the vector obtained by prepending 1  to vector $$a_i$$. The matrix $$A$$ has $$A_i$$ as its $$i$$th row and $$b \in \mathbb{R}^n $$ is the column vector consisting of $$b_i$$.  

$$
\begin{align*}
A=\begin{pmatrix}  
		1 & a_{11} & a_{12} & \cdots & a_{1m}  \\
        1 & a_{21} & a_{22} & \cdots & a_{2m} \\
        \vdots &\vdots & \vdots & \ddots & \vdots \\
        1 & a_{n1} & a_{n2} & \cdots & a_{nm} \\
  \end{pmatrix}
\quad
b=\begin{pmatrix}  
		b_{1}\\
        b_{2}\\
        \vdots \\
        b_{n}\\
  \end{pmatrix}
\end{align*}
$$

We are supposed to find the vector $$x$$ such that $$ \| b-Ax \|^2 $$ is minimum. What this means is vector $$Ax$$ is as close as possible to vector $$b$$.

### Using Calculus

The approach is straightforward. Differentiate $$ \| b-Ax \|^2 $$ with $$x$$ and equate it to zero. A good reference for identities regarding differentiation with matrices and vectors is the [Matrix Cookbook](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf).

$$
\begin{align*}
 \| b-Ax \|^2 &= (b-Ax)^T(b-Ax)\\
 &= b^Tb - x^TA^Tb - b^TAx + x^TA^TAx
\end{align*}
$$

$$x^TA^Tb$$ and $$b^TAx$$ are scalars, so $$x^TA^Tb = b^TAx$$

$$
\begin{align*}
 \| b-Ax \|^2 &= b^Tb - 2x^TA^Tb + x^TA^TAx
\end{align*}
$$

Differentiating this with $$x$$. 

$$
\frac{d( b^Tb - 2x^TA^Tb + x^TA^TAx)}{dx} = -2 \frac{d(x^TA^Tb)}{dx} + \frac{d(x^TA^TAx)}{dx}
$$

Since $$\frac{d(x^Tc)}{dx} = c$$ and $$\frac{d(x^TCx)}{dx} = (C+C^T)x$$.

$$ -2 \frac{d(x^TA^Tb)}{dx} + \frac{d(x^TA^TAx)}{dx} = -2A^Tb + 2A^TAx$$

Equating this to 0.

$$
A^TAx=A^Tb\\
x=(A^TA)^{-1}A^Tb\\
$$


### Using Linear Algebra

First a detour in geometry. You are given a point $$z$$ and a plane $$P$$ in 3 dimensional space. The point closest to $$z$$ on $$P$$ will be the projection of $$z$$ on $$P$$.  The line joining $$z$$ to this projection would be orthogonal to $$P$$.
<p align="center">
<img src="/images/plane_point.png">
</p>
Consider the expression $$Ax$$. This can be seen as a subspace spanned by the columns([column space](https://en.wikipedia.org/wiki/Row_and_column_spaces)) of $$A$$. So $$Ax$$ represents a point in the column space of $$A$$.

$$Ax=\begin{pmatrix}  
		1 & a_{11} & a_{12} & \cdots & a_{1m}  \\
        1 & a_{21} & a_{22} & \cdots & a_{2m} \\
        \vdots &\vdots & \vdots & \ddots & \vdots \\
        1 & a_{n1} & a_{n2} & \cdots & a_{nm} \\
	     \end{pmatrix}
	     \begin{pmatrix}  
		x_{0}\\
        x_{1}\\
        \vdots \\
        x_{m}\\
     \end{pmatrix}\\
     = x_0\begin{pmatrix}  
		1\\
        1\\
        \vdots \\
        1\\
     \end{pmatrix} + x_1\begin{pmatrix}  
		a_{11}\\
        a_{21}\\
        \vdots \\
        a_{n1}\\
     \end{pmatrix} + x_2\begin{pmatrix}  
		a_{12}\\
        a_{22}\\
        \vdots \\
        a_{n2}\\
     \end{pmatrix} + \cdots +  x_m\begin{pmatrix}  
		a_{1m}\\
        a_{2m}\\
        \vdots \\
        a_{nm}\\
     \end{pmatrix} 
 $$
 
In the linear regression problem, we want to find $$x$$ such that $$ \| b-Ax \|^2 $$ is minimized. If $$b$$ lies in the column space of $$A$$, then through Gaussian elimination, we can find $$x$$ such that $$Ax=b$$ and $$ \| b-Ax \|^2 =0$$. But in general, $$b$$ may be anywhere in the $$n$$ dimensional space. So the best thing we can do is find the point $$Ax$$ in the column space of $$A$$ which is closest to $$b$$. This point will be the [projection](https://ocw.mit.edu/courses/mathematics/18-06sc-linear-algebra-fall-2011/least-squares-determinants-and-eigenvalues/projections-onto-subspaces/MIT18_06SCF11_Ses2.2sum.pdf) of $$b$$ onto column space of $$A$$. So the line $$(Ax-b)$$ will be orthogonal to the column space of $$A$$ (will belong to the [Left null space](https://ocw.mit.edu/courses/mathematics/18-06sc-linear-algebra-fall-2011/least-squares-determinants-and-eigenvalues/orthogonal-vectors-and-subspaces/MIT18_06SCF11_Ses2.1sum.pdf) of $$A$$):

$$
A^T(Ax - b) = 0\\
A^TAx = A^Tb\\
x = (A^TA)^{-1}A^Tb\\
$$

Although we have an exact expression for finding the optimum $$x$$ for linear regression, for large datasets, evaluating this expression is inefficient. In practice, gradient descent methods are used. I will write about gradient descent in one of the posts to come.
