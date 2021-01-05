---
title: 【博客】博客搭建
tags: 
  - 博客
  - NexT
  - Hexo
categories:
  - 博客
date: 2020-12-31
---


博客是记录生活，学习的和工作的强大载体，能够帮助你快速的回忆起自己的所整理的知识，所做的笔记。  

我的博客是使用[Hexo](https://hexo.io/zh-cn/index.html)博客框架，[NexT](https://theme-next.js.org/)主题搭建的。  

Hexo是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## Hexo的安装

### 安装前提
安装 Hexo 相当简单，只需要先安装下列应用程序即可：

- Git
- Node.js

如果您的电脑中已经安装上述必备程序，那么恭喜您！你可以直接前往 安装 Hexo 步骤。

如果您的电脑中尚未安装所需要的程序，请根据以下安装指示完成安装。

### 安装Git

- Windows：下载并安装[git](https://git-scm.com/download/win).
- Mac：使用[Homebrew](https://brew.sh/), [MacPorts](https://www.macports.org/) 或者下载[安装程序](http://sourceforge.net/projects/git-osx-installer/)。
- Linux (Ubuntu, Debian)：sudo apt-get install git-core
- Linux (Fedora, Red Hat, CentOS)：sudo yum install git-core

### 安装Node.js
Node.js 为大多数平台提供了官方的[安装程序](https://nodejs.org/en/download/)。对于中国大陆地区用户，可以前往[淘宝 Node.js](https://npm.taobao.org/mirrors/node)镜像 下载。

[Installing Node.js via package manager](https://nodejs.org/en/download/package-manager/)
```
sudo pacman -S nodejs npm
```

其它的安装方法：

- Windows：通过[nvs](https://github.com/jasongin/nvs/)（推荐）或者[nvm](https://github.com/nvm-sh/nvm) 安装。
- Mac：使用[Homebrew](https://brew.sh/)或[MacPorts](http://www.macports.org/)安装。
- Linux（DEB/RPM-based）：从[NodeSource](https://github.com/nodesource/distributions) 安装。

### 安装Hexo

所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。
```
npm install -g hexo-cli # 注意权限的限制，安装不上使用sudo
```

### 进阶安装和使用
对于熟悉 npm 的进阶用户，可以仅局部安装 hexo 包。
```
npm install hexo
```

在终端中输入`hexo version`，若输出版本序列则安装成功。

## Hexo的使用

初次使用Hexo，先要把和hexo环境下载到本地，使用一下命令：
```
hexo init           #在当前文件夹下载
hexo init <Path>    #指定目录下载
```
命令执行后，会发现有许多的文件，在指定的目录下。输入网址[http://localhost:4000](http://localhost:4000)可以查看到博客。

## 使用NexT主题

[NexT官网](https://theme-next.js.org/)

### 下载NexT主题到本地
将next克隆到theme目录下的next目录中：
```
git clone https://github.com/next-theme/hexo-theme-next themes/next
```
### 启用NexT

进入到博客目录下，找到_config.yml文件，打开后找到`theme`选项，将其值设定为next。
```
hexo s
```
如此，就可以预览到了当前Next主题了。

## 部署网站

### 安装 hexo-deployer-git

输入命令安装hexo-deployer-git
```
npm install hexo-deployer-git --save
```

### 修改配置  
在_config.yml文件中的deploy参数格式：
```
deploy:
  type: git
  repo: <repository url>
  branch: [branch]
  message: [message]
```
参考如下：
```
deploy:
  type: git # 部署工具种类
  repo: https://gitee.com/foryoungyu/foryoungyu.git # 远程仓库地址
  branch: master # 分支
```
之后执行
```
hexo clean 
hexo g 
hexo d
```
