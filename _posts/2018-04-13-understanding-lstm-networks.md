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

循环神经网络([RNN](https://en.wikipedia.org/wiki/Recurrent_neural_network))解决了这个问题，他们在自身的网络中拥有循环结构，使得不同时间点的推理信息得以保存传递下来。

<p align="center">
<img src="/mdres/posts/2018/lstm/RNN-rolled.png" width="20%"/> <br>
<strong>循环看到了吗？</strong> </p>

<img src="/mdres/loading.gif" width="20"/>持续更新中。。。
