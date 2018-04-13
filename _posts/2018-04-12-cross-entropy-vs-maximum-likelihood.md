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

If the target is 0,

$$ log[P(y_i^*)] = log[1-P(y_i)] = (1-t_i) \cdot log[1-P(y_i)]$$

If the target is 1, 

$$ log[P(y_i^*)] = t_i \cdot log[P(y_i)]$$

And it doesn't hurt to add 0 term to make the formula more general,

$$ P(y_i^*) = t_i \cdot log[P(y_i)] + (1-t_i) \cdot log[1-P(y_i)]$$

So,

$$ \begin{align}
-\sum_{i=1}^N log[P(y_i^*)] & = -\sum_{i=1}^N t_i \cdot log[P(y_i)] + (1-t_i) \cdot log[1-P(y_i)]\\
 & = CE
\end{align} $$





## Code snippet from Autograd
```python
from __future__ import absolute_import
from __future__ import print_function
from builtins import range
import autograd.numpy as np
from autograd import grad
from autograd.test_util import check_grads

def sigmoid(x):
    return 0.5*(np.tanh(x) + 1)

def logistic_predictions(weights, inputs):
    # Outputs probability of a label being true according to logistic model.
    return sigmoid(np.dot(inputs, weights))

def training_loss(weights):
    # Training loss is the negative log-likelihood of the training labels.
    preds = logistic_predictions(weights, inputs)
    label_probabilities = preds * targets + (1 - preds) * (1 - targets)
    return -np.sum(np.log(label_probabilities))

# Build a toy dataset.
inputs = np.array([[0.52, 1.12,  0.77],
                   [0.88, -1.08, 0.15],
                   [0.52, 0.06, -1.30],
                   [0.74, -2.49, 1.39]])
targets = np.array([True, True, False, True])

# Build a function that returns gradients of training loss using autograd.
training_gradient_fun = grad(training_loss)

# Check the gradients numerically, just to be safe.
weights = np.array([0.0, 0.0, 0.0])
check_grads(training_loss, modes=['rev'])(weights)

# Optimize weights using gradient descent.
print("Initial loss:", training_loss(weights))
for i in range(100):
    weights -= training_gradient_fun(weights) * 0.01

print("Trained loss:", training_loss(weights))
```
