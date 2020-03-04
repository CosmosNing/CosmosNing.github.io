---
title: Git基础入门
toc: true
mathjax: true
date: 2019-12-30 19:32:05
description: Git 从安装到使用
tags:
- 工具
- 版本控制
- git
categories:
- 笔记
---

# Git 基础入门

## 下载与安装

https://git-scm.com/downloads

## 最小配置

### 配置 `user.name` 和 `user.email`

* `git config` 命令

```shell
git config --global user.name '<用户名>'
git config --global user.email '<电子邮件>'
```

* 参数简析

|       --local        |          --global          |           --system           |
| :------------------: | :------------------------: | :--------------------------: |
| 仅对**某个仓库**有效 | 对**当前用户**所有仓库有效 | 对**系统**所有登录的用户有效 |

* 显示 `config` 配置，加上 `--list`, 如

```shell
git config --list -- global
```

## 创建仓库

### 已有项目纳入 Git 管理

```shell
cd <项目根目录>
git init
```

### 新建项目用 Git 管理

```shell
cd <某个文件夹>
git init <项目名> # git 会创建与项目名同名的文件夹
cd <项目名>
```

### （可选）配置 local 用户信息

```shell
git config --local user.name '用户名'
git config --local user.email '电子邮件'
```

## 暂存、提交

### 暂存

```shell
git add <待提交的文件>...
```

* 参数简析

|                      -u                       |                   -A                   |
| :-------------------------------------------: | :------------------------------------: |
| 语义为 update，即将已经跟踪的文件添加到暂存区 | 语义为 all，即将所有的文件添加到暂存区 |

### 提交

```shell
git commit -m '<提交信息>'
```

### 其他常用的命令

* `git status`：git 管理文件状态

* `git log`：查看提交记录

  * ```shell
    git log               # 默认命令（仅显示当前分支）
    git log --oneline     # 仅显示历史提交信息
    git log -n<数字>       # 显示最近几次提交历史（次数由数字决定）
    git log --all         # 查看所有分支log
    git log --graph       # 图形化方式显示（log前会有线，分析多分支版本关系常用）
    ```

* `gitk`：图形化查看提交记录

* `git reset --hard`：清空暂存区所有修改

* `git cat-file`

  * ```shell
    git cat-file -t <哈希值>   # 查看对应 哈希值 对应的类型
    ```

  * ```shell
    git cat-file -p <哈希值>   # 查看对应 哈希值 对应的内容
    ```

* `git chekout`

  * ```shell
    git checkout <与当前分支不同的已存在的分支名> # 切换分支
    git checkout -b <新分支名> <commitid>     # 检出对应版本，并创建新分支管理
    ```

* `git branch`

  * ```shell
    #####显示命令#####
    # 默认命令，显示所有分支，并注明当前处于哪个分支
    git branch
    # 除了默认显示信息，加入了最近一次提交信息
    git branch -v
    
    #####创建命令#####
    # 基于当前最新版本，创建新分支
    git branch <新分支名>
    # 基于commitid 或 tag，创建新分支
    git branch <新分支名> < commitid | tag >
    
    #####删除命令#####
    # 删除对应分支
    git branch -d <已存在的分支名>
    ```

* `git diff`

  * ```shell
    git diff <commitid1> <commitid2> # 显示两个提交之间的差异
    ```

* 

## 应用

### 给文件重命名

```shell
git mv <原文件名> <新文件名>
git commit -m '提交信息'            # 可直接提交，不需通过 add 加入暂存区
```

## 其他

### .git 目录解析

#### 目录结构

```shell
cd .git
ls
```

```shell
# 目录结构（初始）
├─hooks
├─info
├─objects
│  ├─info
│  └─pack
└─refs
    ├─heads
    └─tags
# 目录结构（有提交后）
├─hooks
├─info
├─logs
│  └─refs
│      └─heads
├─objects
│  ├─1b
│  ├─c8
│  ├─dc
│  ├─info
│  └─pack
└─refs
    ├─heads
    └─tags
```

#### 含义

* `HEAD` 文件，存储了当前指向哪个分支
* `config` 文件：当前项目的配置信息
* `refs/heads` 文件夹：存储分支信息
  * 其中的文件，存储了各分支最新的 `commitid`
* `refs/tags` 文件夹：存储标签信息（用于如 **里程碑** 设置）
  * 其中的文件，存储了各标签对应的 `commitid`
* `objects` 文件夹：无含义的文件夹名和对应文件夹下的文件名拼接，形成哈希值，利用  `git cat-file` 可查看对应的信息。

### commit、tree、blob 对象之间的关系

* 每个 **commit** 对应一颗 **tree**
* **tree** 指明了当前 **commit** 的文件目录结构。 **tree** 中可包含 **tree** 和 **blob**
* **blob** 代表文件

### 潜在错误：分离头指针

* 输入如下命令

	* ```shell
    git checkout <哈希值>    # 分离头指针（这个分支没有被 git 管理）
    ```

* 如果在这个基础上 `commit` ，之后切换分支时，这些 `commit` 会丢失。

* 补救措施

  * ```shell
    git branch <新分支名> <commitid>
    ```

* 建议：检出某个版本时，推荐用新的分支管理

  * ```shell
     git checkout -b <新分支名> <commitid>
    ```

* 







