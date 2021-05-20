---
layout: post
title: 2-Step Follow the Regularized Leader
published: true
project: false
---

We have seen 3 online learning algorithms: 1-step OMD, 2-step OMD and FTRL. Usually FTRL is written and studied as a single step update. However, it is possible to define and analyze a 2-step version of FTRL as well.

The FTRL update is written as:

$$\begin{align*}
x_1 &= \arg\min_{x \in \Delta_n} F(x)\\
x_{t+1} &= \arg\min_{x \in \Delta_n} \left[\eta_t \sum_{s=1}^t l_s^\top x + F(x)\right]
\end{align*}$$

Using the Mixed Bregman $$\text{Breg}_{a,b}(x\|y) = \frac{F(x)}{a} - \frac{F(y)}{b} - \frac{\nabla F(y)}{b}^\top(x-y)$$, we can write FTRL in a way similar to 1-step OMD.

$$\begin{align*}
x_1 &= \arg\min_{x \in \Delta_n} F(x)\\
x_{t+1} &= \arg\min_{x \in \Delta_n} \left[ l_t^\top x + \text{Breg}_{\eta_t,\eta_{t-1}}(x\|x_t)\right]
\end{align*}$$

So, we can write a 2-step FTRL similar to 2-step OMD:

$$\begin{align*}
x_1 &= \arg\min_{x \in \Delta_n} F(x)\\
\tilde{x}_{t+1} &= \arg\min_{x \in \mathbb{R}^n} \left[ l_t^\top x + \text{Breg}_{\eta_t,\eta_{t-1}}(x\|x_t)\right]\\
x_{t+1} &= \arg\min_{x \in \Delta_n} \text{Breg}(x\|\tilde{x}_{t+1})
\end{align*}$$

Here is a summary of the four algorithms:

|Alg | 1-step  |2-step  |
|--|---|--|
|OMD |$$x_{t+1} = \arg\min_{x \in \Delta_n} \left[ l_t^\top x + \text{Breg}_{\eta_t,\eta_{t}}(x\|x_t)\right]$$  |  $$\tilde{x}_{t+1} = \arg\min_{x \in \mathbb{R}^n} \left[ l_t^\top x + \text{Breg}_{\eta_t,\eta_{t}}(x\|x_t)\right]$$$$x_{t+1} = \arg\min_{x \in \Delta_n} \text{Breg}(x\|\tilde{x}_{t+1})$$|
|FTRL| $$x_{t+1} = \arg\min_{x \in \Delta_n} \left[ l_t^\top x + \text{Breg}_{\eta_t,\eta_{t-1}}(x\|x_t)\right]$$  |  $$\tilde{x}_{t+1} = \arg\min_{x \in \mathbb{R}^n} \left[ l_t^\top x + \text{Breg}_{\eta_t,\eta_{t-1}}(x\|x_t)\right]$$$$x_{t+1} = \arg\min_{x \in \Delta_n} \text{Breg}(x\|\tilde{x}_{t+1})$$|

In terms of potential functions, they can be written as:

|Alg | 1-step  |2-step |
|--|---|--|
|OMD |$$\theta_t = \theta_{t-1} - \eta_t l_t$$$$x_{t+1} = \psi(\theta_t + \lambda(\theta_t))$$  |  $$\theta_t = \theta_{t-1} - \eta_t l_t$$$$\tilde{x}_{t+1} = \psi(\theta_t + \lambda(\theta_{t-1}))$$$$x_{t+1} = \psi(\theta_t + \lambda(\theta_{t}))$$|
|FTRL| $$\theta_t = \frac{\eta_t}{\eta_{t-1}}\theta_{t-1} - \eta_t l_t$$$$x_{t+1} = \psi(\theta_t + \lambda(\theta_t))$$  |  $$\theta_t = \frac{\eta_t}{\eta_{t-1}}\theta_{t-1} - \eta_t l_t$$$$\tilde{x}_{t+1} = \psi\left(\theta_t + \frac{\eta_t}{\eta_{t-1}}\lambda(\theta_{t-1})\right)$$$$x_{t+1} = \psi(\theta_t + \lambda(\theta_{t}))$$|

### Regret

In previous posts, we have analyzed 1-step OMD, 2-step OMD and 1-step FTRL. Here, we analyze 2-step FTRL

For any $$x \in \Delta_n$$, we have:

$$\begin{align*}
l_t^\top(x_t-x) &= l_t^\top(\tilde{x}_{t+1}-x) + l_t^\top(x_t-\tilde{x}_{t+1})\\
&= \left(\frac{\theta_{t-1}}{\eta_{t-1}}-\frac{\theta_t}{\eta_t}\right)^\top(\tilde{x}_{t+1}-x) + l_t^\top(x_t-\tilde{x}_{t+1})\\
&= \left(\frac{\nabla F(x_t)-\lambda(\theta_{t-1})}{\eta_{t-1}} - \frac{\nabla F(\tilde{x}_{t+1})-\frac{\eta_t}{\eta_{t-1}}\lambda(\theta_{t-1})}{\eta_t}\right)^\top(\tilde{x}_{t+1}-x) + l_t^\top(x_t-\tilde{x}_{t+1})\\
&= \left(\frac{\nabla F(x_t)}{\eta_{t-1}} - \frac{\nabla F(\tilde{x}_{t+1})}{\eta_t}\right)^\top(\tilde{x}_{t+1}-x) + l_t^\top(x_t-\tilde{x}_{t+1})
\end{align*}$$

Using the Mixed Bregman, we can write the first term as:

The first term can be written as:

$$\begin{align*}
\left(\frac{\nabla F(x_t)}{\eta_{t-1}} - \frac{\nabla F(\tilde{x}_{t+1})}{\eta_t}\right)^\top(\tilde{x}_{t+1}-x)  &= \text{Breg}_{\alpha,\eta_{t-1}}(x\|x_t) - \text{Breg}_{\alpha,\eta_{t}}(x\|\tilde{x}_{t+1})- \text{Breg}_{\eta_t,\eta_{t-1}}(\tilde{x}_{t+1}\|x_t)\\
&\leq  \text{Breg}_{\alpha,\eta_{t-1}}(x\|x_t) - \text{Breg}_{\alpha,\eta_{t}}(x\|x_{t+1})- \text{Breg}_{\eta_t,\eta_{t-1}}(\tilde{x}_{t+1}\|x_t)\\
\end{align*}$$

$$\alpha$$ could be any non-zero number in the above expression.

Note that $$\text{Breg}_{\alpha,\eta_{t}}(x\|x_{t+1}) \leq \text{Breg}_{\alpha,\eta_{t}}(x\|\tilde{x}_{t+1})$$ for all $$x \in \Delta_n$$.

$$\begin{align*}
 \eta_t(\text{Breg}_{\alpha,\eta_{t}}(x\|\tilde{x}_{t+1}) - \text{Breg}_{\alpha,\eta_{t}}(x\|x_{t+1}))  &= F(x_{t+1}) - F(\tilde{x}_{t+1}) - \nabla F(\tilde{x}_{t+1})^\top (x-\tilde{x}_{t+1}) + \nabla F(x_{t+1})^\top (x-x_{t+1}) \\
 &= \text{Breg}(x_{t+1}\| \tilde{x}_{t+1}) + ( \nabla F(x_{t+1}) - \nabla F(\tilde{x}_{t+1}))^\top (x-x_{t+1}) \\
 &\geq  ( \nabla F(x_{t+1}) - \nabla F(\tilde{x}_{t+1}))^\top (x-x_{t+1})
\end{align*}$$

Since $$x_{t+1}$$ is the minimizer of $$\text{Breg}(x\| \tilde{x}_{t+1})$$ on $$x \in \Delta_n$$, we have the optimality condition:

$$(\nabla F(x_{t+1} ) - \nabla F(\tilde{x}_{t+1}))^\top (x-x_{t+1}) \geq 0$$

Taking summation over $$t$$, we have:

$$\begin{align*}
\sum_{t=1}^T l_t^\top(x_t-x) &= \sum_{t=1}^T \left[\text{Breg}_{\alpha,\eta_{t-1}}(x\|x_t) - \text{Breg}_{\alpha,\eta_{t}}(x\|x_{t+1})\right] + \sum_{t=1}^T \left[ l_t^\top(x_t-\tilde{x}_{t+1}) -  \text{Breg}_{\eta_t,\eta_{t-1}}(\tilde{x}_{t+1}\|x_t)\right]\\
&=\text{Breg}_{\alpha,\eta_{0}}(x\|x_1) - \text{Breg}_{\alpha,\eta_{T}}(x\|x_{T+1}) + \sum_{t=1}^T \left[ l_t^\top(x_t-\tilde{x}_{t+1}) -  \text{Breg}_{\eta_t,\eta_{t-1}}(\tilde{x}_{t+1}\|x_t)\right]
\end{align*}$$

Let $$\alpha = \eta_0 = \eta_T$$. The Mixed Bregmans in the first term will become normal Bregmans.

$$\begin{align*}
\sum_{t=1}^T l_t^\top(x_t-x) &=\frac{1}{\eta_T}\text{Breg}(x\|x_1) - \text{Breg}(x\|x_{T+1}) + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) -  \text{Breg}_{\eta_t,\eta_{t-1}}(x_{t+1}\|x_t)\right]\\
&\leq \frac{1}{\eta_T}\text{Breg}(x\|x_1) + \sum_{t=1}^T \left[ l_t^\top(x_t-\tilde{x}_{t+1}) -  \text{Breg}_{\eta_t,\eta_{t-1}}(\tilde{x}_{t+1}\|x_t)\right]\\
\end{align*}$$

Inorder to get specific results, we proceed in the same fashion as 1-step FTRL. For the three cases, we can get similar results with $$x_{t+1}$$ replaced by $$\tilde{x}_{t+1}$$ in the second summation.

Here is a summary of the regret inequalitis:

|Algorithm | Regret |
|---|---|
|1-step OMD | $$\sum_{t=1}^T \frac{1}{\eta_t}\left[ \text{Breg}(x\|x_{t}) - \text{Breg}(x\|x_{t+1}) \right] + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) - \frac{1}{\eta_t}\text{Breg}(x_{t+1}\|x_{t}) \right]$$  |
|2-step OMD|    $$\sum_{t=1}^T \frac{1}{\eta_t}\left[ \text{Breg}(x\|x_{t}) - \text{Breg}(x\|x_{t+1}) \right] + \sum_{t=1}^T \left[ l_t^\top(x_t-\tilde{x}_{t+1}) - \frac{1}{\eta_t}\text{Breg}(\tilde{x}_{t+1}\|x_{t}) \right]$$|
|1-step  FTRL | $$\frac{1}{\eta_T}\text{Breg}(x\|x_1) + \sum_{t=1}^T \left[ l_t^\top(x_t-x_{t+1}) -  \text{Breg}_{\eta_t,\eta_{t-1}}(x_{t+1}\|x_t)\right]$$|
|2-step FTRL  |  $$\frac{1}{\eta_T}\text{Breg}(x\|x_1) + \sum_{t=1}^T \left[ l_t^\top(x_t-\tilde{x}_{t+1}) -  \text{Breg}_{\eta_t,\eta_{t-1}}(\tilde{x}_{t+1}\|x_t)\right]$$|
