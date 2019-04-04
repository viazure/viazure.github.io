---
title: 实现在 Andriod 手机上写 Hexo 博客
date: 2019-04-04 18:56:07
tags: 
  - Blog
  - Hexo
  - Andriod
---

由于上班时间一直盯着电脑，下班后便不想再碰它，于是便萌生了用手机写博客的想法。捣鼓了两天，算是比较优雅的实现了，这里分享一下。

<!--more-->

> 本博客采用 Hexo ＋ GitPage 的方式部署和发布，全文由手机编辑，内容基本上都是文字，我尽量表达的清楚易懂。 

> 此文所有的`xxx`代指你的Github名称，请自行替换。


# 准备工作
1. Andriod 系统手机 （需要获取Root权限，MIUI 开发版自带的 Root 也可以）
2. [Root Explorer 文件管理器](https://www.coolapk.com/apk/com.speedsoftware.rootexplorer)（其他能够访问`/data`目录的文件管理器也行）
3. [Termux](https://www.coolapk.com/apk/com.termux) （高级终端模拟器）
4. [MarkdownX](https://www.coolapk.com/apk/com.ryeeeeee.markdownx) （用于编辑Markdown文件的文本编辑器）

> 👆上面提到的软件可以在 Play 商店或[酷安](http://www.coolapk.com)里下载

# 开始
打开 Termux ，安装git
```
apt install git
```

安装nodejs
```
apt install nodejs
```

安装 Hexo
```
npm install hexo-cli -g
```

使用 git 克隆博客的源文件到手机
```
git https://github.com/xxx/xxx.github.io.git
```

**这里需要事先将 Hexo 的源文件备份到 Github 上，我这里采用的是 Github分支备份的方式，具体请搜索“Hexo 备份”，网上很多详细的教程，这里就不再赘述了。**

此时打开文件管理器，博客源文件已经下载到 ` /data/data/com.termux/files/home/xxx.github.io` 目录，而你的文章 Markdown 源文件在 `/data/data/com.termux/files/home/xxx.github.io/source/_posts`目录里。由于权限原因，无法直接在当前文件夹里打开 Markdown 文件，需要先将文件移动到手机的存储区域，然后就能使用 MarkdownX 或其他的文本编辑器编辑文件了，编辑完成后再将文件移动回之前的`_posts`目录里。然后再打开 Termux，使用命令进入到博客源文件所在目录
```
cd xxx.github.io/
```
最后使用 Hexo 命令发布博客
```
hexo g -d
```

完事儿！

# 踩过的坑

## npm 命令无法使用

我在安装完 nodejs 后，发现 npm 无法正常使用，提示
```
Bad system call
```


在酷安和一个老哥交流了半天，最终在 termux-packges 的一个 [issues](https://github.com/termux/termux-packages/issues/3608)
 找到了解决方案。具体如下：

打开 Root Explorer ，访问目录` /data/data/com.termux/files/usr/etc/apt` ，找到文件
` sources.list`，将里面的代码
```
deb https://termux.net stable main
```

改为
```
deb https://dl.bintray.com/termux/termux-packages-24 stable main
```

再次打开Termux，输入命令
```
apt update
```

接着再重新安装 nodejs 即可
```
apt install nodejs
```

## 源文件提交到Github时提示无权限

在编辑完 Markdown 文件移动回`_posts`目录后，需要授予读写权限，否则无法提交。建议将**用户组**和**其他**的读、写权限都勾上。