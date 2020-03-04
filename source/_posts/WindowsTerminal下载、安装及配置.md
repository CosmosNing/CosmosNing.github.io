---
title: WindowsTerminal下载、安装及配置
toc: true
mathjax: false
date: 2020-02-08 21:28:59
description: Windows Terminal 带来了类 Linux 的终端体验，本篇文章介绍它的安装与配置
tags:
- 工具
- Terminal
- Windows
categories:
- 指南
---

# 下载与安装

## Microsoft Store

* 打开 Microsoft Store
* 搜索 `Windows Terminal`
* 点击 `获取`
* 等待下载安装完成即可

P.S. 此版本比较老旧，如果想用最新版，推荐下面的方式安装

## Release pre-build（Recommended）

* 打开网址：https://github.com/microsoft/terminal/releases
* 选择最新 release 的 Assets 中的Microsoft.WindowsTerminal_XXXXXX_8wekyb3d8bbwe.msixbundle（其中，`XXXXXX` 为版本号）下载（建议在科学上网的条件下下载，否则容易下载失败）
* 下载完成后，双击该文件，按提示安装

## Build from source

P.S.这个配置较为复杂，暂且请参考[官方文档](https://github.com/microsoft/terminal#developer-guidance)

# 美化配置

P.S. 安装配置前推荐准备好科学上网

## 安装  Fira 字体



参见 [博客教程](https://www.misiyu.cn/article/134.html)



## 安装 oh-my-posh 前置



照着这个[博客教程](https://www.misiyu.cn/article/134.html) Fira code字体的安装，但是**安装oh-my-posh主题**时，*choco 安装终端模拟器 - ConEmu* 这一步总会出错。



在 [oh-my-posh 仓库](https://github.com/JanDeDobbeleer/oh-my-posh#prerequisites)中，找到了替代方案，即采用 Scoop 安装 ConEmu。



在此之前要先安装 Scoop，具体命令如下：



```powershell
# 下载安装 Scoop
$ set-executionpolicy remotesigned -scope currentuser
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
# 搜索 conemu
$ scoop search conemu
```



此时返回结果



```bash
# 返回结果
Results from other known buckets...
(add them using 'scoop bucket add <name>')

    bucket/conemu-color-themes
    bucket/conemu
```



在安装前，要运行如下命令（感觉是第一次，需要初始化）（[参考](https://github.com/lukesampson/scoop/wiki/Buckets#installing-from-other-buckets)）



```powershell
$ scoop bucket add extras
```



再安装



```powershell
$ scoop install conemu
```




>P.S. 如果安装过程中有错误，需要手动运行命令卸载未安装成功的包。命令如下：
>```powershell
>$ scoop uninstall <包名>
>```
>卸载后再重新安装

## 安装 oh-my-posh



运行如下命令安装。如果有选项，输入 **Y** 即可



```powershell
$ Install-Module posh-git -Scope CurrentUser
$ Install-Module oh-my-posh -Scope CurrentUser
```



更新配置文件



```powershell
$ $PROFILE
```



继续输入



```powershell
$ if (!(Test-Path -Path $PROFILE )) { New-Item -Type File -Path $PROFILE -Force }
$ notepad $PROFILE
```



在打开的文件中添加：



```powershell
Import-Module posh-git
Import-Module oh-my-posh
Set-Theme Avit
```



将配置刷新到终端



```powershell
# 将用户名部分改为你电脑的用户名即可
$ C:\Users\<用户名>\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
```



## 自定义主题颜色



参考[这篇博文](https://dev.to/teckert/roll-your-own-color-scheme-in-windows-terminal-466b)

