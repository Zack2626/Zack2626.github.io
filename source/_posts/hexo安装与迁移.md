---
title: hexo安装与迁移
date: 2019-10-27 23:43:47
tags:
- hexo
categories:
- 建站
---

记录一下hexo安装与迁移过程中遇到的坑。

<!-- more -->

# 1. hexo的基础用法
### 安装
安装hexo，参考[hexo官网](https://hexo.io/zh-cn/docs/#%E5%AE%89%E8%A3%85-Hexo)
在 `_config.yml` 中添加deploy参数：

```json
deploy:
	type:git
	repo:
		github1: [url1],[branch]
		github2: [url2],[branch] 
```

### 使用

```bash
hexo init             #在当前文件夹下面创建hexo网站
hexo server -p xxxx   #创建本地服务器
hexo generate         #生成hexo网页内容
hexo deploy           #将当前的内容发布到_config.yml中的delpoy网站
hexo g -d             #生成并发布
hexo d -g             #生成并发布
hexo new [title]      #新建一篇名为title的文章
```


# 2. hexo与hexo-cli的区别
### 部署时的问题
部署时，按照[hexo.io](https://hexo.io/zh-cn/docs/)的教程，安装完git和node.js后，用语句`npm install -g hexo-cli`安装了hexo。但是部署了出现了错误，deploy后网站首页变成了错误文字。
### 解决方法
在解决这个问题时发现，有hexo和hexo-cli两个库，hexo自带deploy工具，而hexo-cli还需要安装`hexo-deployer-git`库来实现通过git部署网站。
最终，安装hexo后，解决了部署出错的问题。

**这只是一种猜测，具体原因还有待研究**
**
# 3. 新安装hexo的注意事项
在电脑上安装好hexo后，需要到hexo的文件夹下面，执行 `npm install` 的命令。这样，npm会根据文件夹下面package.json文件下的内容，将需要的hexo等库安装到文件夹中。



