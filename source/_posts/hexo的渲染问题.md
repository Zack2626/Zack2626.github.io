---
title: hexo的渲染问题
date: 2019-11-03 00:29:12
tags:
- hexo
categories:
- 建站
---

记录在修改主题时碰到的*奇怪*的问题
<!-- more -->

## 问题  

hexo默认会渲染`/source/`文件夹中所有的md和html文件，为了解决这个问题，hexo提供了以下两种解决方法；

1. 通过在`_config.yml'中添加`skip_render`字段来避免对于这类文件的渲染，如  

    ```yml
    skip_render: XXX/*
    ```

2. 通过在文件的头部添加`layout`字段来阻止渲染，如

    ```yml
    layout: false
    title: XXX
    tags:
    - tag_a
    - tag_b
    ```

然而在这次问题中，两种方式都无法解决渲染的问题。

## 解决方法  

最后发现是`themes/material-X/_config.yml'的问题。material-X中默认的sidebar设置是这样的：

```yml
menu_desktop:
  - name: 示例
    icon: fas fa-grin
    url: /
  - name: 分类
    icon: fas fa-folder-open
    url: blog/categories/
    rel: nofollow
  - name: 标签
    icon: fas fa-hashtag
    url: blog/tags/
    rel: nofollow
  - name: 归档
    icon: fas fa-archive
    url: blog/archives/
    rel: nofollow
```

会增加一个blog文件夹，为使各菜单也能够正确显示，有两种方式：

1. 在`source`文件夹中添加`blog`文件夹，再创建`catagories`文件夹及`index.md`文件放入`blog`中。  

2. 删除`_config.yml`中的blog字段，如：

    ```yml
    menu_desktop:
    - name: 首页
        icon: fas fa-grin
        url: /
    - name: 分类
        icon: fas fa-folder-open
        url: categories/
        rel: nofollow
    - name: 标签
        icon: fas fa-hashtag
        url: tags/
        rel: nofollow
    - name: 归档
        icon: fas fa-archive
        url: archives/
        rel: nofollow
    ```

实测，使用方案1会导致停止渲染失效，使用方案二则完美解决该问题。
