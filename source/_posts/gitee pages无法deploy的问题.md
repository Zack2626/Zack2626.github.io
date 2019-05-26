---
title: gitee pages无法deploy的问题
date: 2019-05-26 12:51:34
tags: 
- gitee 
- hexo
categories:
- 建站
---


目前hexo主要的用途在于做了一个静态页面个手机浏览器做导航。因而对于访问的速度要求比较高。手机打开github pages时会比较慢，因而在gitee上也部署了hexo。  
然而在电脑上通过`hexo deploy`命令部署网页时，github pages能顺利更新，gitee pages并没有更新。 

<!-- more -->

## 创建过程

1. 在gitee上导入github仓库
2. 设定pages的分支
3. 本地git增加gitee的远程仓库
```
git add remote [remote name] [remote url]
```
_只是部署的话并不需要添加远程仓库_  
4. 在_config.yml中添加deploy信息
```
deploy:  
type: git
repo:
    [name1]: [url],[branch]
    [name2]: [url],[branch]
```

## 出现的问题

deploy后网页不更新
- github pages页面
![github pages](/images/github.png)
- gitee pages页面
![gitee pages](/images/gitee.png)

添加新文章后，也不会增加新文章。但是查看仓库，发现仓库已经更新，只是网页没有重新部署。

## 尝试解决

在` gitee——服务——Gitee Pages`中选择`重新部署`，网页更新。

## 后记
发现gitee pages和github不同，自动部署需要Gitee Pages Pro.
![gitee pages pro](/images/gitee_pages_pro.png)







