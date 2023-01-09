---
title: "关于微信小程序onReachBottom上拉触底事件"
date: 2018-11-09T09:15:50+08:00
draft: false
---

`onReachBottom`事件触发有延迟，快速下拉的话有几率不会触发事件

<!--more-->

> 网上的资料说的是有延迟。大概是350ms


## 解决方法
可以利用`scroll-view` `bindscrolltolower`进行事件触发
