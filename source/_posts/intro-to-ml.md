---
title: 《从理论到算法理解机器学习》引言
toc: true
mathjax: true
date: 2020-09-13 14:51:46
description: 学习是什么？什么情况下我们需要机器学习？学习分为哪几种？机器学习和其他领域有什么样的关系？
tags:
- Learning
- Generalization
- Inductive reasoning
- Inductive inference
- Prior knowledge
- Inductive bias
- No-Free-Lunch Theorem
categories:
- 从理论到算法理解机器学习
---

> 最近上了一门课，叫做“智能信息检索”。老师推荐了一本教材 《Understanding Machine Learning: From Theory to Algorithms》（文末提供官方下载链接），来解释**智能**的含义。本文基于我对教材的理解，对其引言部分做了一些要点概括，供读者参考。

# What Is Learning？

粗略地讲，**学习是一个将经历（*experience*）转换为技能（*expertise*）或知识（*knowledge*）的过程**。一般地，一个学习算法的**输入**是**训练数据**（代表经历），**输出**是**技能**。

一种常见的学习方法是 “记忆学习”（*learning by memorization*）。类似人类学习的“死记硬背”，这种学习方法有时候十分有效，但是它却缺失了学习系统中一个十分重要的能力，即**泛化能力**（*generalization*）。换句话讲，一个成功的学习系统理应能够依据已知知识，正确处理未知的情况。

{% note info %}

**泛化能力** （*generalization*） 有时也被称作**归纳推理** （*inductive reasoning / inductive inference*）

{% endnote %}

然而，**归纳推理可能会导致错误结论**。那么怎样才能减少犯错呢？

答案是：在学习系统中**引入先验知识**（*prior knowledge*）！

{% note info %}

**先验知识**（*prior knowledge*） 有时也被称作**归纳偏好** （*inductive bias*）

{% endnote %}

从书中举得几个例子来看，**引入先验知识，使整个学习过程带有偏好，是一个成功的学习算法必然发生的事情**（这种现象是 **No-Free-Lunch Theorem** 的具体表现）。粗略来说，学习算法开始训练时所引入的先验知识越强，从训练样本中学习就越简单。但是，先验知识越强，学习的灵活性也就越低，因为整个学习过程被这些预定假设束缚住了。

# When We Need Machine Learning？

那么，在哪些情况下，我们需要利用机器学习解决问题呢？主要有以下两个方面：

* 问题的复杂性（*complexity*）
* 任务的适应性需求（*adaptivity*）

## Tasks That Are to Complex to Program

* *Tasks Performed by Animals / Humans*：我们暂时无法理解一些我们能够处理的日常任务，例如驾驶汽车、语音识别、图像理解。以现有的知识，难以抽象出一个良好的程序去执行这些任务。然而，先进的机器学习程序，只要接受足够的训练样本训练，就可以从中学习经验，并在这些任务中达到不错的效果。
* *Tasks beyond Human Capabilities*：另一类适合用机器学习解决的问题是对巨大的且复杂的数据（例如宇宙空间数据、医疗数据、天气数据等）进行分析。这些数据的处理和分析通常超越了我们自身的分析能力。使用机器学习，可以充分利用计算机的能力，找到数据中有价值的信息。

## Adaptivity

* 预先编程的程序有一个缺陷，即死板性（*rigidity*）。也就是说，一旦程序写好了，安装到系统中后，程序就不会变化了。然而，现实中许多任务都是在不断变化的。
* 而机器学习（其天生可以适应不同的输入数据）提供了一种解决此类任务的方案。

# Types of Learning

## Supervised versus Unsupervised

如果将学习看作是一个“利用经历来获取技能”的过程，那么监督学习和非监督学习可以描述如下：

* **监督（*Supervised*）学习**：在该场景中，“经历”（亦可称作训练样本）包含了十分重要的信息；而这些信息却在测试样本中丢失了。从监督学习中学习到的技能的目标是，预测测试数据中的确实信息。这就好像环境作为一名老师通过提供额外的信息（通常称作**“标签”** *label*）"监督"学生（学习算法）的学习。
* **非监督（*Unsupervised*）学习**：对于非监督学习来说，训练数据和测试数据几乎没什么区别。通常，这种学习方法通过处理输入数据，来达到**总结数据**、**压缩数据**的目标。**聚类**——将一个数据集分成几个子集，同一子集具有相似性——是非监督学习中一个典型的例子。
* 在监督与非监督之间，还存在一种不同的方法——**强化学习**。

## Active versus Passive

* **主动（*Active*）学习**：主动学习在在训练过程中将会与环境交互。
* **被动（*Passive*）学习**：被动学习只会观测环境所提供的信息，而不会影响（influence）或指导（direct）它。

{% note info %}

**例子**

* 垃圾邮件识别通常是被动学习，因为它等着用户为它标记收到的邮件是否是垃圾邮件。
* 在主动学习的场景下，学习算法会在训练过程中要求用户标记它指定的邮件，或者甚至是由学习算法生成的邮件，来增强它对什么是垃圾邮件的理解。

{% endnote %}

## Helpful of the Teacher

* 主动提供信息
  * 训练信息**正相关**——普通学习：例如，教师会不断尝试为学生提供最有用的信息，来达到某种学习目标。
  * 训练信息**负相关**——**对抗（*Adversarial*）学习**：例如垃圾邮件生成器将会努力误导垃圾邮件识别器。
* 被动提供信息
  * **统计学习（*Statistical Learning*）**：有时候，信息并不是他人主动提供的。例如科学家在认识自然时，环境，作为一位老师，并不会根据学生的需求主动的提供信息，因此是被动的提供信息。这种情况下，输入数据常常被认为是由某种随机过程（*random process*）产生。这样，就可以使用统计学习解决问题

## Online versus Batch Learning Protocol

* **在线学习（*Online Learning*）**：学习算法必须在训练过程中，在线反馈。例如，股票交易员需要基于目前位置积累的经验，做出每日决策。虽说随着时间推移，他会称为一名专家，但是也可能在此过程中犯下代价高昂的错误。
* **批处理学习（*Batch Learning*）**：只有在处理大量的数据后，该算法才能获得所需的技能。在数据挖掘场景下，数据挖掘算法会处理大量的数据之后，才会得出相关的结论。

# Relations to Other Fields

## With AI

* 机器学习是 AI 的一个分支。
* 机器学习更关注**利用计算机的优势和独特的能力**来**补充**人类的智能，因此常常处理一些远超人类能力的任务。
* AI 更关注创造一个能够自动**模仿**智能行为的机器。

## With Statistics

* 统计学：提出假设（专业人员） $\rightarrow$ **验证假设**（统计学）
* 机器学习：利用数据，**挖掘规律**，解释原因。
* 机器学习对**算法**层面的考虑更多
* 统计学常常关注数据的**渐进性**（**无穷**情况下的特性），机器学习则关注**有限**的样本空间
* 统计学通常**预先假设数据分布**，而机器学习是**分布无关**的（*distribution-free*）

# Reference

Shai Shalev-Shwartz, Shai Ben-David. [Understanding Machine Learning: From Theory to Algorithms](https://www.cs.huji.ac.il/~shais/UnderstandingMachineLearning/index.html)