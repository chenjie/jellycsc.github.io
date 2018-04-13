---
layout: post
title:  "深入理解LSTM记忆元网络"
date:   2018-04-13 17:00:00
categories: ML
tags: RNN
---

* content
{:toc}

本文翻译源自[Google Brain](https://ai.google/brain-team/)机器学习研究员[colah](https://github.com/colah/)的[博客](http://colah.github.io/)，原文链接请点击[这里](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)。





## 循环神经网络
人们每一秒思考都是连续的，而并非从零开始。当你在读这篇文章的时候，你通过先前的词句来理解当前词语的含义。你读到一半不会扔掉前面所有获取到的文章信息而从头开始思考，这说明了你的思维是连贯的。

传统的神经网络正因为不能做到这个，所以这也似乎是它的缺陷。举个例子，假如你想要用神经网络来将每一个电影的时间点所发生的事件分类，你不清楚传统的神经网络是如何将它对于之前事件的推理传递到之后的事件中去。

循环神经网络([RNN](https://en.wikipedia.org/wiki/Recurrent_neural_network))解决了这个问题，他们在自身的网络中拥有循环结构，使得不同时间点(timestamp)的推理信息得以保存传递下来。

<p align="center">
<img src="/mdres/posts/2018/lstm/RNN-rolled.png" width="20%"/> <br>
<strong>带有循环结构的RNN模型</strong> </p>

在上图中，$A$代表任何神经网络(CNN, MLP...)，$X_t$代表在$t$时间点的输入，$h_t$代表在$t$时间点的输出。图中的循环结构让信息能够从RNN的一个时间点传递到之后的时间点去。

这些循环结构让RNN看上去有些神秘，但是如果你再仔细思考一下，就会发现其实RNN和普通的神经网络(CNN, MLP...)没有太大差别。RNN可以被看作是一个神经网络的多个镜像，每一个镜像为它连接的下一个镜像传递信息。设想一下RNN的循环结构展开是什么样子的？

<p align="center"> <br>
<img src="/mdres/posts/2018/lstm/RNN-unrolled.png" width="90%"/> <br>
<strong>循环结构被展开的RNN模型</strong> </p>

RNN链式结构的本质展现了它和序列(sequences & lists)是紧密相连的，所以它自然就是序列这种数据类型最合适的神经网络架构模型。

RNN其实就在我们身边，一直在被人们所使用着。近年来(2015)，RNN的应用在以下问题领域中取得了突出成就：语音识别(speech recognition)，语言建模(language modeling)，翻译(translation)，看图说话/图像标注(image captioning)等等，这个列表还在一直延续下去。在[Andrej Karpathy](https://cs.stanford.edu/people/karpathy/)的[这篇出色的博客](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)中论述了RNN非凡的功绩。

RNN成功的关键就是LSTM(Long short-term memory)记忆元“们”，不计其数的LSTM记忆元组成了一个十分特别的循环神经网络，我(译者)将它称作为LSTM记忆元网络，它在很多任务中的表现比普通RNN更加强大，几乎所有令人瞩目的RNN成就的功臣都是它。接下来我们就对LSTM记忆元展开进一步探索。



<img src="/mdres/loading.gif" width="20"/>持续更新中。。。
