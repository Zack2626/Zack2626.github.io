---
title: 调用poplayer播放emby
date: 2020-02-12 16:19:11
tags:
- emby
catagories:
- 建站
---

一直使用emby，主要是因为免费，虽然更新到4.0之后免费版已经不能硬解了，但好在我网速还行，看的视频码率比较低，不需要转码播放。但最近更新之后，网页版的播放体验越来越差了，这菜单贼老大，菜单隐藏情况下还不能通过按键来调节进度，所以尝试用potplayer来播放emby视频。
<!-- more -->

# 1. 原理
potplayer可以打开链接播放，所以只要能够获取emny的播放链接，就能够利用potplayer来播放emby视频了。
分析下来主要有以下几个问题需要解决：

1. 获取emby的视频链接
1. 如何方便地从浏览器调用emby

## 获取emby视频链接
我们打开emby的一个视频，按F12打开开发者工具，就能找到视频链接，在标签下的src属性中。这样我们就获取了视频链接。
![获取视频url.jpg](https://cdn.nlark.com/yuque/0/2020/jpeg/616725/1581498376893-04561e61-6566-47c0-a685-64edb39f6a92.jpeg#align=left&display=inline&height=417&name=%E8%8E%B7%E5%8F%96%E8%A7%86%E9%A2%91url.jpg&originHeight=1080&originWidth=1920&size=308784&status=done&style=stroke&width=746)

## 调用emby播放链接
potplayer自带了打开链接的功能，如下图所示，也可以参考网上用potplayer看直播的教程。
![potpalyer打开链接示意.png](https://cdn.nlark.com/yuque/0/2020/png/616725/1581498388593-41de7bc1-d01a-46d0-aefe-c2a683155375.png#align=left&display=inline&height=332&name=potpalyer%E6%89%93%E5%BC%80%E9%93%BE%E6%8E%A5%E7%A4%BA%E6%84%8F.png&originHeight=499&originWidth=1124&size=29682&status=done&style=stroke&width=746)

这样，就完成了用potplayer播放视频，但是过程太过去繁琐。整个流程需要很多操作。都说懒是第一生产力，因而想办法简化流程。

# 2. 利用chrome插件来简化流程
首先想到的就是利用chrome插件来自动化处理，通过插件来自动识别emby的链接，并调用potplayer播放。
识别链接网上有很多成功的插件；而调用potplayer，可以利用外部协议url，通过写注册表将url与本地应用程序关联。potplayer安装时自动写入了注册表，所以只需要在浏览器内输入 `potplayer://+url` 就能够利用potplayer来打开视频了，效果如下：![image.png](https://cdn.nlark.com/yuque/0/2020/png/616725/1581499249292-77087147-ce9a-4c13-865e-b22421ad1bdd.png#align=left&display=inline&height=419&name=image.png&originHeight=1080&originWidth=1920&size=138091&status=done&style=stroke&width=746)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/616725/1581499393050-75860a41-2e6a-4154-a60a-335e10e2f40b.png#align=left&display=inline&height=123&name=image.png&originHeight=314&originWidth=1912&size=39733&status=done&style=none&width=746)
所以只需要编写chrome插件，识别链接，并打开新标签页，就能够一步实现用potplayer看emby视频了。
兴奋的我立马开始研究如何写chrome插件，奈何自己没有基础，不会js，折腾了一整晚也没有成功，最后只能选择对别人的插件进行修改。（请参考[如何修改下载的插件](https://cloud.tencent.com/developer/article/1028111)）。

这里我选取了[笨笨猫](https://www.94cat.com/)编写的[猫抓](https://chrome.google.com/webstore/detail/%E7%8C%AB%E6%8A%93/jfedfbgedapdagkghmgibemcoggfppbb?hl=zh-CN)，这个插件能够直接获取页面上的视频链接，对于emby的链接也能够有效获取，并且对单个视频提供了调用html视频播放器播放视频的按钮，改起来也较为方便。
猫抓插件使用效果
![image.png](https://cdn.nlark.com/yuque/0/2020/png/616725/1581500044224-aea2abe1-4a75-4d3b-9d20-862cd2e5c3f1.png#align=left&display=inline&height=1080&name=image.png&originHeight=1080&originWidth=1920&size=1945769&status=done&style=none&width=1920)

更改的方法就是找到原有插件中播放按钮的代码，改为上文提到的打开 `potplayer://+url` 就可以了。具体代码如下。
```javascript
//播放
$('#medialist #play').off().on('click',function(){
  var url = $(this).parents().find('.url a').attr('href');
  chrome.tabs.create({
    url: "potplayer://" + url
  });
  return false
}];
```

最后，刷新一下插件，大功告成。

# 3. 后话
其实，除了emby，很多别的视频流也能够通过potplayer来播放，但是现在chrome的插件库中都是提取视频链接下载视频的，却没有调用本地播放器来播放视频的，希望能够有大神解决这个问题。

# 4. 存在问题
使用一段时间后发现，这种方法只适用于H264编码的视频，对于H265(HEVC)的视频，emny会分割成多个小的ts文件来传输，而不是一整个文件，因而没办法用potplayer直接播放到底。