---
title: "Awesome Wm Virtualbox Full Screen"
date: 2023-08-15T22:56:34+08:00
draft: false
categories: ["Linux"]
tags: ["AwesomeWM", "Virtualbox", "full-screen"]
---

通常在 `AwesomeWM` 拥有 `top` 栏的情况下，如果使用 `Virtualbox` 的虚拟机的时候，没有办法获得沉浸式的体验。

正常情况下我们会利用 `Virtualbox` -> `View` -> `Full-Screen Mode` 进行全屏，但是 `top` 栏仍然会存在于界面上。

> `Right Ctrl + f` 可以进行快速的 Full-Screen Mode 的切换，这是由 Virtualbox 提供的快捷键

## 解决方法

首先利用 `Right Ctrl + f` 进入 `Virtualbox` 的全屏模式，然后使用 `Right Ctrl + Home` 呼出控制菜单

当当前窗口聚焦于该控制菜单的时候，快速按 `Mod4 + f` 进入 `AwesomeWM` 提供的全屏模式

此时即可大功告成。

如果是希望退出全屏的状态，则仍然使用 `Right Ctrl + Home` 并且利用 `Mod4 + f` 或者 `Right Ctrl + f` 的方式即可退出全屏。

## 个人理解的好处

- 最终可以获得一个 `tag` 利用虚拟机进行独占显示，可以把该 `tag` 看作为单独的系统进行使用。
- 更加沉浸的体验，当完全希望是在弄外的系统内进行学习的时候
- 除非是触发 `Right Ctrl + Home` 或者其他基于 `Right Ctrl` 的快捷键的情况下，在全屏模式下可以更好的进行虚拟机内部的快捷键的使用

> 由于我的 `HHKB` 键盘没有 `Right Ctrl` 按键， 所以只能在 `Perferences` 中将 `Input` 的 `Virtual Machine` 里的 `Host Key` 改为 `Right Shift` 即可
