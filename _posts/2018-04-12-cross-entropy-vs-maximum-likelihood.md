---
layout: post
title:  "Cross entropy loss vs. max likelihood"
date:   2018-04-12 19:45:54
categories: ML
tags: Math Stats
---

* content
{:toc}

Maximizing likelihood function $\Rightarrow$ (because log based e is monotonically increasing) Maximizing **log** likelihood function $\Rightarrow$ (toggles because of the minus sign) Minimizing **negative** log likelihood function $\Rightarrow$ Minimizing cross entropy loss.

## Quick proof of binary classification:  
Assume $y_1^* ... y_N^*$ is a mini-batch of predictions generated (i.i.d) by your neural network. Each of them
represents the probability of getting the correct prediction (i.e. matches the target). Assume the true targets are $t_1 ... t_N$ where each of them is either 0 or 1.

$$ \begin{align}
-log[P(y_1^* ... y_N^*)] & = -\sum_{i=1}^N log[P(y_i^*)] &&\text{Because of the i.i.d assumption.}&\\
\end{align} $$

If the target is 0, $ log[P(y_i^\*)] = log[1-P(y_i)] = (1-t_i) \cdot log[1-P(y_i)]$  
If the target is 1, $ log[P(y_i^\*)] = t_i \cdot log[P(y_i)]$

And it doesn't hurt to add 0 term to make the formula more general,

$$ P(y_i^*) = t_i \cdot log[P(y_i)] + (1-t_i) \cdot log[1-P(y_i)]$$

So,

$$ \begin{align}
-\sum_{i=1}^N log[P(y_i^*)] & = -\sum_{i=1}^N t_i \cdot log[P(y_i)] + (1-t_i) \cdot log[1-P(y_i)]\\
 & = CE
\end{align} $$
