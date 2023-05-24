---
title: Logseq：开源优雅的知识管理工具
toc: true
mathjax: false
date: 2020-12-06 14:29:14
permalink: review-of-logseq/
description: 最近，我看到一款非常好用的笔记软件——Logseq，本文主要聊聊我的感受和体验。
tags:
- 工具
- 笔记
categories:
- 杂谈
---

![题图](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/12/altumcode-GVASc0_Aam0-unsplash.jpg)

# 前言

笔记，对于我个人来说，是一个非常重要的整理与回顾知识的工具。找到适合自己的笔记载体，将会极大地提高的效率。为追求最佳效率，我尝试过多种方式，从手写到电子，从文档到导图。它们各具特色，又难免缺失某种特性而使我最终放弃。

## 印象笔记

16年，新买的笔记本电脑上预装了印象笔记。当时的印象笔记还仅支持富文本编辑模式。富文本编辑意味着所见即所得，格式可以轻松地点击菜单按钮随时调整。

但是对于我来说，这就意味着我将要腾出一只手握住鼠标，在需要时调整格式。这其实会降低笔记的效率，因为这种模式下，你不仅仅要关注笔记的内容，而且需时不时地分心调整格式。这与我追求高效的笔记体验背道而驰。

除此以外，在富文本模式下，公式的输入非常繁琐，这也是我最终放弃印象笔记的原因之一。

虽然，印象笔记在 18 年开始支持 Markdown，但其编辑、预览双屏的设计，对于追求简洁的我太过复杂，也就没有再拾起。

![印象笔记 Markdown 双屏设计](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/12/yinxiang-markdown.png)

## Typora

18 年左右，经朋友推荐，了解到 [Typora](https://typora.io/) 这款简洁的 Markdown 编辑器，并一直使用至今。

![Typora:所见即所得](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/12/typora.png)

Typora 主要令我喜爱的一点是其“**所见即所得**”的即时 Markdown 渲染。该软件只用一屏，既提供了 Markdown 的排版功能，又减去了编辑模式下 Markdown 中的影响阅读体验的格式符号。

![Typora 代码块](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/12/fences.png)

![Typora 数学公式](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/12/math.png)

然而，Typora 只是一个编辑器，只能记录，**难以检索和回顾**。因此，仅靠 Typora ，难以完成笔记的完整闭环。一种常见的工作流可以勉强解决这个问题，即 Typora（编辑）+ Hexo（生成静态博客网页）+ GitHub Pages（发布与检索）。然而，笔记的**私密性**和博客的**开放性**的矛盾无法得到有效解决，这使我陷入了两难的境地。

## 幕布

![幕布](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/12/mubu.png)

20 年，准备考研复试的专业课时，我开始尝试幕布这个在线笔记软件。对于整理概念性的知识，幕布绝对是一个非常强大的工具。将一门课，一本书化为一个个知识点，并通过层次表达它们的关系，最后还能生成相应的思维导图，这个笔记体验真的是太美妙了。

美中不足的是，幕布并**不支持公式和代码块的渲染**，这对于有大量公式代码笔记需求的我来说有点遗憾。

## Logseq

最近，在 GitHub 上发现了一款开源笔记应用，[Logseq](https://github.com/logseq/logseq)，个人感觉比较完美的满足了我的几点需求：

1. 支持 Markdown，并且所见即所得 ；
2. 支持公式和代码块渲染；
3. 保持笔记的私密性。

下一部分，我将聊一聊我对 Logseq 的使用体验。

# Logseq 初体验

## 快速上手

使用 Logseq 非常简单，只需要你拥有一个浏览器，一个 GitHub 账户及笔记仓库。

{% note info %}

如果要保持你笔记的私密性，你可以创建一个**私有**的 GitHub 笔记仓库供 Logseq 存储数据文件。

{% endnote %}

当你，准备好上述要求，只需访问 [https://logseq.com/](https://logseq.com/) ，并使用 Github 登录授权，设置好笔记仓库，就可以开始你的笔记了。

![Logseq默认界面](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/12/daily-notes.PNG)

登陆后，Logseq 会默认创建一个标题为今天的文件，你可以直接在此开始笔记。如果要创建新的页面，你可以在左上的搜索框输入新页面的文件名，并选择相应的下拉选项，完成创建。

## 体验感受

从我的使用体验来说，Logseq 的手感是十分惊艳的。主要有如下几点：

* **快速笔记**
  * 十分类似于幕布的高效的笔记手感
* **数据完全属于你**
  * Logseq 不保存你的任何数据。它在运行过程中，仅仅将你的数据缓存在浏览器的本地缓存中，并与你设定的 GitHub 仓库同步。
* **强大的页面引用、块引用**
  * 受 [Roam](https://roamresearch.com/) 启发
  * 这建立了独立的笔记文件之间的联系，更利于笔记整合成知识库。
* **支持 Markdown，并且所见即所得**

![Logseq 中 Markdown 支持情况](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/12/markdown-support.png)

* **丰富的命令**
  * 键入 `/` （slash 键），你会发现一个全新的大陆：可以创建待办事项、插入页面引用或者块引用，甚至还能在其中插入一个画图的页面。

![Logseq 中的命令](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/12/commands.PNG)

## 使用场景

在我看来，Logseq 具有非常丰富的使用场景：

* **自由写作**：你可以在 Logseq 写下任何你思想的火花，供日后整理。
* **待办事项**：Logseq 提供了比较丰富的待办事项功能，包括创建待办事项，设置事项优先级，设定 deadline 和 schedule。
* **知识整理**：这也许是 Logseq 最核心的使用方式之一。
* ...（也许还有更多）

![Logseq 的 deadline 和 schedule](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/12/deadline-and-schedule.png)

# 从内联到外联

传统的笔记软件，通常把笔记独立为一个一个文件，关注一个文件内的内在联系，而忽视了不同笔记之间的联系。

Logseq 受 Roam 启发，将不同的笔记文件通过**页面引用**联系起来，从而更好地构建自己的知识网络。

![Logseq 图谱示例](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/12/KG.PNG)

近期，新版的幕布也上线了 `@文件名` 引用不同的笔记，想必也是受到类似应用的启发。

由此可见，笔记软件正在从内联走向外联，越来越符合对知识整理的需求。 

# 不足

目前来说，Logseq 还存在许多不足：

* Logseq 还处在快速迭代阶段，很多功能存在不稳定的可能；
* 相关的文档稀少，对新手不太友好；

但我相信，开源社区的支持会使得它更加完善。