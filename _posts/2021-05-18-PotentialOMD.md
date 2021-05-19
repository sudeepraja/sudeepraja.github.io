---
layout: post
title: Online Mirror Descent using Potentials
published: true
project: false
---

Let $$\psi$$ be a potential and $$f$$ be the associated function. Define $$F(x)=\sum_{i=1}^n f(x_i)$$. Define the Bregman Divergence as :

$$\text{Breg}(x\|y) = F(x) - F(y) - \nabla F(y)^\top (x-y)$$

### 1 step OMD
Consider the Online Mirror Descent(OMD) algorithm on the simplex $$\Delta_n$$ with regularizer $$F(x)$$. Assume the losses are linear. The 1-step OMD update is written as:

$$\begin{align*}
x_1 &= \arg\min_{x \in \Delta_n} F(x)\\
x_{t+1} &= \arg\min_{x \in \Delta_n} \left[\eta_t l_t^\top x + \text{Breg}(x\|x_t)\right]
\end{align*}$$

Solving the minimization above and using facts about potential functions along with induction, we can show that:

$$\begin{align*}
x_1 &=  \psi(\lambda(0)) = (1/n,\dots,1/n)\\
x_{t+1} &= \psi(\theta_t + \lambda(\theta_t)) \quad \text{ where } \quad \theta_t = - \sum_{s=1}^t \eta_s l_s
\end{align*}$$

So, OMD can be written as:
1. Let $$\theta_0=0$$
2. For $$t=1 \dots T$$
   1. Play $$x_t = \psi(\theta_{t-1} + \lambda(\theta_{t-1}))$$
   2. Observe $$l_t$$
   3. Update $$\theta_t = \theta_{t-1} - \eta_t l_t$$

### 2 step OMD
Sometimes, one can write OMD using a two step update:

$$\begin{align*}
x_1 &= \arg\min_{x \in \Delta_n} F(x)\\
\tilde{x}_{t+1} &= \arg\min_{x \in \mathbb{R}_n} \left[\eta_t l_t^\top x + \text{Breg}(x\|x_t)\right]\\
x_{t+1} &= \arg\min_{x \in \Delta_n} \text{Breg}(x\|\tilde{x}_{t+1})
\end{align*}$$

Solving the minimization above we can show that:

$$\begin{align*}
x_1 &=  \psi(\lambda(0)) = (1/n,\dots,1/n)\\
\tilde{x}_{t+1} &= \psi(\theta_t + \lambda(\theta_{t-1}))\\
x_{t+1} &= \psi(\theta_t + \lambda(\theta_t)) \quad \text{ where } \quad \theta_t = - \sum_{s=1}^t \eta_s l_s
\end{align*}$$

The 2 step OMD is valid if $$\tilde{x}_{t+1}$$ exists. Also, note that $$\tilde{x}_{t+1}$$ may not belong to the simplex.

# Regret Inequality

Using potentials, obtaining the regret inequality for OMD is considerably easier compared to previous techniques.

### 1 step OMD

For any $$x \in \Delta_n$$, we have:

$$\begin{align*}
l_t^\top(x_t-x) &= l_t^\top(x_{t+1}-x) + l_t^\top(x_t-x_{t+1})\\
&= \frac{1}{\eta_t}(\theta_{t-1}-\theta_t)^\top(x_{t+1}-x) + l_t^\top(x_t-x_{t+1})\\
&= \left(\frac{\nabla F(x_t)-\lambda(\theta_{t-1})}{\eta_t} - \frac{\nabla F(x_{t+1})-\lambda(\theta_t)}{\eta_t}\right)^\top(x_{t+1}-x) + l_t^\top(x_t-x_{t+1})\\
&= \left(\frac{\nabla F(x_t)}{\eta_t} - \frac{\nabla F(x_{t+1})}{\eta_t}\right)^\top(x_{t+1}-x) + l_t^\top(x_t-x_{t+1})
\end{align*}$$


We can write the first term as:

$$\begin{align*}
\left(\nabla F(x_t) - \nabla F(x_{t+1})\right)^\top(x_{t+1}-x) &= \text{Breg}(x\|x_{t}) - \text{Breg}(x\|x_{t+1}) - \text{Breg}(x_{t+1}\|x_{t})
\end{align*}$$

Taking summation over $$t$$, we have:

$$\begin{align*}
\sum_{t=1}^T l_t^\top(x_t-x) &= \sum_{t=1}^T \frac{1}{\eta_t}\left[ \text{Breg}(x\|x_{t}) - \text{Breg}(x\|x_{t+1}) \right] + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) - \frac{1}{\eta_t}\text{Breg}(x_{t+1}\|x_{t}) \right]\\
\end{align*}
$$

**Specific Results**:

- Constant learning rate $$\eta_t=\eta$$. We can see that the first term telescopes:

  $$\begin{align*}
  \sum_{t=1}^T l_t^\top(x_t-x) &= \frac{1}{\eta} \left[\text{Breg}(x\|x_1) - \text{Breg}(x\|x_{T+1}) \right] + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) - \frac{1}{\eta}\text{Breg}(x_{t+1}\|x_{t}) \right]\\
  &\leq \frac{1}{\eta} \text{Breg}(x\|x_1) + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) - \frac{1}{\eta}\text{Breg}(x_{t+1}\|x_{t}) \right]
  \end{align*}$$

- Non-increasing learning rate $$\eta_t\geq \eta_{t+1}$$ and $$\sup_{1\leq t\leq T}\text{Breg}(x\|x_t) < D$$. We have:

  $$\begin{align*}
  \sum_{t=1}^T l_t^\top(x_t-x) &\leq \frac{1}{\eta_1} \text{Breg}(x\|x_1) + \sum_{t=2}^{T} \text{Breg}(x\|x_{t})\left(\frac{1}{\eta_{t}} - \frac{1}{\eta_{t-1}}\right)  + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) - \frac{1}{\eta_t}\text{Breg}(x_{t+1}\|x_{t}) \right]\\
  &\leq \frac{1}{\eta_T} D + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) - \frac{1}{\eta_t}\text{Breg}(x_{t+1}\|x_{t}) \right]
  \end{align*}$$

- Non-decreasing learning rate $$\eta_t\leq \eta_{t+1}$$. We have:

  $$\begin{align*}
  \sum_{t=1}^T l_t^\top(x_t-x) &\leq \frac{1}{\eta_1} \text{Breg}(x\|x_1) + \sum_{t=2}^{T} \text{Breg}(x\|x_{t})\left(\frac{1}{\eta_{t}} - \frac{1}{\eta_{t-1}}\right)  + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) - \frac{1}{\eta_t}\text{Breg}(x_{t+1}\|x_{t}) \right]\\
  &\leq \frac{1}{\eta_1} \text{Breg}(x\|x_1) + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) - \frac{1}{\eta_t}\text{Breg}(x_{t+1}\|x_{t}) \right]
  \end{align*}$$


### 2-step OMD

For any $$x \in \Delta_n$$, we have:

$$\begin{align*}
l_t^\top(x_t-x) &= l_t^\top(\tilde{x}_{t+1}-x) + l_t^\top(x_t-\tilde{x}_{t+1})\\
&= \frac{1}{\eta_t}(\theta_{t-1}-\theta_t)^\top(\tilde{x}_{t+1}-x) + l_t^\top(x_t-\tilde{x}_{t+1})\\
&= \left(\frac{\nabla F(x_t)-\lambda(\theta_{t-1})}{\eta_t} - \frac{\nabla F(\tilde{x}_{t+1})-\lambda(\theta_{t-1})}{\eta_t}\right)^\top(\tilde{x}_{t+1}-x) + l_t^\top(x_t-\tilde{x}_{t+1})\\
&= \left(\frac{\nabla F(x_t)}{\eta_t} - \frac{\nabla F(\tilde{x}_{t+1})}{\eta_t}\right)^\top(\tilde{x}_{t+1}-x) + l_t^\top(x_t-\tilde{x}_{t+1})
\end{align*}$$


Note that $$\text{Breg}(x\|x_{t+1}) \leq \text{Breg}(x\|\tilde{x}_{t+1})$$ for all $$x \in \Delta_n$$.

$$\begin{align*}
 \text{Breg}(x\|\tilde{x}_{t+1}) - \text{Breg}(x\|x_{t+1})  &= F(x_{t+1}) - F(\tilde{x}_{t+1}) - \nabla F(\tilde{x}_{t+1})^\top (x-\tilde{x}_{t+1}) + \nabla F(x_{t+1})^\top (x-x_{t+1}) \\
 &= \text{Breg}(x_{t+1}\| \tilde{x}_{t+1}) + ( \nabla F(x_{t+1}) - \nabla F(\tilde{x}_{t+1}))^\top (x-x_{t+1}) \\
 &\geq  ( \nabla F(x_{t+1}) - \nabla F(\tilde{x}_{t+1}))^\top (x-x_{t+1})
\end{align*}$$

Since $$x_{t+1}$$ is the minimizer of $$\text{Breg}(x\| \tilde{p}_{t+1})$$ on $$x \in \Delta_n$$, we have the optimality condition:

$$(\nabla F(x_{t+1} ) - \nabla F(\tilde{x}_{t+1}))^\top (x-x_{t+1}) \geq 0$$

We can write the first term as:

$$\begin{align*}
\left(\nabla F(x_t) - \nabla F(\tilde{x}_{t+1})\right)^\top(\tilde{x}_{t+1}-x) &= \text{Breg}(x\|x_{t}) - \text{Breg}(x\|\tilde{x}_{t+1}) - \text{Breg}(\tilde{x}_{t+1}\|x_{t})\\
&\leq \text{Breg}(x\|x_{t}) - \text{Breg}(x\|x_{t+1}) - \text{Breg}(\tilde{x}_{t+1}\|x_{t})\\
\end{align*}$$

Taking summation over $$t$$, we have:

$$\begin{align*}
\sum_{t=1}^T l_t^\top(x_t-x) &\leq \sum_{t=1}^T \frac{1}{\eta_t}\left[ \text{Breg}(x\|x_{t}) - \text{Breg}(x\|x_{t+1}) \right] + \sum_{t=1}^T \left[ l_t^\top(x_t-\tilde{x}_{t+1}) - \frac{1}{\eta_t}\text{Breg}(\tilde{x}_{t+1}\|x_{t}) \right]\\
\end{align*}
$$

For the three cases, we can get similar results with $$x_{t+1}$$ replaced by $$\tilde{x}_{t+1}$$ in the second summation.
