---
title: Git基础命令总结
toc: true
mathjax: true
permalink: summary-of-basic-git-commands/
date: 2023-06-02 16:01:54
description: 总结常用的Git命令及其用法
tags: 
- Git
categories:
- 笔记
---

## 总结图示

![Git基础命令总结图示](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2023/06/02/git_cmd.drawio.png)

### 核心概念

* 工作区：即当前仓库目录下的所有文件所在的区域
* 暂存区：`git add` 命令执行后，修改文件暂时所在的区域（等待后续`git commit` 命令统一提交）
* 本地仓库：`git commit`命令执行后，提交文件所在的区域
* 远端仓库：与本地仓库关联；通过`git push`可将本地提交同步至远端。

### 核心操作流

Git的核心操作流为：`git add` $\rightarrow$ `git commit` $\rightarrow$ `git push`，其中，

*  `git add`将修改的文件加入到暂存区；
* `git commit` 将暂存区的修改提交至本地仓库；
* `git push`将本地仓库的提交推送到远端仓库。

另外，记住在`git add`与`git commit`之前，记住执行 `git pull`，从远端拉取最新代码，并解决潜在的冲突。

### 不同区域的撤销操作

一条常规的修改提交操作流是 `git add` $\rightarrow$ `git commit` $\rightarrow$ `git push`。但是如果在其中某一步，我们需要撤销相应的操作，该怎么办呢？Git提供了如下命令，在不同区域实现撤销操作：

* 对于已纳入Git管理的文件，如果要丢弃工作区中的修改，可使用：
  * `git restore <file>`
  * `git checkout -- <file>`
* 对于已经被`git add`命令加入暂存区的文件，可使用：
  * `git restore --staged <file>` 将暂存区的相应文件放回工作区，并保持修改不改变
  * `git reset HEAD <file>`（同上）
* 对于已经被 `git commit` 命令提交至本地仓库，可使用：
  * `git reset --soft <commit id>` 将提交头节点（HEAD）移动到指定commit id，**并将修改保留在暂存区**。
  * `git reset --mixed <commit id>` 将提交头节点（HEAD）移动到指定commit id，**并将修改保留在工作区**。
  * `git reset --hard <commit id>` 将提交头节点（HEAD）移动到指定commit id，**不保留任何修改**（如果修改包含新文件的提交，那么这个新文件将被删除。**请慎用**）。
* 对于已经被`git push` 到远端仓库，没有相应的一条命令实现，但是可采用如下方法：
  * 首先，通过`git reset` 命令将提交头节点移动到指定提交
  * 再通过 `git push -f` 强制更新远端
  * 最后再修改本地仓库文件，进行 `git add` $\rightarrow$ `git commit` $\rightarrow$ `git push`操作。


## 参考

* [git reset --soft,--hard的区别](https://www.jianshu.com/p/c6927e80a01d)
* [git restore指令和git restore --staged 的使用)](https://juejin.cn/post/7130883174472630286)
* [Git撤销已经推送(push)至远端仓库的提交(commit)信息](https://blog.csdn.net/hanchao5272/article/details/79435730)

## 推荐阅读

* [Git 基础入门](/2019/12/30/git-ji-chu-ru-men/)