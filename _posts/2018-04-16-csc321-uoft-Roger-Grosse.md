---
layout: post
title:  "CSC321严谨的ML入门课程"
date:   2018-04-16 15:31:54
categories: ML
tags: MLP CNN RNN GAN
---

* content
{:toc}

这里有UofT Intro to Neural Networks and Machine Learning课程资料总汇，由$2/3$的Supervised learning以及$1/3$的Unsupervised learning and Reinforcement learning组成。废话不多说，欢迎来到机(ren)器(gong)学(zhi)习(zhang)的世界。





## Instructor
<p align="center">
<img src="http://www.cs.toronto.edu/~rgrosse/photo.png" width="30%"/>
<br> <strong><a href="http://www.cs.toronto.edu/~rgrosse/">Roger Grosse</a></strong> </p>

## Course Webpage (2018)
http://www.cs.toronto.edu/~rgrosse/courses/csc321_2018/

## Lectures
* **Lecture 1: Introduction** [[Slides](/mdres/posts/2018/csc321/lec01.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L01 Introduction.pdf)]  
What are machine learning and neural networks, and what would you use them for? Supervised, unsupervised, and reinforcement learning. How this course is organized.

* **Lecture 2: Linear Regression** [[Slides](/mdres/posts/2018/csc321/lec02.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L02 Linear Regression.pdf)]  
Linear regression, a supervised learning task where you want to predict a scalar valued target. Formulating it as an optimization problem, and solving either directly or with gradient descent. Vectorization. Feature maps and polynomial regression. Generalization: overfitting, underfitting, and validation.

* **Lecture 3: Linear Classification** [[Slides](/mdres/posts/2018/csc321/lec03.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L03 Linear Classifiers.pdf)]  
Binary linear classification. Visualizing linear classifiers. The perceptron algorithm. Limits of linear classifiers.

* **Lecture 4: Learning a Classifier** [[Slides](/mdres/posts/2018/csc321/lec04.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L04 Training a Classifier.pdf)]  
Comparison of loss functions for binary classification. Cross-entropy loss, logistic activation function, and logistic regression. Hinge loss. Multiway classification. Convex loss functions. Gradient checking. (Note: this is really a lecture-and-a-half, and will run into what's scheduled as Lecture 5.)

* **Lecture 5: Multilayer Perceptrons** [[Slides](/mdres/posts/2018/csc321/lec05.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L05 Multilayer Perceptrons.pdf)]  
Multilayer perceptrons. Comparison of activation functions. Viewing deep neural nets as function composition and as feature learning. Limitations of linear networks and universality of nonlinear networks.  
Suggested reading: Deep Learning Book, Sections 6.1-6.4

* **Lecture 6: Backpropagation** [[Slides](/mdres/posts/2018/csc321/lec06.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L06 Backpropagation.pdf)]  
The backpropagation algorithm, a method for computing gradients which we use throughout the course.

* **Lecture 7: Distributed Representations** [[Slides](/mdres/posts/2018/csc321/lec07.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L07 Distributed Representations.pdf)]  
Language modeling, n-gram models (a localist representation), neural language models (a distributed representation), and skip-grams (another distributed representation).

* **Lecture 8: Optimization** [[Slides](/mdres/posts/2018/csc321/lec08.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L08 Optimization.pdf)]  
How to use the gradients computed by backprop. Features of optimization landscapes: local optima, saddle points, plateaux, ravines. Stochastic gradient descent and momentum.  
Suggested reading: Deep Learning Book, Chapter 8

* **Lecture 9: Generalization** [[Slides](/mdres/posts/2018/csc321/lec09.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L09 Generalization.pdf)]  
Bias/variance decomposition, data augmentation, limiting capacity, early stopping, weight decay, ensembles, stochastic regularization, hyperparameter tuning.  
Suggested reading: Deep Learning Book, Chapter 7

* **Lecture 10: Automatic Differentiation** [[Slides](/mdres/posts/2018/csc321/lec10.pdf)]  
How to implement an automatic differentiation system. Based on Autodidact, a pedagogical implementation of Autograd.

* **Lecture 11: Convolutional Networks** [[Slides](/mdres/posts/2018/csc321/lec11.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L11 Convolutional Networks.pdf)]  
Convolution operation. Convolution layers and pooling layers. Equivariance and invariance. Backprop rules for conv nets.

* **Lecture 12: Image Classification** [[Slides](/mdres/posts/2018/csc321/lec12.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L12 Object Recognition.pdf)]  
Conv net architectures applied to handwritten digit and object classification. Measuring the size of a conv net.

* ~~Lecture 13: Cancelled~~

* **Lecture 14: Optimizing the Input** [[Slides1](/mdres/posts/2018/csc321/lec14.pdf)] [[Slides2](/mdres/posts/2018/csc321/lec14b.pdf)]  
Interesting things you can do with gradient descent on the inputs: conv net visualizations, adversarial inputs, Deep Dream.

* **Lecture 15: Recurrent Neural Nets** [[Slides](/mdres/posts/2018/csc321/lec15.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L15 Recurrent Neural Nets.pdf)]  
Recurrent neural nets. Backprop through time. Applying RNNs to language modeling and machine translation.

* **Lecture 16: Learning Long-Term Dependencies** [[Slides](/mdres/posts/2018/csc321/lec16.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L16 Learning Long Term Dependencies.pdf)]  
Why RNN gradients explode and vanish, both in terms of the mechanics of backprop, and conceptually in terms of the function the RNN computes. Ways to deal with it: gradient clipping, input reversal, LSTM.

* **Lecture 17: ResNets and Attention** [[Slides](/mdres/posts/2018/csc321/lec17.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L17 ResNets and Attention.pdf)]  
Deep Residual Networks. Attention-based models for machine translation and caption generation.

* **Lecture 18: Learning Probabilistic Models** [[Slides](/mdres/posts/2018/csc321/lec18.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L18 Learning Probabilistic Models.pdf)]  
Maximum likelihood estimation. Optional: basics of Bayesian parameter estimation and maximum a-posteriori estimation.

* **Lecture 19: Generative Adversarial Networks** [[Slides](/mdres/posts/2018/csc321/lec19.pdf)] [[Lecture Notes](/mdres/posts/2018/csc321/L19 GANs.pdf)]

* **Lecture 20: Autoregressive and Reversible Models** [[Slides](/mdres/posts/2018/csc321/lec20.pdf)]

* **Lecture 21: Policy Gradient** [[Slides](/mdres/posts/2018/csc321/lec21.pdf)]

* **Lecture 22: Q-Learning** [[Slides](/mdres/posts/2018/csc321/lec22.pdf)]

* **Lecture 23: Go** [[Slides](/mdres/posts/2018/csc321/lec23.pdf)]  
Reinforcement learning好的学习资料：[UCL Course on RL taught by David Silver](http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching.html)  
Youtube视频: https://www.youtube.com/watch?v=2pWv7GOvuf0

## Tutorials
* **Tutorial 1: Linear Regression** [[IPython Notebook](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut1.ipynb)]

* **Tutorial 2: Classification** [[IPython Notebook](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut2.ipynb)]

* **Tutorial 3: Backpropagation** [[Slides](/mdres/posts/2018/csc321/tut3_derivation.pdf)] [[IPython Notebook](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut3.ipynb)]

* **Tutorial 4: Autograd** [[IPython Notebook](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut4.ipynb)]

* **Tutorial 5: PyTorch** [IPython Notebooks: [1](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut5a.ipynb), [2](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut5b.ipynb), [3](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut5c.ipynb), [4](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut5d.ipynb)]

* **Tutorial 6: Conv Nets** [[Slides](/mdres/posts/2018/csc321/tut6_slides.pdf)] [IPython Notebooks: [MNIST](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut6_mnist.ipynb), [CIFAR-10](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut6_cifar.ipynb)]

* **Tutorial 7: Midterm Review**

* **Tutorial 8: Attention and Maximum Likelihood** [[IPython Notebook](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut8_complete.ipynb)]

* **Tutorial 9: GANs** [IPython Notebooks: [GAN](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut9_GAN.ipynb), [DCGAN](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut9_DCGAN.ipynb)]

* **Tutorial 10: Policy Gradient** [[Slides](/mdres/posts/2018/csc321/tut10_slides.pdf)] [[IPython Notebook](http://nbviewer.jupyter.org/github/jellycsc/jellycsc.github.io/blob/master/mdres/posts/2018/csc321/tut10_code.ipynb)]

<img src="/mdres/loading.gif" width="20"/>持续更新中。。。
