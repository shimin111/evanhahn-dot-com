---
title: 以最简单的方式创建个人博客网站(2)
layout: post
permalink: /how-to-build-a-blog-website-ealiy-(2)/
---
在上一篇博文中，我参考博客[Bebop.C](http://playingfingers.com/2016/03/26/build-a-blog/#jekyll)的教程，结合自己的实际，快速地在Github Pages上部署了自己的博客网站。那么在这一篇博文中，我将讲解如何实现提交博客。

# 撰写博客

## 本地仓库

首先你需要在一个合适的地方创建一个文件夹myblog（比如D:\myblog)，然后在该文件夹内鼠标右键，点击Git Bash Here,
![Git Bash Here](/uploads/20160814124205.jpg)
此时输入命令clone（克隆）自己的远程仓库

    $ git clone https://github.com/username/username.github.io.git //用自己的用户名替换username

完成后，如下图所示：
![complete image](/uploads/20160814130352.jpg)
这时，所有的源码都已经拷贝到D:\myblog\username.git.io这个文件夹内，也就是本地仓库

## 撰写博文

本地仓库的_post文件夹内是所有正式博文的位置，撰写的博文要放入这个文件夹内。撰写博文可以使用Markdown或者Textile语言来写文章。比如使用Markdown语言写文章后缀就是`.md`，当你熟悉这样的标记语言后，可以使用稍微加强的文本编辑器来写文章，比如`sublime`。此外需要注意一点的是，文件的命令需要严格遵守`[年-月-日-文章标题.文档格式]`，要注意月份和日期是两位数。

关于Markdown的语法可以参考[认识与入门Markdown](http://sspai.com/25137)

## 同步远程仓库

当修改了本地仓库的文件后，需要同步到Github上的远程仓库中，同步完成后等待些许时间访问`https://username.git.io`就可以看到修改效果。

在(`D:\myblog\username.git.io`)文件夹下打开Git Bash(参考上面的方法)，依次输入下面的命令：

    $ git add .
    $ git commit -m "stetement" //填写修改的内容，方便以后查阅
    $ git push origin master

这样子浏览自己的主页就可以查看提交新文章或者修改后的效果了。

# 进阶：安装Jekyll本地编译环境

每次修改文件，需要使用三个Git命令和服务器的延迟刷新才能看到修改效果，的确不方便。如果安装了Jekyll本地编译环境，就可以很方便的在浏览修改效果。

## 环境安装

> 本文主要介绍的是Windows下的安装方法，其他可以自行搜索教程。

1. 安装[Ruby](http://rubyinstaller.org/downloads/)

根据需要选择64位或者32位的安装包，并且在安装过程中注意勾选添加环境变量到PATH。
![add path](/uploads/20160326174117476.jpg)  
2. 安装[RubyGems](https://rubygems.org/pages/download)

建议下载ZIP格式，解压到合适的文件夹中，在文件夹中打开cmd(shift+鼠标右键，在此处打开命令窗口)，输入命令：

    $ ruby setup.rb

等待安装完毕就可以了。  
3. 安装[RubyGems](https://rubygems.org/pages/download)

建议下载ZIP格式，解压到合适的文件夹中，在文件夹中打开cmd(shift+鼠标右键，在此处打开命令窗口)，输入命令：

    $ ruby setup.rb

等待安装完毕就可以了。  
4. 安装jekyll-paginate

在cmd终端里输入

```
$ gem install jekyll-paginate
````

## 开启本地预览

完成了上面的工作后，在(`D:\myblog\username.git.io\`)文件夹下打开cmd命令行(shift+鼠标右键)，输入命令：

```
$ jekyll serve
```

运行正常的话就可以访问[localhost:4000](http://localhost:4000)。

注意，在我实际运行中遇到一个问题，需要修改本地仓库文件夹下的`_config.yml`文件，主要是将markdown一行改为如下所示：
![_config.yml](/uploads/20160814141619.jpg)
这主要是因为redcarpet引擎不再被支持，因此需要更换。

至此，建站和提交博客到这里就结束了，想要了解更多的知识并准备自定义的朋友可以看一下[jekyll的中文网站](http://jekyll.bootcss.com/).