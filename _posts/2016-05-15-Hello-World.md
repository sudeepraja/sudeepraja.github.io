---
layout: post
title: Ratio of Heads and Tails
published: true
---


Recently, I had the opportunity to interview for the Research Fellow position at Microsoft Research India. In one of the interviews I was asked a peculiar question on probability. I took the liberty to change the question a little:

You have $$n$$ fair coins to begin with. The coins are flipped in rounds. In each round, you flip all the available coins and note down the results. Only if a coin turns out to be TAILS it goes to the next round, otherwise it is does not. After the last round ends, what is the ratio of total HEADS obtained to TAILS?

As each coin is flipped until a HEADS is obtained, the total number of HEADS will be $$n$$.

So the number of TAILS for a certain coin must be: number of trials of the coin $$- 1$$

The expected number of trials for a coin is given by the sequence:

$$
\begin{align*}
& E[ trails ] = 1 \cdot \frac{1}{2} + 2 \cdot \frac{1}{2} \cdot \frac{1}{2} + 3 \cdot \left( \frac{1}{2} \right)^{2} \cdot \frac{1}{2} + 4 \cdot \left( \frac{1}{2} \right) ^{3}  \cdot \frac{1}{2} + .....
\end{align*}
$$






