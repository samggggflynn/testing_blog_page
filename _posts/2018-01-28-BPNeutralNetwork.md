---
layout: post
title: 神经网络入门
category: 技术
tags: [Machine-Learning]
description: 
---

>神经网络是机器学习的入门必备，本文将带领大家一起进入机器学习的世界。

**神经元**

　　神经元（Neuron）是构成神经网络的基本单元。人工神经元和感知器非常类似，也是模拟生物神经元特性，接受一组输入信号并产出输出。生物神经元
有一个阀值，当神经元所获得的输入信号的积累效果超过阀值时，它就处于兴奋状态；否则，应该处于抑制状态。

　　人工神经元使用一个非线性的激活函数，输出一个活性值。假定神经元接受n个输入x=(x1,x2,...,xn),用状态z表示一个神经元所获得的输入信号x的
加权和，输出为该神经元的活性值a。具体定于如下:

<p align="center">
    <img src="/assets/img/BPNetwork/equation1.png">
</p>

　　其中，w是n维的权重向量，b 是偏置。典型的激活函数f 有sigmoid型函数、非线性斜面函数等。如果我们设激活函数f为0或1的阶跃函数，人工神经元就是感知器。

**sigmoid 函数**

　　sigmoid 型函数是指一类S 型曲线函数，常用的sigmoid 型函数有logistic 函数$$\sigma(x)$$ 和tanh(x) 函数。

<p align="center">
    <img src="/assets/img/BPNetwork/equation2.png">
</p>

　　logistic函数对应的函数曲线如下图所示：

<p align="center">
    <img src="/assets/img/blogimg/sigmoid.png">
</p>

　　tanh函数以看作是放大并平移的logistic函数$tanh(x) = \frac{2}{sigma(2x)}-1$

　　sigmoid 型函数对中间区域的信号有增益，对两侧区的信号有抑制。这样的特点也和生物神经元类似，对一些输入有兴奋作用，另一些输入（两侧区）
有抑制作用。和感知器的阶跃激活函数(−1/1, 0/1) 相比，sigmoid 型函数更符合生物神经元的特性，同时也有更好的数学性质。

　　每一个神经元的模型如下所示

<p align="center">
    <img src="/assets/img/blogimg/Perceptron.png">
</p>

**前馈神经网络**

　　给定一组神经元，我们可以以神经元为节点来构建一个网络。不同的神经网络模型有着不同网络连接的拓扑结构。一种比较直接的拓扑结构是前馈网络。
前馈神经网络（Feed-forward Neural Network）是最早发明的简单人工神经网络。各神经元分别属于不同的层。每一层的神经元可以接收前一层神经元
的信号，并产生信号输出到下一层。第一层叫**输入层**，最后一层叫**输出层**，其它中间层叫做**隐藏层**。整个网络中无反馈，信号从输入层向输出
层单向传播，可用一个有向无环图表示。一个前馈神经网络如下图所示：

<p align="center">
    <img src="/assets/img/blogimg/FeedForwardNeuralNetwork.png">
</p>

给定一个前馈神经网络，我们用下面的记号来描述这个网络。

前馈神经网络通过下面公式进行信息传播：

<p align="center">
    <img src="/assets/img/BPNetwork/equation3.png">
</p>

这样，前馈神经网络可以通过逐层的信息传递，得到网络最后的输出a^L

<p align="center">
    <img src="/assets/img/BPNetwork/equation4.png">
</p>

谢谢观看，希望对您有所帮助，欢迎指正错误，欢迎一起讨论！！！








　　




　　






  



