---
layout: post
title: Fixed point iteration for finding $$(I_n+UV)^{-1}$$
published: true
project: false
---

Let $$B=I_n+UV$$, where $$U \in \mathbb{R}^{n \times k}$$ and $$V \in \mathbb{R}^{k \times n}$$ such that $$k>>n$$. The goal is to find $B^{-1}$. Using the **[Sherman–Morrison–Woodbury formula](https://en.wikipedia.org/wiki/Woodbury_matrix_identity)**, we have:

$$B^{-1} = (I_n+UV)^{-1}= I_n - U(I_k+VU)^{-1}V$$

Using the Woodbury matrix identity again:

$$(I_k+VU)^{-1} = I_k - V(I_n+UV)^{-1}U$$

Substituting this in the above expression, we get:

$$(I_n+UV)^{-1} = I_n - UV + UV(I_n+UV)^{-1}UV$$

Which is the same as:

$$B^{-1} = I_n - UV + UVB^{-1}UV$$

This gives us a fixed point iteration for finding $$B^{-1}$$. Let $$B^{-1}_{(0)} = I_n$$. We apply the following fixed point equation repeatedly:

$$B^{-1}_{(k+1)} = I_n - UV + UVB^{-1}_{(k)}UV$$

We obtain:

$$\begin{align}
B^{-1}_{(0)} &= I_n\\
B^{-1}_{(1)} &= I_n - UV  + UV(B^{-1}_{(0)})UV = I_n - UV +(UV)^2\\
B^{-1}_{(2)} &= I_n - UV+ UV(B^{-1}_{(1)})UV = I_n - UV +(UV)^2 - (UV)^3 + (UV)^4\\
&\vdots\\
B^{-1}_{(k)} &= \sum_{i=0}^{2k} (-1)^i (UV)^i\\
\end{align}$$

$$B^{-1} =\lim_{k \to \infty} B^{-1}_{(k)}$$ if and only if $$B(\lim_{k \to \infty} B^{-1}_{(k)}) = (\lim_{k \to \infty} B^{-1}_{(k)}) B = I_n$$

Consider $$B(\lim_{k \to \infty} B^{-1}_{(k)})$$ 

$$\begin{align}
B(\lim_{k \to \infty} B^{-1}_{(k)})&=\lim_{k \to \infty} BB^{-1}_{(k)}\\
&=\lim_{k \to \infty} (I_n+UV)\left(\sum_{i=0}^{2k} (-1)^i (UV)^i \right)\\
&= \lim_{k \to \infty} \sum_{i=0}^{2k} (-1)^i (UV)^i  + UV \sum_{i=0}^{2k} (-1)^i (UV)^i \\
&=\lim_{k \to \infty} \sum_{i=0}^{2k} (-1)^i (UV)^i + \sum_{i=0}^{2k} (-1)^i (UV)^{i+1}\\
&= \lim_{k \to \infty} \sum_{i=0}^{2k} (-1)^i (UV)^i + \sum_{i=0}^{2k} (-1)^i (UV)^{i+1}\\
&= \lim_{k \to \infty} I_n + (UV)^{k+1}\\
&= I_n + \lim_{k \to \infty}(UV)^{k+1}
\end{align}$$

If we consider $$(\lim_{k \to \infty} B^{-1}_{(k)})B$$, we also get the same expression. So, we must have that $$I_n = I_n + \lim_{k \to \infty}(UV)^{k+1}$$ i.e, $$\lim_{k \to \infty}(UV)^{k} = 0$$

The matrix $$UV$$ is said to be [Convergent](https://en.wikipedia.org/wiki/Convergent_matrix) if $$\lim_{k \to \infty}(UV)^{k} = 0$$. The necessary and sufficient condition for $$UV$$ to be convergent is that its [Spectral radius](https://en.wikipedia.org/wiki/Spectral_radius) (largest absolute value of its eigenvalues) is strictly less than 1.

So, this fixed point iteration find $$(I_n+UV)^{-1}$$ if and only if the spectral radius of $$UV$$ is strictly less than 1.