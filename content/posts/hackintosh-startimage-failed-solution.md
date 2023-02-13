---
title: "Hackintosh OCB：Startimage Failed 的解决方法"
date: 2021-01-24T22:44:15+08:00
draft: false
categories: ["系统"]
tags: ["hackintosh", "boot"]
---

如果你的黑苹果，在进入系统的时候，出现该错误，而且该错误并不是每次都会发生，当你继续回到主菜单，多次重新尝试的时候，你可能可以成功进入系统。

## 多次尝试一次都未能进入系统

Booter->Quirks->DevirtualiseMmio 设为 true

## 多次尝试有几率进入系统

Booter->Quirks->ProvideCustomSlide 设为 true

### z390 主板特殊处理

Booter->Quirks->ProtectUefiServices 可能会需要设为 true，具体自己测试
