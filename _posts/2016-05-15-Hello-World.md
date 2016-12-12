---
layout: post
title: Ratio of Heads and Tails
published: true
project: true
---

This is my first blog post. In this blog, I would be writing about Algorithms, Machine Learning, Optimization and Mathematics. I wanted to begin with something simple, so this post would be about a problem from Probability.

Recently, I had the opportunity to interview for the Research Fellow position at Microsoft Research India. In one of the interviews I was asked a peculiar question on probability. I took the liberty to change the question a little:

_You have $$n$$ fair coins to begin with. The coins are flipped in rounds. In each round, you flip all the available coins and note down the results. Only if a coin turns out to be TAILS it goes to the next round, otherwise it is does not. After the last round ends, what is the expected ratio of total HEADS obtained in all rounds to TAILS obtained in all rounds?_

As each coin is flipped until a HEADS is obtained, the total number of HEADS will be $$n$$.

The number of TAILS obtained for a certain coin must be: number of trials of the coin $$- 1$$

The expected number of trials for a coin is given by the sequence:

$$
\begin{align*}
& E[ trials ] = 1 \cdot \frac{1}{2} + 2 \cdot \frac{1}{2} \cdot \frac{1}{2} + 3 \cdot \left( \frac{1}{2} \right)^{2} \cdot \frac{1}{2} + 4 \cdot \left( \frac{1}{2} \right) ^{3}  \cdot \frac{1}{2}  +  ..... \\
& E[trials] = \sum_{i=1}^{\infty} i \cdot \left( \frac{1}{2}  \right)^{i-1} \cdot \frac{1}{2} \\
& E[trials] = \sum_{i=1}^{\infty} i \cdot \left( \frac{1}{2}  \right)^{i} \\
\end{align*}
$$

We know the summation for an infinite geometric series:

$$
\begin{align*}
& 1 + x + x^2 + x^3 + .... = \frac{1}{1-x}, if |x| < 1
\end{align*}
$$

Differentiate both sides with $$x$$, that would give us:

$$
\begin{align*}
& 1 + 2 \cdot x + 3 \cdot x^2 + 4 \cdot x^3 + .... = \frac{1}{(1-x)^2}, if |x| < 1
\end{align*}
$$

Now multiply both sides with $$x$$, that would give us:

$$
\begin{align*}
& x + 2 \cdot x^2 + 3 \cdot x^3 + 4 \cdot x^4 + .... = \frac{x}{(1-x)^2}, if |x| < 1
\end{align*}
$$

So, the expected number of trials would be:

$$
\begin{align*}
& E[trials] = \sum_{i=1}^{\infty} i \cdot \left( \frac{1}{2}  \right)^{i}  = \frac { \frac{1}{2} }{ ( 1-\frac{1}{2} )^2 } = 2
\end{align*}
$$

The expected number of TAILS for a coin would be $$ 2 - 1 = 1 $$. The ratio of HEADS and TAILS would be $$ 1:1 $$.

This can also be seen intuitively. In the beginning, you would toss $$n$$ coins, half of them would turn out to be HEADS and the other half TAILS. Now you are left with $$ n/2 $$ coins, out of which half would be HEADS and half would be TAILS.

The number of HEADS would be: $$\frac{n}{2} + \frac{n}{4} + \frac{n}{8} + ... = n$$. Which is what we had said earlier.

The number of TAILS would be: $$\frac{n}{2} + \frac{n}{4} + \frac{n}{8} + ... = n$$. 

The ratio will be $$ 1:1 $$.

Now to take this question further, you use $$n$$ fair dice and stop rolling when you get an even number. What would be the expected ratio of the sum of even numbers to the sum of odd numbers obtained ?
