---
title: "第一篇博客，兼记录部分搭建过程"
subtitle: ""
tags: ["Hugo"]
categories: ["建站笔记"]
date: 2021-12-29T22:01:26+08:00
---

![eris](/~taoyang_2002/blog/public/img/blog/eris1.jpg)

**首先的首先：**
Hugo永远滴神，比Hexo高到不知道哪里去了（引战打死


<!--more-->
## 为什么要建立这个博客

一年前就知道科大有个FTP服务器可以用了，可是因为知识原因，不了解HTML和CSS等网络相关的语言，所以一直没有动手
直到最近看了点相关书籍，觉得自己可以了，就动手搭建了现在这个博客。虽然说技术含量那是低低的，不过不耽误这个博客确实挺好康ww

至于主观能动性啥的，果然有个博客才有互联网从业人员内味吧（逃
而且建站的过程确实挺有意思，不算上Hexo的折磨时光的话

## 之后会发什么内容

因为有计划之后使用Markdown语言来做笔记，所以之后可能会把markdown的笔记分享到网站上
以及老本行不能丢，会发一些关于ACG的个人感想以及杂谈

## 技术记叙
这个模块大概记录我建立博客的一些过程，供有兴趣的小伙伴参考
### 博客环境

因为科大自带ftp服务器，所以只需要选择博客的框架即可
我一开始选择的是Hexo，但因为在使用Hexo的过程中从安装NodeJS开始就频繁出现奇怪的提示和错误，直到后面实在受不了了连夜卸载转用Hugo

#### Hugo安装

首先是Hugo的安装，在linux下直接利用apt包管理器即可
``` Bash
sudo apt install hugo
```

#### Git与Git-ftp的安装

在这之后为了能够自动部署到科大的ftp服务器，查阅资料后选择使用git-ftp
需要提醒的是如果没有git则需要先安装git
``` Bash
sudo apt install git
sudo apt install git-ftp
```
git设置相关搜索引擎上到处都是，不再赘述，具体说明git-ftp的设置
在博客文件夹中启动命令行，使用git命令来添加设置
``` Bash
git config git-ftp.url ftp.example.net
git config git-ftp.user ftp-user
git config git-ftp.password secr3t
git config git-ftp.syncroot path/dir
git config git-ftp.remote-root htdocs
```
其中每个命令的最后一个为需要更换的选项
* `ftp.example.net`
    主机地址，ftp://home.ustc.edu.cn
* `ftp-user`
    用户名，科大邮箱在@mail.ustc.edu.cn前面的部分
* `secr3t`
    邮箱密码
* `path/dir`
    本地需要同步的目录，一般来说Hugo生成的静态网页在根目录的`/public`文件夹，则设为`./public/`即可
* `htdocs`
    远程目录，上传到ftp服务器的哪个文件夹。需要注意的是科大ftp服务器貌似需要把文件放在根目录的`/public_html/`下才能访问。


#### Git-ftp的使用

第一次要先`git ftp init`
之后更新在`git commit`后启动`git ftp push`即可
```bash
git add ./public/
git commit -m "update"
git ftp push
```

### 博客设置
目前没有增加很多插件，最主要还是普通的网页排版
不过有用到一个小方法来帮助网页排版，就是使用到Hugo的shortcodes的特性
首先Hugo解析你的markdown文件的时候是不支持html解析的，也就是说你的所有html语法全部都木大了
所以为了能够在md文件中使用html语法，在根目录的新建如下文件`./layouts/shortcodes/raw.html`并在里面添加下面语块即可
```html
{{ .Inner }}
```
使用方法就是在markdown需要用到html的地方的前后加上（为防止触发语法加入了"\",使用时请把"\"去掉
```markdown
\{\{<raw>\}\}

<!-- 你的html语法 -->

\{\{</raw>\}\}
```
原理猜测大概就是把包含起来的部分直接插入最后生成的html文件中

关于ACG两个子网站的风格有参考[YuC's AnimeList](URL 'https://yuc.wiki/')，之后手搓（以及复制）css文件，得到现在的样子

## 总结
摸了没有总结，整了好几天了才搞成现在这个样子，直接进行一个鱼的摸