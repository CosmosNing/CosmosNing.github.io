---
title: Vim简易入门
toc: true
mathjax: false
date: 2020-06-15 12:07:06
description: 介绍 Vim 的常见命令及其用法
tags:
- Vim
- Linux
- 命令
categories:
- 指南
---

> 最近，笔者在学习 Linux 中的常见命令。其中 Vim 作为文本编辑命令，是十分基础又非常强大的工具。然而，许多开发者因其命令繁琐，上手难度大，并没有完全发挥出 Vim 的潜力。因此，本文将介绍 Vim 的常见命令及其用法，供读者参考。通过本文，你将获得：
>
> 1. 了解 Vim 的四种模式。
> 2. 掌握 Vim 常见文本编辑命令。

![题图](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/06/kdwk-leung-nupZCrr61Fg-unsplash.jpg)

Photo by [Kdwk Leung](https://unsplash.com/@kdwk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/t/nature?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 什么是 Vim？

Vim 是 **Vi** I**M**proved 的缩写。它是由 Bram Moolenaar 开发的开源免费文本编辑器。顾名思义，Vim 基于 Vi（另一个早期的文本编辑器） 但又不止于 Vi。由于其强大的文本处理功能，在很多类 Unix 系统中，Vim 都成了默认的文本编辑器。尤其在无图形化界面的服务器上，Vim 成为了绝大部分运维人员线上运维的选择。

很多 Linux 发行版都内置了 Vim 命令，只需打开终端，输入如下命令，即可使用 Vim。

``` bash
vim
```

![Vim](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/06/vim.PNG)

# Vim 的四种模式

Vim 中的四种模式是 Vim 的核心设计理念之一。以四种模式为基础，理解和使用 Vim 的文本编辑命令就会容易许多。

## 总览

Vim 中有四种模式，分别是：

* 正常模式（Normal Mode）：用于查看文本和执行复制、粘贴等文本操作命令。
* 插入模式（Insert Mode）：用于编辑文本。
* 命令模式（Command Mode）：用于执行指定命令。
* 可视模式（Visual Mode）：用于选中文本。

## 正常模式（Normal Mode）

在 Vim 打开文件或者新建文件时，默认进入的模式是正常模式。在正常模式下，使用键盘直接输入字母或者数字，可能不会出现类似编辑文本时的结果。也就是说，该模式下，文本无法直接编辑。然而，通过某些特定的指令，可以完成一些常见的文本操作命令。

### 移动光标

光标的位置将决定复制、粘贴等操作的位置，因此光标的操作于文本操作十分重要。不同于当前拥有图形界面的文字处理软件，Vim 中无法使用鼠标指定光标位置，而是使用键盘指令，完成光标的移动。这一设计，使得 Vim 无需鼠标，即可完成文本编辑的任务。这大大提高了文本编辑的效率。

Vim 的光标移动又可以分为两类。分别是**小范围移动**、**大范围移动**。

在**小范围移动**中，只涉及到四个字母 `h`、`j`、`k`、`l`，分别代表**左、下、上、右**（相当于键盘上的光标键）

![小范围移动光标](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/06/cursorSD.png)

{% note info %}

部分 Vim 版本支持**光标键**移动光标，但仍有部分 Vim 不支持（由于相关操作系统会将光标键解释为其他字符），推荐熟练掌握 `h`、`j`、`k`、`l` 指令，完成小范围移动光标的任务。

{% endnote %}

在**大范围移动**中，可以通过 `g`、`G` 快速跳转到**第一行**和**最后一行**；在 `G` 前加上数字，就可以跳转到指定行；`^`、`$` 则可以跳转到光标所在行的行首和行尾。这些移动方式将会极大地提高光标移动效率。

![大范围移动光标](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/06/cursorLD.png)

### 常见文本操作

在正常模式下，除了可以移动光标，也可以执行一些简单的文本操作命令。具体的如下表所示：

|     功能     |        指令         |                     备注                     |
| :----------: | :-----------------: | :------------------------------------------: |
|     复制     |        `yy`         |                   复制整行                   |
|              |     `<数字>yy`      |  复制当前光标所在行开始，连续<数字>行的文本  |
|              | `y$`（y +shift +4） | 复制从当前光标所在字符开始，到本行末尾的文本 |
|     粘贴     |         `p`         |                 粘贴复制结果                 |
|     剪切     |        `dd`         |                   剪切整行                   |
|              |        `d$`         | 剪切从当前光标所在字符开始，到本行末尾的文本 |
|     撤销     |         `u`         |                   撤销命令                   |
|     重做     |     `ctrl + r`      |                   重做命令                   |
| 删除单个字符 |         `x`         |           将当前光标位置的字符删除           |
| 替换单个字符 |   `r+<指定字符>`    |      将当前光标位置的字符替换为指定字符      |

## 插入模式（Insert Mode）

在正常模式中，用户无法直接编辑文本。而如果需要直接编辑文本，则需要进入插入模式。插入模式有多种进入方式。具体如下：

|             进入命令             |               效果               |
| :------------------------------: | :------------------------------: |
|           按 `i` 进入            |           光标会在原位           |
| 按 `I `（大写 i：shift + i）进入 |         光标会在本行开头         |
|           按 `a ` 进入           |       光标会在原位的下一位       |
|     按 `A `（shift + a）进入     | 光标会在本行最后一个字符的下一位 |
|           按 `o` 进入            |   光标会在当前行的下面另起一行   |
|     按 `O` （shift + o）进入     |   光标会在当前行的上面另起一行   |

进入插入模式之后，就可以直接编辑文本了。编辑完成后，可以按 `ESC` 键返回正常模式。

## 命令模式（Command Mode）

当处于正常模式时，按 `:` 键可进入命令模式。在命令模式下，可执行更多丰富的命令。这些命令大致可分为两类，一类命令用于设置 Vim 编辑器的属性，另一类则用来完成文件、文本操作任务。

### Vim 设置

vim 设置的一般格式如下

``` bash
: set <设置项>
```

如

``` bash
: set nu
```

可显示行号。

![Vim 显示行号](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/06/setnu.PNG)

然而，上述设置方式仅对当前编辑文本进程有效，如需永久生效，则要修改 `/etc/vimrc` 文件。

### 文件、文本操作

在命令模式下，Vim 可以执行一些文件操作命令，如保存、退出等；亦可执行更加复杂的文本操作命令，如查找、替换等。

|     功能     |                         命令                          |                   备注                    |
| :----------: | :---------------------------------------------------: | :---------------------------------------: |
|     保存     |                    `: w <文件名>`                     |                保存新文件                 |
|              |                         `: w`                         |            表示保存到当前文件             |
|     退出     |                         `: q`                         |                 退出 vim                  |
|              |                        `: q !`                        |        强制退出（默认不保存修改）         |
| 执行其他命令 |                     `: ! <命令>`                      |              执行 shell 命令              |
|   查找操作   |                   `: /<正则表达式>`                   | 按 `n` 查找下一个，`shift + n` 查找上一个 |
|   替换操作   |    `: s/<旧字符或正则表达式>/<新字符或正则表达式>`    |     针对当前光标所在行的字符进行替换      |
|              |   `: %s/<旧字符或正则表达式>/<新字符或正则表达式>`    |             全文搜索替换一处              |
|              |  `: %s/<旧字符或正则表达式>/<新字符或正则表达式>/g`   |            全文搜索、全部替换             |
|              | `: m,n s/<旧字符或正则表达式>/<新字符或正则表达式>/g` |   指定行范围内替换，其中 m，n 为正整数    |

## 可视模式（Visual Mode）

在复制等文本操作命令时，常常需要选中相关文本。这就需要进入 Vim 的可视模式。进入可视模式的方式比较常见的有 3 种，具体如下表所示：

|      进入命令      |                             解释                             |
| :----------------: | :----------------------------------------------------------: |
|    按 `v` 进入     |     字符可视模式，按行序以**单个字符**为单位进行选中操作     |
|    按 `V` 进入     |        行可视模式，按行序以**单行**为单位进行选中操作        |
| 按 `ctrl + v` 进入 | 块可视模式，以光标所在位置为顶点，自定义矩形块中的字符全部选定 |

按 `ESC` 键返回正常模式。

# 总结

本文主要介绍了 Vim 的四种模式和四种模式下的文本编辑及操作命令，希望对读者有所帮助。

![Vim 四种模式](https://raw.githubusercontent.com/CosmosNing/MyPicGo/master/images/2020/06/Vim-Four-Modes.png)

# 拓展阅读

[【视频·中文】vim入门教程（共3讲） by 正月点灯笼](https://www.bilibili.com/video/BV1Yt411X7mu)

[【文本·英文】Vim - Quick Guide from tutorialspoint](https://www.tutorialspoint.com/vim/vim_quick_guide.htm)





