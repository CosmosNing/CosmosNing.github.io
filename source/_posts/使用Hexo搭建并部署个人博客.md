---
title: 使用Hexo搭建并部署个人博客
toc: true
mathjax: false
date: 2020-03-12 23:15:36
description: 手把手教你从零开始搭建一个个人博客
tags:
- 前端
- Hexo
- GitHubPages
- TravisCI
categories:
- 指南
---

>前段时间，看到 Hexo 官方文档更新了使用 TravisCI 自动化部署部分。趁着这个机会，我将我的博客重新构建了一遍。这里记下简略过程，供读者参考。通过本篇文章，你将获得：
>
>1. 使用 Hexo 构建静态博客
>2. 简单配置 NexT 主题
>3. 使用 TravisCI + GitHubPages 完成自动化部署

# 前置需求

在安装 Hexo 之前，你必须保证你的机器上安装了**Node.js**及**Git**。如果你未安装，你需要到如下网址，下载安装。

* Node.js：https://nodejs.org/en/download/
* Git：https://git-scm.com/downloads

然后，你可以在命令行中输入如下命令，完成 **Hexo** 的安装

```powershell
npm install -g hexo-cli
```

此外，为了部署在 Github 上，你需要有一个 **GitHub 账号**。如果没有，你可以到[GitHub](https://github.com/)注册一个。

# Hexo 最小配置

打开命令行（Windows 上可以使用 **Git Bash** 或者 **PowerShell**），输入如下命令：

```powershell
hexo init blog
```

等待片刻，**Hexo** 会帮我们做初始化网站的工作。

![Hexo初始化](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/hexo-init-blog.PNG)

为方便后面部署，我们在 `blog` 中初始化一个 git 仓库，并配置好 git 相关信息。

```powershell
# 进入 blog 文件夹
cd blog
# 初始化 Git 仓库
git init
# 查看 Git 状态
git status
# 配置个人信息（与你注册 GitHub 的用户名、邮箱一致）
git config --local user.name <你的用户名>
git config --local user.email <你的邮箱名>
# 查看配置，user.name、user.email 是否配置成功
git config --local --list
```

![初始化Git仓库](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/git-init-and-config.PNG)

接着，将仓库里的已有改动添加、提交。

```powershell
git add .
git commit -m "init with hexo-cli"
```

![Git提交改动](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/git-add-and-commit.PNG)

在 blog 根目录，使用任意文本编辑器打开 `_config.yml` 文件，并在 `#Site` 部分输入你网站的信息。一个示例如下

```yaml
# Site
title: CosmosNing的个人博客
subtitle: 探索·好奇
description: 编程·学习·生活
keywords: 个人博客;编程;学习;生活
author: CosmosNing
language: zh-CN
timezone: Asia/Shanghai
```

到现在， Hexo 最小配置已经完成。我们来看看效果如何。继续在命令行下输入如下命令

```powershell
# 如果使用 PowerShell，则选择输入下一条命令（下同）
hexo g; hexo s
# 如果使用 Git Bash 或者 Linux 终端，则选择输入下一条命令（下同）
hexo g && hexo s
```

![Hexo本地预览命令](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/hexo-g-and-hexo-s.PNG)

打开浏览器，访问 http://localhost:4000/

你应该能得到类似这个画面

![Hexo最小化配置效果](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/basic-result.PNG)

{% note info %}
## 提示-终止预览
你可以在命令行中，按下 `Ctrl + C` 终止上述预览命令
{% endnote %}

# 设置 NexT 主题

默认的主题不太美观，而且功能有限。下面，我将介绍目前 **Hexo** 中最受欢迎的主题——NexT 的安装与配置。

## 最小配置

首先，在命令行中输入如下命令，安装 **NexT** 主题

```powershell
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

使用文本编辑器，打开项目根目录的 `_config.yml` 文件，将 `theme` 配置成 `next`

```yaml
theme: next
```

至此，**NexT** 最小配置已经完成。我们来看看效果吧

```powershell
hexo clean; hexo g; hexo s
```

![Hexo本地预览](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/next-hexo-g.PNG)

打开浏览器，访问 http://localhost:4000/

你应该能得到类似这个画面

![next最小配置效果](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/next-basic-result.PNG)

使用 `Ctrl + C` 终止上述命令。接下来，我们将这一阶段的改动提交到 Git 仓库。

```powershell
# 将 NexT 主题作为子模块加入仓库
git submodule add https://github.com/theme-next/hexo-theme-next themes/next
# 添加改动至暂存区
git add .
# 提交
git commit -m "add common site config with theme next(default config)"
```

![NexT最小化配置提交](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/git-commit-next-default.PNG)

## 配置功能页面及资源文件

一个完整的博客应该包括 `主页` 、`分类`、`标签`、 `归档` 等功能页面。而在 **Hexo** 帮我们初始化时，只是完成了博文页的设置。其他功能页需要手动设置。具体步骤如下

* 创建 `关于` 页

在项目根目录下的 `source` 文件夹中，创建一个 `about` 文件夹。在 `about` 文件夹下，创建 `index.md` ，并在 `index.md` 中输入如下文字：

```markdown
---
title: 关于我
---

<在这里输入你的个人介绍>
```

* 创建 `分类` 页

在项目根目录下的 `source` 文件夹中，创建一个 `categories` 文件夹。在 `categories` 文件夹下，创建 `index.md` ，并在 `index.md` 中输入如下文字：

```markdown
---
title: 分类
type: "categories"
comments: false
---
```

* 创建 `标签云` 页

在项目根目录下的 `source` 文件夹中，创建一个 `tags` 文件夹。在 `tags` 文件夹下，创建 `index.md` ，并在 `index.md` 中输入如下文字：

```markdown
---
title: 标签云
type: "tags"
comments: false
---
```

至此，你可以在 `_config.yml` 配置使用这些页面

```yaml
# 增加主题配置
theme_config:
  menu:
    home: / || home
    about: /about/ || user
    tags: /tags/ || tags
    categories: /categories/ || th
    archives: /archives/ || archive
```

另外，你可以将博客中常用的图片（如 logo 、头像等）内置在博客站点中。你需要在项目根目录下的 `source` 文件夹中，创建 ` images` 文件夹。然后将图片文件拷贝到这里即可。这样，你在 `_config.yml` 中可以通过 `/images/<图片名>` 来引用。

## 安装插件

**Hexo** 开放的设计使得开发者可以通过插件的方式增强其能力。为了使得我们的博客功能更加完善，这里我们需要安装一些有用的插件。

* [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed)

`hexo-generator-feed` 是一个 RSS Feed 生成器，通过它可以为我们的博客生成可用于 RSS 订阅的文件。默认配置下，它会在你的站点根目录生成 `atom.xml` 。

你可以输入如下命令安装插件

```powershell
npm install hexo-generator-feed --save
```

然后，在 `_config.yml` 中配置使用

```yaml
feed:
  type: atom
  path: atom.xml
  limit: 10
  hub:
  content:
  content_limit: 140
  content_limit_delim: ' '
  order_by: -date
  # 这个是自定义的 icon
  icon: /images/avatar-32.ico
```

* [hexo-generator-searchdb](https://github.com/theme-next/hexo-generator-searchdb)

`hexo-generator-searchdb`是一个本地搜索索引生成器，通过它可以为我们的博客生成可用于本地搜索的文件。默认配置下，它会在你的站点根目录生成 `search.xml` 。你不需要额外书写搜索算法，因为强大的 `NexT` 主题已经内置了搜索功能。

你可以输入如下命令安装插件：

```powershell
npm install hexo-generator-searchdb --save
```

然后，在 `_config.yml` 中配置使用：

```yaml
theme_config:
  # 注意需要将下列配置增加在 theme_config 中，并保持相对缩进
  local_search:
    enable: true
    # If auto, trigger search by changing input.
    # If manual, trigger search by pressing enter key or search button.
    trigger: auto
    # Show top n results per article, show all results by setting to -1
    top_n_per_article: 1
    # Unescape html strings to the readable one.
    unescape: false
    # Preload the search data when the page loads.
    preload: true
```

* [hexo-permalink-pinyin](https://github.com/viko16/hexo-permalink-pinyin)

`hexo-permalink-pinyin`是一个中文链接转拼音的工具，使用它有助于搜索引擎的抓取我们的博客页面（当然，如果你部署在 GitHub 上，百度无法抓取，因为 GitHub 禁止百度对此类页面的抓取）。

你可以输入如下命令安装插件：

```powershell
npm i hexo-permalink-pinyin --save
```

然后，在 `_config.yml` 中配置使用：

```yaml
permalink_pinyin:
  enable: true
  separator: '-' # default: '-'
```

* [hexo-symbols-count-time](https://github.com/theme-next/hexo-symbols-count-time)

`hexo-symbols-count-time`是一个统计文章字数、估计阅读时间的工具，使用它有助于读者提高阅读效率。

你可以输入如下命令安装插件：

```powershell
npm install hexo-symbols-count-time --save
```

然后，在 `_config.yml` 中配置使用：

```yaml
theme_config:
  # 注意需要将下列配置增加在 theme_config 中，并保持相对缩进
  symbols_count_time:
    separated_meta: true
    item_text_post: true
    item_text_total: false
    exclude_codeblock: true
    awl: 2
    wpm: 300
```

* [hexo-generator-sitemap](https://github.com/hexojs/hexo-generator-sitemap)

`hexo-generator-sitemap`是一个生成网站 `sitemap` 工具，你可以主动将生成的 `sitemap.xml` 提交给搜索引擎，这样搜索引擎就可以更好的抓取你的博文。

你可以输入如下命令安装插件：

```powershell
npm install hexo-generator-sitemap --save
```

然后，在 `_config.yml` 中配置使用：

```yaml
sitemap:
    path: sitemap.xml
    rel: false
```

至此，插件部分已配置完成。

{% note info %}

## 提示-更多配置

更多 NexT 主题的配置，可以查看 `themes/next/_config.yml`，将你需要改动的配置拷贝到根目录下 `_config.yml` 中的 `theme_config` 结点下。（[参考](https://github.com/theme-next/hexo-theme-next/blob/master/docs/zh-CN/DATA-FILES.md#选择-1hexo-方式)）

如果你懒得自定义，可以参考我的[主题配置](https://github.com/CosmosNing/CosmosNing.github.io/blob/source/_config.yml)

{% endnote %}

我们来看看效果如何。

```powershell
hexo clean; hexo g; hexo s
```

打开浏览器，访问 http://localhost:4000/

我得到了以下画面

![效果预览](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/my-result.PNG)

## 定制主题颜色及简单样式

NexT 主题非常灵活，用户可以高度自定义，包括颜色，布局等。本节将浅显的介绍如何自定义主题的颜色及样式。

{% note default %}

推荐你使用 `Chrome` 浏览器作为调试工具

{% endnote %}

首先，在根目录的 `source` 文件夹下创建 `_data` 文件夹。在 `_data` 文件夹下，创建 `styles.styl` 文件。

然后，配置根目录下 `_config.yml` ，以确保生成器将使用你自定义的样式：

```yaml
theme_config:
  # 注意需要将下列配置增加在 theme_config 中，并保持相对缩进
  custom_file_path:
    style: source/_data/styles.styl
```

接下来，我们就可以使用 `Chrome` 浏览器对主题的样式进行微调。

```powershell
# 本地预览命令
hexo clean; hexo g; hexo s
```

打开浏览器，访问 http://localhost:4000/

按下 `F12` 打开`Chrome` 浏览器开发者工具。并点选左上角红框所示的按钮。这样，你就可以使用鼠标快速找到网页中某块元素的样式。然后修改右半部分（如蓝框所示） `Styles` 的样式，并观察网页效果。如果合适，将对应的样式类，复制到上述创建的 `styles.styl` 文件中，并保存。下次预览生成，你就会得到定制化的主题样式。

![ChromeDevTools](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/ChromeDevTools.PNG)

{% note default %}

点[这里](https://github.com/CosmosNing/CosmosNing.github.io/blob/source/source/_data/styles.styl)，查看我的样式配置。

{% endnote %}

至此，我们的主题配置已经完成。下面，将改动提交至仓库

```powershell
git add .
git commit -m "<你的提交信息>"
```

# 使用 TravisCI + GitHubPages 实现自动化部署

## 前置工作

由于 Github Pages 目前只是支持 master 分支的页面渲染，所以我们需要提前做一些准备工作。打开命令行，输入如下命令

```powershell
# 基于最新提交，创建名为 source 的分支
git branch source
# 切换到 source 分支
git checkout source
# 删除 master ，供静态页面的部署
git branch -d master
```

## 创建 GithubPages 仓库

登录你的 GitHub 账号，在主页的左侧，点击 `New` ，新建一个 GithubPages 仓库。你应该将其命名为 `<你的 GitHub 用户名>.github.io`，并点击 `Create repository` 完成创建。

![创建仓库](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/new-repo.png)

![填写仓库信息](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/new-repo-info.PNG)

接下来，需要将本地的博客仓库，推送到 GitHub。

```powershell
git remote add origin git@github.com:<你的用户名>/<你的用户名>.github.io.git
git push -u origin source
```

## 创建 Personal Access Token

在浏览器新建一个标签页，前往 GitHub [新建 Personal Access Token](https://github.com/settings/tokens)，只勾选 `repo` 的权限并生成一个新的 Token。Token 生成后请**复制并保存好**（因为只会出现一次，刷新页面就再也看不见了）。

![token](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/token.PNG)

![token-info](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/token-info.PNG)

## 安装及配置 TravisCI

在你的 GitHub 主页，点击上方菜单栏中的 `Marketplace`。

![Travis-01](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/travis01.png)

在搜索框中，搜索 `travis`

![Travis-02](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/travis02.PNG)

![Travis-03](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/travis03.PNG)

点选第一个 `Travis CI` ，并滑动到最后，选择 `Open Source`，再点击 `Install it for free`。

![Travis-04](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/travis04.PNG)

![Travis-05](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/travis05.PNG)

继续点 `绿色` 按钮。

![Travis-06](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/travis06.PNG)

`Install` !

![Travis-07](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/travis07.PNG)

现在我们可以在 `Travis CI` 官网上，登录并设置它了。

![Travis-08](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/travis08.PNG)

![Travis-09](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/travis09.PNG)

点击你的博客仓库，新建一个环境变量。**Name** 为 `GH_TOKEN`，**Value** 为刚才你在 GitHub 生成的 Token。确保 **DISPLAY VALUE IN BUILD LOG** 保持 **不被勾选** 避免你的 Token 泄漏。点击 **Add** 保存

![Travis-10](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/travis10.PNG)

在你的 Hexo 站点文件夹中新建一个 `.travis.yml` 文件：

```yaml
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
cache: npm
branches:
  only:
    - source # build source branch only
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  target_branch: master
  on:
    branch: source
  local-dir: public
```

最后，将改动提交，并推送到 GitHub 上:

```powershell
git add .
git commit -m "你的提交信息"
git push
```

你的工作至此完成，可以泡杯咖啡，等待 `Travis CI` 运行结果（在你的 GitHub 博客仓库主页可以查看，如下图。如果是绿色的 ✅，那么成功；❎ 则代表失败）。

![build-status](https://gitee.com/CosmosNing/MyPicGo/raw/master/images/2020/04/build-status.PNG)

# 写作

接下来你就可以自由的写博客啦。这里为你总结一些常用命令

```powershell
# 新建博文
hexo new <标题名>
# 本地预览
hexo clean; hexo g; hexo s
# Git 推送
git add .
git commit -m "你的提交信息"
git push
```



# 参考资料

[Hexo 官方文档](https://hexo.io/zh-cn/docs/)