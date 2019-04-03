---
title: 一行命令为你的 Hexo 博客整一个音乐播放器
date: 2019-04-03 11:5:29
tags: 
  - blog
  - hexo
---
![给我也整一个！](https://wx4.sinaimg.cn/large/0068Lfdegy1fzsb3jvxfdj309q09qmzj.jpg)

为了让我的博客更加丰满，也为了找到音乐口味和我相似的朋友，我给我的博客增加了一个播放音乐的页面。

简单记录一下实现的过程：

为了实现这个需求，我使用了 [hexo-tag-aplayer](https://github.com/MoePlayer/hexo-tag-aplayer)，它是基于[APlayer](https://github.com/MoePlayer/APlayer) 播放器的 Hexo 标签插件。APlayer 是目前广泛使用的 HTML5 音乐播放器，这里暂不深入研究，以后再去折腾。

<!--more-->

## 安装

进入hexo 博客所在目录，使用命令安装 hexo-tag-aplayer

```
npm install --save hexo-tag-aplayer
```
## 使用

hexo-tag-aplayer 现在的版本新增了 MeingJS 的支持，[MetingJS](https://github.com/metowolf/MetingJS) 是基于[Meting API](https://github.com/metowolf/Meting) 的 APlayer 衍生播放器，引入 MetingJS 后，播放器将支持对于 QQ音乐、网易云音乐、虾米、酷狗、百度等平台的音乐播放。

这里我们在 Hexo 配置文件 `_config.yml` 中增加下面的设置：

```yaml
aplayer:
  meting: true
```


最后在我们的文章页面里增加这样一行代码：

```
{% meting "33374262" "netease" "playlist" %}
```

效果如下：
{% meting "33374262" "netease" "playlist" %}

更多用法请参考 hexo-tag-aplayer 的[中文文档](https://github.com/MoePlayer/hexo-tag-aplayer/blob/master/docs/README-zh_cn.md)

好的，完事儿！