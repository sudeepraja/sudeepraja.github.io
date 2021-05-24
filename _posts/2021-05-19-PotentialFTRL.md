---
layout: post
title: Follow the Regularized Leader using Potentials
published: true
project: false
---

Let $$\psi$$ be a potential and $$f$$ be the associated function. Define $$F(x)=\sum_{i=1}^n f(x_i)$$.

### FTRL
Consider the Follow the Regularized Leader(FTRL) algorithm on the simplex $$\Delta_n$$ with regularizer $$F(x)$$. Assume the losses are linear. The FTRL update is written as:

$$\begin{align*}
x_1 &= \arg\min_{x \in \Delta_n} F(x)\\
x_{t+1} &= \arg\min_{x \in \Delta_n} \left[\eta_t \sum_{s=1}^t l_s^\top x + F(x)\right]
\end{align*}$$

Solving the minimization above and using facts about potential functions, we can show that:

$$\begin{align*}
x_1 &=  \psi(\lambda(0)) = (1/n,\dots,1/n)\\
x_{t+1} &= \psi(\theta_t + \lambda(\theta_t)) \quad \text{ where } \quad \theta_t =-\eta_t \sum_{s=1}^t  l_s
\end{align*}$$

So, FTRL can be written as:
1. Let $$\theta_0=0$$
2. For $$t=1 \dots T$$
   1. Play $$x_t = \psi(\theta_{t-1} + \lambda(\theta_{t-1}))$$
   2. Observe $$l_t$$
   3. Update $$\theta_t = \frac{\eta_t}{\eta_{t-1}}\theta_{t-1} - \eta_t l_t$$

Note that $$\eta_{0}$$ can be any non-zero number as $$\theta_0=0$$.

# Regret Inequality

The current techniques for obtaining FTRL's regret inequality uses "Be The Leader" lemma. However, when using potential functions, we can obtain the same result without this lemma.

For any $$x \in \Delta_n$$, we have:

$$\begin{align*}
l_t^\top(x_t-x) &= l_t^\top(x_{t+1}-x) + l_t^\top(x_t-x_{t+1})\\
&= \left(\frac{\theta_{t-1}}{\eta_{t-1}}-\frac{\theta_t}{\eta_t}\right)^\top(x_{t+1}-x) + l_t^\top(x_t-x_{t+1})\\
&= \left(\frac{\nabla F(x_t)-\lambda(\theta_{t-1})}{\eta_{t-1}} - \frac{\nabla F(x_{t+1})-\lambda(\theta_t)}{\eta_t}\right)^\top(x_{t+1}-x) + l_t^\top(x_t-x_{t+1})\\
&= \left(\frac{\nabla F(x_t)}{\eta_{t-1}} - \frac{\nabla F(x_{t+1})}{\eta_t}\right)^\top(x_{t+1}-x) + l_t^\top(x_t-x_{t+1})
\end{align*}$$

Define the "Mixed Bregman" $$\text{Breg}_{a,b}(x\|y)$$:

$$\text{Breg}_{a,b}(x\|y) = \frac{F(x)}{a} - \frac{F(y)}{b} - \frac{\nabla F(y)}{b}^\top(x-y)$$

The first term can be written as:

$$\left(\frac{\nabla F(x_t)}{\eta_{t-1}} - \frac{\nabla F(x_{t+1})}{\eta_t}\right)^\top(x_{t+1}-x)  = \text{Breg}_{\alpha,\eta_{t-1}}(x\|x_t) - \text{Breg}_{\alpha,\eta_{t}}(x\|x_{t+1})- \text{Breg}_{\eta_t,\eta_{t-1}}(x_{t+1}\|x_t)$$

$$\alpha$$ could be any non-zero number in the above expression.

Taking summation over $$t$$, we have:

$$\begin{align*}
\sum_{t=1}^T l_t^\top(x_t-x) &= \sum_{t=1}^T \left[\text{Breg}_{\alpha,\eta_{t-1}}(x\|x_t) - \text{Breg}_{\alpha,\eta_{t}}(x\|x_{t+1})\right] + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) -  \text{Breg}_{\eta_t,\eta_{t-1}}(x_{t+1}\|x_t)\right]\\
&=\text{Breg}_{\alpha,\eta_{0}}(x\|x_1) - \text{Breg}_{\alpha,\eta_{T}}(x\|x_{T+1}) + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) -  \text{Breg}_{\eta_t,\eta_{t-1}}(x_{t+1}\|x_t)\right]
\end{align*}$$

Let $$\alpha = \eta_0 = \eta_T$$. The Mixed Bregmans in the first term will become normal Bregmans.

$$\begin{align*}
\sum_{t=1}^T l_t^\top(x_t-x) &=\frac{1}{\eta_T}\text{Breg}(x\|x_1) - \text{Breg}(x\|x_{T+1}) + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) -  \text{Breg}_{\eta_t,\eta_{t-1}}(x_{t+1}\|x_t)\right]\\
&\leq \frac{1}{\eta_T}\text{Breg}(x\|x_1) + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) -  \text{Breg}_{\eta_t,\eta_{t-1}}(x_{t+1}\|x_t)\right]\\
\end{align*}$$

**Specific Results:**

- Constant Learning rate $$\eta_t = \eta$$. The Mixed Bregmans in the second term become normal Bregmans. We recover the same regret bound as OMD with constant learning rate.

  $$\sum_{t=1}^T l_t^\top(x_t-x) \leq \frac{1}{\eta}\text{Breg}(x\|x_1) + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) -  \frac{1}{\eta}\text{Breg}(x_{t+1}\|x_t)\right]$$

- If we can ensure that $$\text{Breg}_{\eta_t,\eta_{t-1}}(x_{t+1}\|x_t) \geq \frac{1}{\eta_{t-1}} \text{Breg}(x_{t+1}\|x_t)$$, then we have the inequality:

  $$\begin{align*}
  \sum_{t=1}^T l_t^\top(x_t-x)
  &\leq \frac{1}{\eta_T}\text{Breg}(x\|x_1) + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) -  \frac{1}{\eta_{t-1}}\text{Breg}(x_{t+1}\|x_t)\right]\\
  \end{align*}$$

  $$\text{Breg}_{\eta_t,\eta_{t-1}}(x_{t+1}\|x_t) \geq \frac{1}{\eta_{t-1}} \text{Breg}(x_{t+1}\|x_t)$$ is equivalent to:

  $$F(x_{t+1})\left( \frac{1}{\eta_t} - \frac{1}{\eta_{t-1}}\right)\geq 0$$

  This can hold if $$F(x)\geq 0$$ for all $$x \in \Delta_n$$ and $$\eta_t$$ are non-increasing, i.e. $$\eta_t\geq \eta_{t+1}$$.

  Otherwise, it can hold if $$F(x)\leq 0$$ for all $$x \in \Delta_n$$ and $$\eta_t$$ are non-decreasing, i.e. $$\eta_t \leq \eta_{t+1}$$

  $$ \quad $$

- If we can ensure that $$\text{Breg}_{\eta_t,\eta_{t-1}}(x_{t+1}\|x_t) \geq \frac{1}{\eta_{t}} \text{Breg}(x_{t+1}\|x_t)$$, then we have the inequality:

  $$\begin{align*}
  \sum_{t=1}^T l_t^\top(x_t-x)
  &\leq \frac{1}{\eta_T}\text{Breg}(x\|x_1) + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) -  \frac{1}{\eta_{t}}\text{Breg}(x_{t+1}\|x_t)\right]\\
  \end{align*}$$

  $$\text{Breg}_{\eta_t,\eta_{t-1}}(x_{t+1}\|x_t) \geq \frac{1}{\eta_{t}} \text{Breg}(x_{t+1}\|x_t)$$ is equivalent to:

  $$(F(x_{t}) + \nabla F(x_{t})^\top(x_{t+1}-x_t))\left( \frac{1}{\eta_t} - \frac{1}{\eta_{t-1}}\right)\geq 0$$

  This can hold if $$(F(x) + \nabla F(x)^\top(y-x))\geq 0$$ for all $$x,y \in \Delta_n$$ and $$\eta_t$$ are non-increasing, i.e. $$\eta_t\geq \eta_{t+1}$$.

  Otherwise, it can hold if $$(F(x) + \nabla F(x)^\top(y-x)) \leq 0$$ for all $$x,y \in \Delta_n$$ and $$\eta_t$$ are non-decreasing, i.e. $$\eta_t \leq \eta_{t+1}$$
