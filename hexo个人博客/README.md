# `hexo`搭建博客

使用`hexo`搭建个人博客并托管到`github`上。

步骤：

- 首先要先安装`git`、`node`、`hexo`
- 在`github`上创建一个仓库以 ==[用户名].github.io== 结尾，并设置`GitHub Pages`
- 初始化本地博客站点：`hexo init myBlog`
- 测试本地：`hexo g`生成博客 / `hexo s`本地运行博客将于`localhost:4000`呈现
- 将本地生成的博客上传至`github`，需要使用到`ssh`以及`hexo-deployer-git`

`ssh`的安装，后续补充！

下面命令用来检查`ssh`是否成功安装：

```shell
ssh -T git@github.com;
```

---

安装`hexo-deployer-git`：

```shell
npm install hexo-deployer-git --save;
```

安装完成之后，修改`_config.yml`内的配置信息：

[配置一]：

```
deploy:
    type: git
    repo: [填写ssh链接到的git仓库地址]
    branch: master
```

[配置二]：

```
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://sxw19960127.github.io/lostmyself.github.io/
root: /lostmyself.github.io/
permalink: :year/:month/:day/:title/
permalink_defaults:
```

==注意==：符号`:`后面必须空一格！

---

## `hexo`基本指令

- 创建文章：`hexo new "第n篇文章"`
- 每次同步前的清除缓存：`hexo clean`
- 将`md`文档在本地生成博客：`hexo g`
- 开启本地服务器预览效果：`hexo server`
- 上传`github`：`hexo d`

---

## 关于文章部分

文章归属分类：

```
title: 文章标题
tags: 文章标签
categories: 文章分类
description: " "
```