---
title: 以最简单的方式创建个人博客网站(1)
layout: post
permalink: /how-to-build-a-blog-website-ealiy-(1)/
---
首先，我必须要说明一下，一直以来，我都想创建一个个人博客网站，但限于自己知识的缺乏和懒惰心理，一直都没有开始。暑假的时候，正好很闲，思前想后，还是决定动手。

这篇博文将以一个全局的方面来介绍如何以最简单的方式创建属于自己个人的博客网站。我采用的是Githup Pages + Jekyll的方式搭建网站。在搭建过程中，我使用了[Bebop.C](http://playingfingers.com/2016/03/26/build-a-blog/#jekyll)的教程，并且结合自己的实际，完成了博客网站的搭建。

注意：Github Pages + Jekyll的优点如下：

> Github免费托管源文件，自动编译符合Jekyll规范的网站。  
> 引入版本管理，修改网站更加方便。  
> 支持[Markedown](http://sspai.com/25137)，编写具有优美排版的文章。

Github Pages + Jekyll的缺点如下：

> 对于Windows用户来说，有一些不便，需要安装Git，并掌握一些Git命令。  
> 自己搭建网站需要了解Jekyll的文档并熟悉前端开发。幸运的是有很多前辈的工作可以借鉴，我们可以在他们的基础上修改。  
> 生成的是静态网页，不支持动态功能。若使用动态功能，需要第三方服务支持。

综合以上，如果自己仅仅是搭建一个博客网站，分享自己的心得，不需要支持一些动态功能，这个方案是非常不错的。

下面我按照最基本的方式搭建博客网站。

# 注册个Github账户
首先注册[Github](https://github.com/join?source=header-home)账户，如果已经有账户，则可以忽略这个步骤。

# 安装Git

## 安装git for windows
由于大家普遍使用Windows操作系统，所以本文介绍的是Windows下的Git安装方法，其他操作系统安装方法同样简单，可以自行搜索。

先下载[git for windows](https://git-for-windows.github.io/)，并安装。

安装好以后，在开始菜单中打开Git Bash。
![Git bash](/uploads/20160812132412.png)
在打开的bash命令行中执行以下命令，设置个git用户名和邮箱：

    $ git config --global user.name"{username}" //用自己的用户名替换{username}
    $ git config --global user.email"{name@site.com}" //用自己的邮箱替换{name@site.com}

## SSH设置
为了和Github的远程仓库进行传输，需要SSH加密设置。在打开的bash命令行中执行：

    $ ssh-keygen -t rsa -C"{name@site.com}" //用自己的邮箱替换{name@site.com}

一直敲回车直至命令完成。这时你的用户目录(`C:\Users\你的用户名`)会出现.ssh的文件夹，里面包括两个文件`id_rsa`和`id_rsa.pub`两个文件，前者是你的私钥，后者是你的公钥。公钥可以公开，私钥不能公开。用记事本或者其他的文本编辑器打开公钥文件，复制其中的所有内容，在下面的步骤中有用。

在浏览器中登陆[Github](https://github.com/)，点击右上角的设置setting。

![click setting](/uploads/20160812135819.png)

点击"SSH and GPG keys",随后点击"SSH keys"中右侧"New SSH key"。

![SSH and GPG keys](/uploads/20160812140054.png)

将前面公钥的内容复制到SSH的key里去，title取一个合适的名字即可。

![enter the ssh keys](/uploads/20160812140628.png)

# 建立个人的Github Pages

在建立Github Pages + Jekyll的个人博客网站时，有两条路线：

1. 学习[Jekyll](http://jekyll.bootcss.com/)的教程，当然要求自己掌握一定的前端知识，DIY。
2. 在Github上有许多开源的基于Jekyll的开源博客仓库，你可以挑选自己喜欢的直接fork一份保存在自己的仓库里，在此基础上进行创作。

我就是选择第二条路线，这样子网站可以很快上线。同时我是fork了[Evan hahn](http://www.evanhahn.com)的网站[源码](https://github.com/EvanHahn/evanhahn-dot-com)。在[Jekyll 项目的wiki页面](https://github.com/jekyll/jekyll/wiki/Sites)的网站中也提供了不少风格的网站，可以选择合适的source(源码)fork到自己的仓库中。

下面以Tom Preston-Werner为例，点击source链接，进入作者的模板仓库。

![source](/uploads/20160812143157.png)

点击右上角的fork

![fork](/uploads/20160812143612.png)

随后在自己的个人主页中找到刚才fork的项目，点击进去,并点击设置。

![setting](/uploads/20160812144301.png)

将"setting"里的"Repository name"中改为`{你的github用户名}.github.io`，点击"Rename"。

![rename](/uploads/20160812144502.png)

至此，你就可以在浏览器里访问`http://{你的用户名}.github.io`这个网址了，个人博客网站也就搭建起来了。当然下一步还需要提交博客，修改显示等等，都留在下一篇博客中讲吧。