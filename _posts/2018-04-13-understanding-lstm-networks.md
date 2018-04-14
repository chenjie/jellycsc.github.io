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

此文是本人翻译的处男作，耗时一天，若有翻译错误或不当的地方，欢迎在评论区指正(可能需要科学上网)。另外，引用或转载请注明本文(https://jellycsc.github.io/2018/04/13/understanding-lstm-networks/)以及原文(http://colah.github.io/posts/2015-08-Understanding-LSTMs/)，谢谢！





## RNN循环神经网络
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

RNN成功的关键就是LSTM(Long short-term memory)记忆元“们”，不计其数的LSTM记忆元组成了一个十分特别的循环神经网络，我(译者)将它称作为LSTM记忆元网络，它在很多任务中的表现比标准RNN更加强大，几乎所有令人瞩目的RNN成就的功臣都是它。接下来我们就对LSTM记忆元展开进一步探索。

## 长期(Long-Term)依赖的问题
RNN的吸引力之一在于这样一种将之前的信息用于当前判断/抉择的想法，就像视频是连贯的，先前的视频帧对于理解当前的视频画面有很大的帮助和提示。如果RNN也能做到这样，它们会变得非常有用。但是它们真的可以做到吗？还不一定。

有时候我们只需要近(短)期的信息来完成当前的任务，就像在语言模型中预测下一个词语基于先前的一些词语。如果我们要尝试预测`The clouds are in the __.`这句话的最后一个词，我们不需要任何更多的上下文信息，答案很明显就应该是`sky`(因为云绝大多数情况下在天上)。在这种，答案就藏在前后几个词的情况下，标准RNN也能够学习训练用先前信息作出正确判断。

<p align="center">
<img src="/mdres/posts/2018/lstm/RNN-shorttermdepdencies.png" width="70%"/> <br>
<strong>短期(Short-Term)依赖的RNN模型</strong> </p>

但是绝大多数情况下答案不会藏在前后几个词中，我们需要更多的上下文信息来帮助作出准确的预测。这次如果我们要尝试预测`I grew up in France ..(此处省略一段话).. I speak fluent __.`这段话的最后一个词，首先我们可以通过前后词的信息来确定这应该是一门语言的名字，但是如果要具体到究竟是哪一门语言，我们就需要上文中(一段话之前的)`France`的信息。在我们日常的语言中，完全有可能出现像这样的相关信息`I grew up in France`，离预测位置`I speak fluent __.`很远的情况。

很遗憾的是，当这个距离拉开的越来越大，标准RNN很难再通过训练学习来连接`相关信息`和`预测位置`的关系，从而就鲜有令人满意的预测了。

<p align="center">
<img src="/mdres/posts/2018/lstm/RNN-longtermdependencies.png" width="90%"/> <br>
<strong>长期(Long-Term)依赖的RNN模型</strong> </p>

从理论上来说，标准RNN完全有能力处理长期依赖关系，但是现实中为了解决简单的问题，也需要人工精确地调整其参数，而RNN自己似乎无法将那些参数学习到。这个问题在以下两篇论文中都有深入的探讨：[Hochreiter (1991) [German]](/mdres/posts/2018/lstm/SeppHochreiter1991ThesisAdvisorSchmidhuber.pdf)，[Bengio, et al. (1994)](/mdres/posts/2018/lstm/tnn-94-gradient.pdf) 他们找到了一些关于这个问题如此困难的根本原因。

幸好，LSTM没有这个问题。

## LSTM记忆元网络
LSTM记忆元网络(通常简称为`LSTMs`)是一种特别的RNN模型，它擅长捕捉学习长期的依赖关系。它是由[Hochreiter & Schmidhuber (1997)](/mdres/posts/2018/lstm/2604.pdf)提出的，并且在接下来的工作中被许多人提炼和推广<sup>1</sup>。LSTM记忆元结构在各种各样的问题上都获得了显著的成效，现在被广泛使用。

LSTM设计之初是用来避免长期依赖问题的，长期地记忆信息是LSTM记忆元的缺省模式，并不是它们要刻意去学习的。

所有RNN都具有神经网络的重复链式模组结构(重复代表它展开后每一个timestamp的结构都一模一样，链式代表它们之间相互连接传递信息，模组代表前面说的每一个timestamp的结构)，在标准的RNN模型中，这个重复的模组式结构具有非常简单的形态，比如只有一个$tanh$激活函数层：

<p align="center"> <br>
<img src="/mdres/posts/2018/lstm/LSTM3-SimpleRNN.png" width="90%"/> <br>
<strong>标准RNN重复模组式结构中的$tanh$激活层</strong> </p>

LSTM记忆元网络也有与标准RNN相似的链式结构，但是重复的模组式结构有不同的形态。它不是单单一层$tanh$，而是拥有四(4)个元素以特殊逻辑形式交互的复杂层。[译者注：这里参考了[lorderYu](https://blog.csdn.net/yujianmin1990)在其[CSDN博客](https://blog.csdn.net/yujianmin1990/article/details/78826506)中的翻译，用词非常精准，感谢！]

<p align="center"> <br>
<img src="/mdres/posts/2018/lstm/LSTM3-chain.png" width="90%"/> <br>
<strong>一个LSTM记忆元是包括四(4)个元素以特殊逻辑形式交互的复杂层</strong> </p>

不用担心图中的细节都表示什么，我们会一步一步讲解上图LSTM的含义。现在让我们先认识下需要使用的一些符号：

<p align="center">
<img src="/mdres/posts/2018/lstm/LSTM2-notation.png" width="80%"/> <br> </p>

在上图中，每条线都代表一个多维向量，从一个节点的输出端流向其他节点的输入端。粉色的圆圈代表逐点计算(element-wise or point-wise operation)，比如向量的加法。

$$[译者注：(x_1, x_2) + (y_1, y_2) = (x_1 + y_1, x_2 + y_2)]$$  

黄色的矩形代表神经网络层。箭头汇合代表向量合并为多维矩阵，箭头分支表示数据流复制并流向不同路径。

## LSTM记忆元的核心思想
LSTM记忆元网络的关键是在每一个timestamp时LSTM记忆元的状态，也就是下图那条黑色加粗水平线。

LSTM记忆元的状态有点像传送带[译者注：更加确切来说是一条工厂的流水线]，它将上一个timestamp时候的记忆元信息直接传递至下一个，期间仅作一些线性操作来对它加工处理[译者注：线性操作是指对原有LSTM记忆元信息改动不大，复杂度是线性的]。这对当前的信息而言，可以非常容易地“流”过去让原有信息保持不变。[译者注：在RNN中，信息能否保持同一性(identity)是非常重要的，同一性也就意味着在反向传播时，梯度不会消失或激增]

<p align="center">
<img src="/mdres/posts/2018/lstm/LSTM3-C-line.png" width="100%"/> <br> </p>

LSTM记忆元的强大之处在于它能选择性删除或保留状态信息，这个过程是由一个叫“门(gate)”的结构来仔细控制的。

门可以有选择性地让信息通过，最后的结果是由$Sigmoid$激活函数神经层紧接着逐点乘积运算两步构成的。

<p align="center">
<img src="/mdres/posts/2018/lstm/LSTM3-gate.png" width="20%"/> <br> </p>

$Sigmoid$激活函数输出值在0到1之间，描述了信息能够通过的比例。0代表**没有**信息通过，1代表信息**完整地**通过。一个LSTM记忆元有三(3)个这种门，目的是为了保护和控制记忆元的状态。

## 一步一步了解LSTM记忆元
LSTM记忆元的第一步是要决定从现有记忆元状态中扔掉什么信息，这一决定是由以$Sigmoid$激活函数层组成的“忘记门”(forget gate)来作出的，它的输入是$h_{t−1}$和$x_t$，输出是与记忆元状态向量$C_{t−1}$同维度的向量，向量中的每一个数字都介于0到1之间，再重复一下，1代表保留所有信息，0代表完全清除/忘却信息。[译者注：例如$f_t=(0.7,0.5,0.9)$]

就举个之前语言模型的例子，我们的任务是基于之前的词语预测下一个词语。在这样的问题中，记忆元状态可能包含当前主语的性别[译者注：比如在德语中，每个名词都是有性别的，分为阴性feminine、阳性masculine、中性neuter]，为的是在后文中能正确使用代词[译者注：比如在德语中，指代主格，阴性为die、阳性为der、中性为das]。当我们看到一个新的主语时，我们就可以忘掉之前那个主语的性别(gender)信息了。

<p align="center">
<img src="/mdres/posts/2018/lstm/LSTM3-focus-f.png" width="90%"/> <br> </p>

下一步我们要决定储存哪些新的信息到记忆元状态中去。这个分为两部分，第一，由$Sigmoid$激活函数层组成的被称为是“输入门”(input gate)的东西决定哪些向量中的维度将要被更新；第二，由$tanh$激活函数层生成新的，可以被加入到记忆元状态中的候选向量$\tilde{C}_t$。然后，将这两个向量合并作逐点乘积，生成记忆元状态更新向量。

继续之前语言模型的例子，我们会将新主语的性别(gender)信息增加[译者注：做逐点加法$+$]到记忆元状态中，以替换之前忘记的上一个主语的性别(gender)信息。

<p align="center">
<img src="/mdres/posts/2018/lstm/LSTM3-focus-i.png" width="90%"/> <br> </p>

-------------------↓未完待续↓-------------------

现在，将旧的cell状态Ct−1更细为Ct，上面几步已经给出了要做的事情。

首先，将旧cell状态乘以ft，忘掉我们决定要遗忘掉的内容；然后，叠加it∗Ct~，那是我们决定要追加的更新内容。
在语言预测模型的例子中，上述动作对应实际要丢弃的关于旧主题的性别信息，并增加新的信息。　

<p align="center">
<img src="/mdres/posts/2018/lstm/LSTM3-focus-C.png" width="90%"/> <br> </p>

最后，我们需要决定输出什么，这个输出基于cell状态的，但是个过滤后的版本。先用sigmoid（“output gate”）决定cell状态的哪些部分是将要输出的，以及对应的输出比例；再用tanh将cell状态投射到[-1,1]之间；再将两者乘积得到我们想要输出的结果。

对语言预测例子而言，当LSTM刚看到一个主题，它想输出动词相关的信息时，比如，它可能输出单数/复数形式的主题，所以需要知道什么类型的动词的格式应该被对应地加入后续要发生的动作中。　　

<p align="center">
<img src="/mdres/posts/2018/lstm/LSTM3-focus-o.png" width="90%"/> <br> </p>

## LSTM记忆元的不同版本
What I’ve described so far is a pretty normal LSTM. But not all LSTMs are the same as the above. In fact, it seems like almost every paper involving LSTMs uses a slightly different version. The differences are minor, but it’s worth mentioning some of them.

One popular LSTM variant, introduced by Gers & Schmidhuber (2000), is adding “peephole connections.” This means that we let the gate layers look at the cell state.

<p align="center">
<img src="/mdres/posts/2018/lstm/LSTM3-var-peepholes.png" width="90%"/> <br> </p>

The above diagram adds peepholes to all the gates, but many papers will give some peepholes and not others.

Another variation is to use coupled forget and input gates. Instead of separately deciding what to forget and what we should add new information to, we make those decisions together. We only forget when we’re going to input something in its place. We only input new values to the state when we forget something older.

<p align="center">
<img src="/mdres/posts/2018/lstm/LSTM3-var-tied.png" width="90%"/> <br> </p>

A slightly more dramatic variation on the LSTM is the Gated Recurrent Unit, or GRU, introduced by Cho, et al. (2014). It combines the forget and input gates into a single “update gate.” It also merges the cell state and hidden state, and makes some other changes. The resulting model is simpler than standard LSTM models, and has been growing increasingly popular.

<p align="center">
<img src="/mdres/posts/2018/lstm/LSTM3-var-GRU.png" width="90%"/> <br> </p>

These are only a few of the most notable LSTM variants. There are lots of others, like Depth Gated RNNs by Yao, et al. (2015). There’s also some completely different approach to tackling long-term dependencies, like Clockwork RNNs by Koutnik, et al. (2014).

Which of these variants is best? Do the differences matter? Greff, et al. (2015) do a nice comparison of popular variants, finding that they’re all about the same. Jozefowicz, et al. (2015) tested more than ten thousand RNN architectures, finding some that worked better than LSTMs on certain tasks.

-------------------↑未完待续↑-------------------

## 总结
在本文开始的时候，我提到了人们用RNN实现的显著成果，基本上所有这些都是使用LSTM实现的。对于大多数任务来说，LSTM比标准RNN确实更好！

上文中的一组组等式让LSTM看起来着实吓人，但希望你们在阅读完这篇文章后中，能对它有更加深刻的了解，觉得它们更加平易近人。

LSTMs是RNN成就的重大迈进，你可能很自然地就会问：还会有更大一步吗？研究人员们普遍认为：“会有的！下一步就是注意力(attention)机制！“这个想法是让RNN的每一步都从更多的信息中自己挑选信息。例如，如果你使用RNN来创建描述图像的标注，那么它可能会选取图像的一部分来查看并决定其输出的每个单词。事实上，这篇论文[Xu, et al. (2015)](/mdres/posts/2018/lstm/1502.03044v2.pdf) 在做的就是这个 - 如果你想探索注意力机制，这或许是一个很好的起点！很多令人兴奋的结果都使用了注意力机制，而且似乎还有更多……

注意力并不是RNN研究中唯一令人兴奋的线索，例如，这篇[Kalchbrenner, et al. (2015)](/mdres/posts/2018/lstm/1507.01526v1)论文中的的Grid LSTMs似乎非常有前途。在生成模型中使用RNN的论文 - [Gregor, et al. (2015)](/mdres/posts/2018/lstm/1502.04623), [Chung, et al. (2015)](/mdres/posts/2018/lstm/1506.02216v3), or [Bayer & Osendorfer (2015)](/mdres/posts/2018/lstm/1411.7610v3) - 也都非常有趣。过去的几年(2015)对于循环神经网络来说是一个激动人心的时刻，而未来的那些只会更加令人瞩目！

## 鸣谢
我很感谢许多人，他们帮助我更好地理解LSTM，对上文中的可视化图片作出评论，并给予反馈。

我非常感谢我Google的同事提供的非常有帮助的反馈，尤其是[Oriol Vinyals](http://research.google.com/pubs/OriolVinyals.html)，[Greg Corrado](http://research.google.com/pubs/GregCorrado.html)，[Jon Shlens](http://research.google.com/pubs/JonathonShlens.html)，[Luke Vilnis](http://people.cs.umass.edu/~luke/)以及[Ilya Sutskever](http://www.cs.toronto.edu/~ilya/)。我也很感谢其他朋友和同事抽出他们的时间来帮助我，包括[Dario Amodei](https://www.linkedin.com/pub/dario-amodei/4/493/393)和[Jacob Steinhardt](http://cs.stanford.edu/~jsteinhardt/)。我特别感谢[Kyunghyun Cho](http://www.kyunghyuncho.me/)对我的图表极其缜密的交流和沟通。

在这发布篇文章之前，我曾两次在神经网络教学系列讲座中练习解释LSTM。感谢所有参与这些对我很耐心的人，并感谢他们的反馈。

## 注释
1. 除了原作者以外，还有很多人为现代LSTM做出了贡献。以下是一份不完全名单：Felix Gers, Fred Cummins, Santiago Fernandez, Justin Bayer, Daan Wierstra, Julian Togelius, Faustino Gomez, Matteo Gagliolo, and [Alex Graves](https://scholar.google.com/citations?user=DaFHynwAAAAJ&hl=en).

<img src="/mdres/loading.gif" width="20"/>持续更新中。。。
