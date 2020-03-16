---
layout: post
title: 贝叶斯篇之参数估计
category: 技术
tags: [Math,Bayesian]
description: 
---

> 在将贝叶斯决策和朴素贝叶斯完美结合之前，还有一些数学基础需要打扎实，那就是概率论中较为重要的参数估计。为了给贝叶斯理论铺路，
数学上的知识是避免不了的，本博文只对参数估计中的极大似然估计和极大后验概率估计，主要是对以后理解朴素贝叶斯公式打基础。

　　无论是贝叶斯学派还是频率学派，一个无法避开的问题就是如何从已有的样本中获取信息并据此估计目标模型的参数。比较有名的“频率近似概率”其实就是
（基于大数定律的）相当合理的估计之一，本博文所叙述的两种参数估计方法在最后也通常会归结于它。参数估计可以帮助我们在已知一堆数据是从某一种分布中随机取出来的，
可是我们并不知道这个分布具体的参，即“模型已定，参数未知”的情况下找出一组参数，使得模型产生出观测数据的概率最大这是极大似然估计的方法。
但是一些情况下参数的估计是对模型参数从样本出发去后验参数概率让得到的参数概率值可能性更加的合理，这就是贝叶斯的思维方式，尽量用已有的去获得更好的评估，
这个方法就是极大后验概率估计的思路。我们接下来从严谨的数学角度去分析这两中参数估计。

# 极大似然估计（ML 估计） #

　　如果把模型描述成一个概率模型的话，一个自然的想法是希望得到的模型参数$$\theta$$能够使得在训练集$$\tilde X$$作为输入时、模型输出的概率达到极大。
这个最大概率当然是我们希望得到的，比如对于模型预测、分类上是意义很大的。这里就有一个似然函数的概念，它能够输出$$\tilde{X} = \left( x_{1},\ldots,x_{N} \right)^{T}$$
在模型参数为$$\theta$$下的概率：

$$p\left( \tilde{X} \middle \vert \theta \right) = \prod_{i = 1}^{N}{p(x_{i} \vert \theta)}$$

　　我们希望找到的$$\hat{\theta}$$就是使得似然函数在$$\tilde X$$作为输入时达到极大的参数：

$$\hat{\theta} = \arg{\max_\theta{p\left( \tilde{X} \middle \vert \theta \right) = \arg{\max_\theta{\prod_{i = 1}^{N}{p(x_{i} \vert \theta)}}}}}$$

　　举一个抛硬币的简单例子。 现在有一个正反面不是很匀称的硬币，如果正面朝上记为H，方面朝上记为T，抛10次的结果如下：

<p align="center">
    T,T,H,T,T,T,H,T,T,T,T
</p>

　　从结果上可以很明显的知道这10次抛掷硬币正面朝上的概率为0.2，我们试着用极大似然估计的方法去求解这个硬币正面向上的概率。

　　在这个问题中，模型的参数$$\theta$$可以设为硬币正面向上的概率，样本$$x_{i}$$可以描述为第$$i$$次硬币是否是正面向上；如果是就取 1、否则取 0。
这样的话，似然函数就可以描述为：

$$p\left( \tilde{X} \middle \vert \theta \right) = \prod_{i = 1}^{N}{p(x_{i} \vert \theta)} = \prod_{i = 1}^{N}{\theta ^{x_i} (1-\theta)^{1 - x_i}}$$

　　直接对它求极大值（虽然可行但是）不太方便，通常的做法是将似然函数取对数之后再进行极大值的求解：

$$
\begin{eqnarray}
\ln{p\left( \tilde{X} \middle \vert \theta \right)} &=& \prod_{i = 1}^{N}\ln ({\theta ^{x_i} (1-\theta)^{1 - x_i}}) \\
&=& \sum_{i=1}^{N}(x_{i}\ln \theta + (1- x_{i}) \ln (1 - \theta)) \\
&=& \sum_{i=1}^{N}(x_{i}\ln \theta) + \sum_{i=1}^{N}(1- x_{i}) \ln (1 - \theta)
\end{eqnarray}
$$

　　对参数$$\theta$$求偏导：

$$
\begin{eqnarray}
\frac{\partial }{\partial \theta }\ln{p\left( \tilde{X} \middle \vert \theta \right)} &=& 
\frac{\partial }{\partial \theta }\lbrack \sum_{i=1}^{N}(x_{i}\ln \theta) + \sum_{i=1}^{N}(1- x_{i}) \ln (1 - \theta)\rbrack \\
&=& \frac{1}{\theta} \sum_{i=1}^{N} x_{i} - \frac{1}{1 - \theta} \sum_{i=1}^{N}(1- x_{i}) 
\end{eqnarray}
$$

　　令偏导为0得到最大解下的参数$$\theta$$的值：

$$
\frac{1}{\theta} \sum_{i=1}^{N} x_{i} - \frac{1}{1 - \theta} \sum_{i=1}^{N}(1- x_{i}) = 0 \\
\Rightarrow \left ( 1 - \theta \right ) \sum_{i=1}^{N} x_{i} - \theta \sum_{i=1}^{N}(1- x_{i}) = 0 \\
\Rightarrow \sum_{i=1}^{N} x_{i} - \theta \sum_{i=1}^{N} x_{i} - \theta \sum_{i=1}^{N} + \theta \sum_{i=1}^{N} x_{i} = 0 \\
\Rightarrow N\theta = \sum_{i=1}^{N} x_{i} \\
\Rightarrow \theta = \frac{1}{N}\sum_{i=1}^{N} x_{i}
$$

　　我们将结果带进去，正面向上为2次，总次数为10次，代入后可得：

$$ \theta = 0.2 $$

　　这和我们预料的结果是一致的，极大似然估计其实就是频率派的做法，视待估参数为一个未知但固定的量、不考虑“观察者”的影响（亦即不考虑先验知识的影响），
我们接下来要说说更加“人性化”的贝叶斯派的估计方法。

# 极大后验概率估计（MAP估计）#

　　相比起极大似然估计，极大后验概率估计是更贴合贝叶斯学派思想的做法；也有不少人直接称其为“贝叶斯估计”，当然贝叶斯估计不仅仅是我们今天说的这个，但是整体的思想是相通的。
极大后验概率估计是假设参数$$ \theta $$具有一个先验概率，然后根据训练集$$\tilde X$$使得参数的可能性达到最大。举个例子，在上面抛硬币的例子，假如我们的经验告诉我们，
硬币一般都是匀称的，也就是$$ \theta = 0.5 $$的可能性最大，$$ \theta = 0.2 $$的可能性比较小，那么参数该怎么估计呢？这就是MAP要考虑的问题，
MAP优化的是一个后验概率，即给定了观测值后使$$ \theta $$概率最大：

　　在讨论 MAP 估计之前，我们有必要先知道何为后验概率$$p(\theta|\tilde{X})$$：它可以理解为参数$$ \theta $$在训练集$$\tilde X$$下所谓的“真实的出现概率”，
能够利用参数的先验概率$$p\left( \theta \right)$$、样本的先验概率$$p(\tilde{X})$$和条件概率$$p\left( \tilde{X}|\theta \right) = \prod_{i = 1}^{N}{p\left( x_{i}|\theta \right)}$$来计算得到。
这中间其实做了一个贝叶斯变换，我们待会解说。MAP 估计的核心思想就是将待估参数$$ \theta $$看成是一个随机变量、从而引入了极大似然估计里面没有引入的、参数$$ \theta $$
的先验分布。MAP 估计$${\hat{\theta}}_{\text{MAP}}$$的定义为：

$$
\begin{eqnarray}
{\hat{\theta}}_{\text{MAP}} &=& \arg {\max_\theta {p(\theta \vert \tilde{X})}} \\
&=& \arg{\max_\theta{}\frac{p(\tilde{X} \vert \theta) p(\theta)}{p(\tilde{X})}} \\
&=& \arg{\max_\theta{} \frac{p(\theta) \prod_{i = 1}^{N}{p(x_{i} \vert \theta)}}{p(\tilde{X})}} 
\end{eqnarray}
$$

　　由于$$p(\tilde{X})$$与参数$$ \theta $$无关，所以上式可写为：

$$
\arg{\max_\theta{}\frac{p(\tilde{X} \vert \theta) p(\theta)}{p(\tilde{X})}} \\
= \arg{\max_\theta{} p(\theta) \prod_{i = 1}^{N}{p(x_{i} \vert \theta)}}
$$

　　同样的，为了计算简洁，我们通常对上式取对数：

$$
{\hat{\theta}}_{\text{MAP}} = \arg{\max_\theta{\ln{p(\theta|\tilde{X})} = \arg{\max_\theta\left\lbrack \ln{p\left( \theta \right)} + \sum_{i = 1}^{N}{\ln{p\left( x_{i} \middle| \theta \right)}} \right\rbrack}}}
$$

　　可以看到，从形式上、极大后验概率估计只比极大似然估计多了$$\ln{p(\theta)}$$这一项，不过它们背后的思想却相当不同。不过有意思的是，
在之后具体讨论朴素贝叶斯算法时我们会看到、朴素贝叶斯在估计参数时选用了极大似然估计法、但是在做决策时则选用了 MAP 估计。和极大似然估计相比，
MAP 估计的一个显著优势在于它可以引入所谓的“先验知识”，这正是贝叶斯学派的精髓。当然这个优势同时也伴随着劣势：它要求我们对模型参数有相对较好的认知，
否则会相当大地影响到结果的合理性。

　　既然先验分布如此重要，那么是否有比较合理的、先验分布的选取方法呢？事实上，如何确定先验分布这个问题，正是贝叶斯统计中最困难、最具有争议性却又必须解决的问题。
虽然这个问题确实有许多现代的研究成果，但遗憾的是，尚未能有一个圆满的理论和普适的方法。这里拟介绍“协调性假说”这个相对而言拥有比较好的直观的理论：

　　**我们选择的参数$$ \theta $$的先验分布、应该与由它和训练集确定的后验分布属同一类型。**

　　此时先验分布又叫共轭先验分布。这里面所谓的“同一类型”其实又是难有恰当定义的概念，但是我们可以直观地理解为：概率性质相似的所有分布归为“同一类型”。比如，
所有的正态分布都是“同一类型”的。我们本博客对参数估计做了一定的数学上的解释，下篇博客主要写朴素贝叶斯算法，真正难的地方即将开始。

谢谢观看，希望对您有所帮助，欢迎指正错误，欢迎一起讨论！！！