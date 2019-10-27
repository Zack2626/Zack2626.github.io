---
title: 记录一次迁移hexo遇到的问题
date: 2019-10-27 23:43:47
tags:
- hexo
categories:
- 建站
---

电脑重装了系统，之前的软件都没有了，所以需要重新部署hexo。但是在部署的过程中遇到了一些问题。

<!-- more -->

## 遇到的问题

部署时，按照[hexo.io](https://hexo.io/zh-cn/docs/)的教程，安装完git和node.js后，用语句`npm install -g hexo-cli`安装了hexo。但是部署了出现了错误，deploy后网站首页变成了错误文字。

## 怀疑问题与解决方法

在解决这个问题时发现，有hexo和hexo-cli两个库，hexo自带deploy工具，而hexo-cli还需要安装`hexo-deployer-git`库来实现通过git部署网站。  
最终，安装hexo后，解决了部署出错的问题。
**这只是一种猜测，具体原因还有待研究**