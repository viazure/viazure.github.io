---
title: 使用 Travis CI 自动部署 Hexo 博客
tags:
  - Blog
  - Hexo
  - Travis CI
date: 2019-05-26 14:14:34
---

## 前言

前段时间折腾了下手机写博客，但是 Termux 不是太稳定重装了两次，导致博客所需要的环境也跟着重装了两次。在重装 Hexo 时突然又遇到一些莫名奇妙的问题，不想把时间浪费在这上面了，恰好之前看到过 Travis CI 可以自动部署博客的相关文章，所以这次就试着折腾了一下。
<!--more-->
## 正文

### 申请 GitHub Access Token

在使用 Travis CI 之前，需要申请一个 Github token ，方便之后使用。打开 GitHub 主页，账号 -> Developer Settings -> Personal access tokens -> Generate new token
按照提示输入密码，然后会跳转到创建 token 的界面，在 Note 取一个名字，勾选 repo 下的所有权限。
![UTOOLS1558777004909.png](https://i.loli.net/2019/05/25/5ce90cad642f590717.png)
生成完成后，将该 token 拷贝下来，这个 token 页面只会出现一次，忘了就只能重新申请了。

### Travis CI 设置

首先进入 Travis CI 官网 https://travis-ci.org/ ，点击右上角的 Sign in with GitHub 的按钮，使用自己的 Github 账号登录，登录成功后，在主页面勾选上你需要启用的博客项目。
![UTOOLS1558775202523.png](https://i.loli.net/2019/05/25/5ce905a2e36c495485.png)

点击 Setings 按钮进入设置界面， `More option -> Settings`
![UTOOLS1558776219709.png](https://i.loli.net/2019/05/25/5ce9099bef00187419.png)

General 选项卡和 Auto Cancellation 选项卡中保持默认选项即可，在 Environment Variables 选项卡中的 value 文本框中填入刚刚申请的 token，在 name 文本框中填入一个名称，这个名称我们在写 Travis CI 的配置文件时会用到。
> 这里可以看到我已经填过一个名为 `gh_token` 的密钥了，下面我就会用到`gh_token` 这个名称。

### 编辑配置文件

在 hexo 博客源代码路径的根目录下创建 `.travis.yml` 配置文件，我的配置文件如下：

```yaml
# 配置语言及相应版本
language: node_js
node_js: stable

# 只监听项目源代码所在分支的变动，我这里的源代码分支为 `source`
branches:
  only:
    - source

# 缓存依赖，节省持续集成时间
cache:
  directories:
    - node_modules

before_install:
  # 安装hexo环境
  - npm install -g hexo-cli

install:
  # 安装hexo及插件
  - npm install
  - npm install hexo-deployer-git --save

after_script:
  # 设置git提交名，邮箱
  - git config user.name "viazure"
  - git config user.email "vicrease@gmail.com"
  # 替换同目录下的 _config.yml 文件中 gh_token 字符串为 travis 后台刚才配置的变量名称，注意此处sed命令用了双引号。单引号无效！
  - sed -i "s/gh_token/${gh_token}/g" ./_config.yml
  # 使用 hexo 自带部署命令，可以少写几行提交静态文件的命令，提交的日志还自带文件更新时间，美滋滋~
  - hexo d

script:
  # 执行清缓存，生成静态文件操作
  - hexo clean
  - hexo g

```

接下来再打开同一路径下的 Hexo 的配置文件 `_config.yml` ，修改其中的 `deploy` 节点的 `repository` 选项，这里的 `gh_token` 就是刚刚在 Travis CI 里设置的名称

```yaml
deploy:
  type: git
  # 下方的 gh_token 会被 .travis.yml 中 sed 命令替换
  repository: https://gh_token@github.com/viazure/viazure.github.io.git
  branch: master
```

最后将修改后的两个 yml 文件提交到博客的源代码分支，Travis CI 就会读取 Hexo 博客源码分支下的 `.travis.yml` 文件，自动帮我们生成并部署网站了。

## 结束

以后每次更新博客，只需要编写 Markdown 文件，放入 `/source/_post/` 文件夹下，再提交到 Github 上就行了。这样我无论是在手机上或是换电脑了，都可以不用装一堆环境依赖，爽快的写写博客了。

## 参考文章

[Hexo遇上Travis-CI：可能是最通俗易懂的自动发布博客图文教程](https://juejin.im/post/5a1fa30c6fb9a045263b5d2a)
[使用Travis CI持续部署Hexo博客](https://www.jianshu.com/p/5691815b81b6)
[使用Travis CI自动部署Hexo博客](https://www.itfanr.cc/2017/08/09/using-travis-ci-automatic-deploy-hexo-blogs/)