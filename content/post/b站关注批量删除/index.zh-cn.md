---
title: b站关注批量删除
description: 简单的B站关注列表清除方法
date: 2023-04-09
slug: bz-gzqc
image:
categories:
    - tool
tags:
    - Bilibili
    - 关注列表清除
    - 一键清除B站关注列表
keywords: "B站,bilibili,关注清除"
lastmod: 2023-04-09 08:56:12
---

# 快速取消B站（哔哩哔哩）关注的UP？

当你**互粉**之后你就会发现一堆没有用的视频就出来了（我没互粉）或者你在注册新号的时候不小心关注了一堆UP

然后你发现B站没有批量取消关注功能，只能一个一个（恼）的点击。

```javascript
var ms = 300; // 暂停300毫秒
var ii = 0;
var xx = $(".be-dropdown-item:contains('取消关注')");
console.log("本页关注了", xx.length, "个UP！");
tt = setInterval(function(){
  if (0 <= ii && ii < xx.length) {
    xx[ii].click(); // 自动点击｛取消关注｝
  } else {
    clearInterval(tt); // 停止操作
    console.log("OK！本页没UP了！");
  }
  ii += 1;
}, ms+ii*10); // 暂停多少毫秒，再执行下一次点击，时间间隔增加变化
```

以**Chrome**浏览器为例

按**F12**

进入**Console**界面

粘贴以上代码

`Enter`

即可批量快速取消关注本页20所有up主

**F5**或**Ctrl+R**刷新一下页面，再执行一遍，就可以快速把所有up主都果断取关了。
