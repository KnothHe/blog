---
title: 使用 Hexo + GitHub 搭建博客
date: 2019-09-23 09:14:57
updated: 2019-09-28 21:40:15
tags:
    - Hexo
categories:
    - Tools
---

因为不止有一个同学问过我如何借助 GitHub 搭建博客，是决定把搭建博客的整个流程记录下来，
以供想要自己借助 Hexo 和 GitHub 搭建博客的同学参考。
我主要使用 Hexo 生成静态站点文件，然后　push 到 GitHub 上借助 GitHub Pages 展示静态博客。

<!-- more -->

## 安装 Hexo

在安装 `Hexo` 之前，首先需要先安装 `Node.js` 和 `Git`。

1. 安装 `Node.js` 和　`Npm`

- 最简单的方式就是到 [Node.js download page](https://nodejs.org/en/download/) 直接下载安装。

2. 安装并配置 Git

- 安装 Git

    到 [Git Download page](https://git-scm.com/downloads) 根据所用的操作系统下载并安装 Git

- 配置 Git

    在使用 Git 之前，需要先配置 Git 的用户名和用户邮箱

    ```sh
    git config --global user.name "your name"
    git config --global user.email youremail@example.com
    ```

3. 安装 Hexo

    使用 Npm 安装 Hexo。

    ```sh
    npm install hexo-cli -g
    ```

## 使用 Hexo

使用下面的命令初始化目标文件夹，所有需要的文件都会被下载到该文件夹下。
这个过程需要一段时间。

```sh
hexo init <folder>
cd <folder>
npm install
```

初始化之后就可使用如下命令预览，默认已经有一篇 Hello World 文档用于预览效果的展示。

```sh
hexo s
```

可以看到命令行有提示，Hexo 运行在地址 <http://localhost:4000>，通过浏览器打开该网址即可看到预览结果。

## 博客编写

所有的文档的编写都需要放在 `source/_posts` 目录下，初始化后可以看到该目录下有一个 `hello-world.md`，同样，我们需要写的文档也类似。

简单使用 [Markdown 的指导](https://www.markdownguide.org/basic-syntax)

## 发布

1. 在你的 GitHub 账户下创建一个仓库用于存放 Hexo 生成的静态文件。GitHub Page 会根据仓库名给定一个对应的域名。假定用户名为 username，规则如下:

    1. 如果仓库名为 `username.github.io`，则对应的域名为 `username.github.io`
    2. 如果是其他名字，如 `theBlogRepository`，则对应的域名为 `username.github.io/theblogrepository`

    比如，我的 GitHub 用户名为 [knothhe](https://github.com/KnothHe)，我存放博客的仓库名为 `blog`，我的博客地址就是 <https://knothhe.github.io/blog>。

    最后在该仓库的设置页面需要开启 GitHub Page 的选项，默认有 master 分支和 master 分支下的 /docs 文件夹。选择 master 分支即可。

2. 配置 `_config.yml`

    - 可以看到该文件默认有 deploy 小节，由于是发布到 GitHub，那么按下面配置编写即可:

    ```yml
    deploy:
        type: git
        repo:
            github: git@github.com:yourGitHubUsername/theBlogRepository.git
        branch: master
    ```

3. 安装 Hexo 使用 Git 发布的插件

    ```sh
    npm install hexo-deployer-git --save
    ```

4. 使用下面的命令发布到你的 GitHub 仓库

    ```sh
    hexo d g
    ```

## 其他

详细的使用和指导可以参考 [Hexo 的文档](https://hexo.io/docs/)。

已经有很多人写过类似的文章了，通过 google 或者 baidu 搜索下面的关键词即可得到很多的详细教程。

搜索关键字: `hexo github 个人博客`

## 参考文献

1. `Git` 官网的[配置指导](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)
2. `Git` 官网的[安装指导](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
3. `Npm` 官网的[安装指导](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm#os-x-or-windows-node-installers)
4. `Hexo` 官网的[安装指导](https://hexo.io/docs/index.html)
5. `Hexo` 官网的[使用指导](https://hexo.io/docs/setup)
