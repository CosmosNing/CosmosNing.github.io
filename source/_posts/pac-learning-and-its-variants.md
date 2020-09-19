---
title: PAC 模型及其变体
toc: true
mathjax: true
date: 2020-09-19 09:30:55
description: 本文主要介绍机器学习中一种较通用的 PAC 学习模型及其拓展形式。
tags:
- PAC
- Agnostic PAC
- Generalized loss function
categories:
- 从理论到算法理解机器学习
---

>本文将介绍通用的学习模型 PAC 及其变体形式。
>
>* 最原始的 PAC 主要需要满足**可实现假设**，**训练样本量要大于样本复杂度函数**。
>
>* 现实情况中，**可实现假设难以满足**（先验知识不一定猜得准），因此**将目标标记函数改为数据-标签分布，并从中采样训练样本**，就可以类似的得到 Agnostic PAC 。
>
>* 原始的 PAC 中，标记函数将学习问题限制在二元分类任务中。为了获得更加通用的学习模型，我们定义更加一般的损失函数。通过损失函数，可计算真实误差和经验风险：
>
>  > * 在**问题分布**（$\mathcal{D}$）上采样得到的误差函数的**期望**，作为**真实误差**；
>  >
>  > * 在**训练样本**（$S$）上采样得到的误差函数的**平均值**，作为**经验风险**；

# PAC Learning

在[上一篇文章](/2020/09/16/statistical-learning-framework-and-empirical-risk-minimization/)中，我们证明了对于一个有限的假设集，<mark>如果拥有足量的训练样本（这些训练样本是从同一分布中独立采样，并被同一标记函数标记而生成的），ERM 会输出近似正确的假设</mark>。更一般的，我们定义 *Probably Approximately Correct Learning* （PAC）如下：

{% note primary%}

**Def.    PAC Learnability**

如果

* 存在一个函数 $m_{\mathcal{H}}:(0,1)^2\rightarrow \mathbb{N}$ ，
* 并且存在一个学习算法满足以下条件：
  * 对于任意的 $\epsilon, \delta\in(0,1)$ ，任意的 $\mathcal{X}$ 上的分布 $\mathcal{D}$ ，任意的标记函数 $f: \mathcal{X}\rightarrow \lbrace 0,1\rbrace$ ，如果对于 $\mathcal{H,D},f$ 可实现假设（*realizability assumption* ，即 $\exists\ h^\star\in \mathcal{H}, L_{D,f}(h^\star) = 0$）成立，那么当学习算法的训练样本量 $m \ge m_{\mathcal{H}}(\epsilon, \delta)$ 时（这些训练样本是由从 $\mathcal{D}$ 中独立采样，并被 $f$ 标记而生成的），学习算法会以至少 $1 - \delta$ 的概率返回一个假设 $h$，并且该假设满足 $L_{(\mathcal{D},f)}(h) \le \epsilon$

则一个假设类 $\mathcal{H}$ 是 PAC 可学习的

{% endnote %}

{% note info%}

**有关 PAC Learnability 的补充说明**

* $\epsilon$ 称作**精度参数**（*accuracy parameter*），代表输出分类器与最优之间的误差大小（对应 PAC 中的 ***approximately correct***）。这意味着，$\epsilon$ 允许学习算法犯一些小错误。
* $\delta$ 称作**置信参数**（*confidence parameter*），代表输出分类器满足上述精度要求的可能性（对应 PAC 中的 ***probably***）
* $m_{\mathcal{H}}:(0, 1) \rightarrow \mathbb{N}$ 称作**样本复杂度**（*sample complexity*）函数，表示**需要多少训练样本数据，才能够保证获得一个尽可能近似正确的答案**。该函数依赖于 $\epsilon, \delta,\mathcal{H}$ 。依据 PAC Learnability 的定义，存在不止一个 $m_{\mathcal{H}}$ 满足条件。通常，我们定义 $m_{\mathcal{H}}$ 为满足条件的**最小整数**。

{% endnote %}

对上一篇文章中获得的结论，我们换一种表达，则有下面的推论

{% note default %}

**PAC 可学习的样本复杂度条件**

每一个有限假设类都是 PAC 可学习的，只要它的样本复杂度满足：
$$
m_{\mathcal{H}} \le \lceil \frac{log(\frac{\mathcal{H}}{\delta})}{\epsilon} \rceil
$$
{% endnote %}

# A More General Learning Model

对于大多数任务，可实现假设很难得到满足，而且上述定义只适用于二元分类问题，因此通用性不佳。为了得到更加通用的学习模型，我们对上述模型从以下两个方面进行拓展：

1. 去除可实现假设。引入 **Agnostic PAC** 解决这个问题。
2. 学习比二元分类问题更加广泛的任务。通过对**损失函数的一般化**来支持这种拓展。

## Releasing the Realizablity Assumption —— Agnostic PAC Learning

### A More Realistic Model for the Data-Generating Distribution

简单来说，我们可以将 “目标标记函数” 替换为**数据-标签**生成分布。

规范地，设 $\mathcal{D}$ 为 $\mathcal{X} \times \mathcal{Y}$ 上的概率分布，其中 $\mathcal{X}$ 为领域集，$\mathcal{Y}$ 为标签集。也就是说，$\mathcal{D}$ 为在领域点和标签上的联合分布。我们可以将 $\mathcal{D}$ 看作由两个部分组成：一个在未被标记的领域点上的分布 $\mathcal{D}_x$ （有时候，也称作**边缘分布**） ，另一个为每一个领域点的标签上的**条件概率** $\mathcal{D}((x,y)|x)$ 。

{% note info %}

**如何理解条件概率？**

* 很多事情无法精确刻画，因此可用一个条件分布来描述。随着某一变量的变化，对应事件的概率也会发生变化。例如，$\mathcal{D}(y|x)$ 事件 $y$ 的概率会随着 $x$ 的变化而变化；此外，在 $\mathcal{D}(y|x)$ 中，$\mathcal{D}(y|x)$ 是关于 $y$ 的函数，而 $x$ 是作为函数的参数（如果要确定关于 $y$ 的函数关系，$x$ 要事先指定）。

{% endnote %}

### The Empirical and True Error Revised

与分布的改变相对应，我们定义预测规则 $h$ **真实误差**（或者叫做**风险**）如下：
$$
L_{\mathcal{D}}\overset{def}{=}\underset{(x,y)\sim \mathcal{D}}{\mathbb{P}}[h(x)\ne y] \overset{def}{=}\mathcal{D}(\lbrace (x,y): h(x) \ne y \rbrace) \tag{3.1}
$$
而经验风险并没有改变，依然是：
$$
L_{S}(h)\overset{def}{=}\frac{|\lbrace i\in[m] : h(x_i) \ne y_i \rbrace|}{m}
$$


### The Goal

学习算法的目标是寻找一些假设，$h:\mathcal{X}\rightarrow \mathcal{Y}$ ，尽可能近似最小化真实风险，$L_{\mathcal{D}}(h)$

### The Bayes Optimal Predictor

给定任意的在 $\mathcal{X} \times \lbrace 0,1\rbrace$ 上的概率分布，从 $\mathcal{X}$ 到 $\lbrace 0, 1 \rbrace$ 的最佳标记函数为
$$
f_{\mathcal{D}}(x) = \begin{cases} 1 &if\ \mathbb{P}[y=1|x]\ge \frac{1}{2}\\ 0 & otherwise\end{cases}
$$
这也就是说，对于任意的二元分类器 $g: \mathcal{X}\rightarrow \lbrace 0, 1\rbrace$，$L_{\mathcal{D}}(f_{\mathcal{D}}) \le L_{\mathcal{D}}(g)$

{% note info %}

**证明 The Bayes Optimal Predictor**（TODO）

{% endnote %}

一旦我们对数据生成分布不做任何预先假设，那么将没有算法将保证找到一个预测器，媲美 Bayes Optimal Predictor 的效果。因此，我们需要学习算法找到一个预测器，在给定某些假设类的基准测试时，**它的误差并不会比可能的最佳的预测器的误差大太多**。因此，我们有如下的 **Agnostic PAC Learnability**：

{% note primary%}

**Def.    Agnostic PAC Learnability**

如果

* 存在一个函数 $m_{\mathcal{H}}:(0,1)^2\rightarrow \mathbb{N}$ ，

* 并且存在一个学习算法满足以下条件：

  * 对于任意的 $\epsilon, \delta\in(0,1)$ ，任意的 $\mathcal{X} \times\mathcal{Y}$ 上的分布 $\mathcal{D}$ ，当学习算法的训练样本量 $m \ge m_{\mathcal{H}}(\epsilon, \delta)$ 时（这些训练样本是由从 $\mathcal{D}$ 中独立采样而生成的），学习算法会以至少 $1 - \delta$ 的概率返回一个假设 $h$，并且该假设满足：
    $$
    L_{\mathcal{D}}(h) \le \underset{h'\in\mathcal{H}}{min}L_D(h') + \epsilon
    $$
    

则一个假设类 $\mathcal{H}$ 是 Agnostic PAC 可学习的

{% endnote %}


## Generalized Loss Function —— Agnostic PAC Learning for General Loss Function

为了适应不同的任务，我们接下来定义通用的损失函数。

给定任意的集合 $\mathcal{H}$ 和领域集 $Z$ ，设 $l$ 为任意 $\mathcal{H}\times Z$ 到非负实数的函数， $l: \mathcal{H} \times Z \rightarrow \mathbb{R}_+$ ，我们称 $l$ 为**损失函数**。

下面用分类器误差的期望来定义**风险函数**。

给定一个 $h\in \mathcal{H}$ ，关于在 $Z$ 上的概率分布 $\mathcal{D}$ ，对应的风险函数为：
$$
L_{\mathcal{D}}(h)\overset{def}{=}\underset{z\sim \mathcal{D}}{\mathbb{E}}[l(h,z)]\tag{3.3}
$$
相似的，经验风险函数为对采样样本 $S=(z_1,\dots,z_m)\in Z^m$ 上的损失的期望：
$$
L_S(h)\overset{def}{=}\frac{1}{m}\sum_{i=1}^{m}l(h,z_i)\tag{3.4}
$$
综上所述，有如下更加通用的 Agnostic PAC 的定义：

{% note primary %}

**Def.    Agnostic PAC Learning for General Loss Function**

如果

* 存在一个函数 $m_{\mathcal{H}}:(0,1)^2\rightarrow \mathbb{N}$ ，

* 并且存在一个学习算法满足以下条件：

  * 对于任意的 $\epsilon, \delta\in(0,1)$ ，任意的 $Z$ 上的分布 $\mathcal{D}$ ，当学习算法的训练样本量 $m \ge m_{\mathcal{H}}(\epsilon, \delta)$ 时（这些训练样本是由从 $\mathcal{D}$ 中独立采样而生成的），学习算法会以至少 $1 - \delta$ 的概率返回一个假设 $h\in \mathcal{H}$，并且该假设满足 
    $$
    L_{\mathcal{D}}(h) \le \underset{h'\in\mathcal{H}}{min}L_{\mathcal{D}}(h')+ \epsilon
    $$
    其中 $L_{\mathcal{D}}(h) = \mathbb{E}_{z\sim D}[l(h, z)]$

则一个假设类 $\mathcal{H}$ 关于一个集合 $Z$ 和 损失函数 $l:\mathcal{H}\times Z\rightarrow \mathbb{R}_+$ 是 Agnostic PAC 可学习的

{% endnote %}

# Reference

Shai Shalev-Shwartz, Shai Ben-David. [Understanding Machine Learning: From Theory to Algorithms](https://www.cs.huji.ac.il/~shais/UnderstandingMachineLearning/index.html)