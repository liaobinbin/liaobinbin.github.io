---
title: "修复DropBox同步到导致的SSH密钥文件权限丢失"
date: 2018-11-07T21:29:51+08:00
categories: ["系统"]
tags: ["文件权限", "linux"]
---

`ssh -Tv git@gitlab.com`
得到以下提示

<!--more-->

```bash
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/Users/boiao/.ssh/gitlab' are too open.
```

## 解决方法

```bash
chmod 600 ~/.ssh/gitlab
```
