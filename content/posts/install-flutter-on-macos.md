---
title: "在MacOS上安装Flutter环境"
date: 2018-07-12T11:15:04+08:00
categories: ["Flutter"]
tags: ["开发环境", "安装教程"]
---

## 系统需求

- 操作系统： MacOS(64 位)
- 磁盘空间： 700MB(不包括 IDE 工具的磁盘占用)
- 必备工具： 需要以下命令行工具的支持
  - `bash`,`mkdir`,`rm`,`git`,`curl`,`unzip`,`which`
  <!--more-->

## 获取 FLutter SDK

1. 跟随以下链接下载到最新的 Beta 版本的 Flutter SDK(寻找其他发布版本，或者一些旧的版本编译，请查看[SDK 归档](https://flutter.io/sdk-archive/)页面。)：

- [flutter_macos_v0.5.1-beta.zip](https://storage.googleapis.com/flutter_infra/releases/beta/macos/flutter_macos_v0.5.1-beta.zip)

2. 解压这个文件在你想要的目录，例如：

```bash
$ cd ~/Study
$ unzip ~/Desktop/flutter_macos_v0.5.1-beta.zip
```

3. 添加`flutter`到你环境变量 path：

```bash
$ export PATH=`pwd`/flutter/bin:$PATH
```

_注：该方法只对当前终端进程有效，需要永久生效建议修改 PATH 文件_

执行以上命令以后，就可以在命令行直接输入`flutter`命令了，运行`flutter doctor`来完成安装。得到系统需要更新，这里再次输入`flutter upgrade`。

执行完后会自动执行`flutter doctor`，会检测当前系统缺少的依赖环境。如下所示：

```bash
 [✓] Flutter (Channel beta, v0.5.1, on Mac OS X 10.13.5 17F77, locale en-CN)
[!] Android toolchain - develop for Android devices (Android SDK 27.0.3)
    ! Some Android licenses not accepted.  To resolve this, run: flutter doctor --android-licenses
[!] iOS toolchain - develop for iOS devices (Xcode 9.4.1)
    ✗ Missing Xcode dependency: Python module "six".
      Install via 'pip install six' or 'sudo easy_install six'.
    ✗ libimobiledevice and ideviceinstaller are not installed. To install, run:
        brew install --HEAD libimobiledevice
        brew install ideviceinstaller
    ✗ ios-deploy not installed. To install:
        brew install ios-deploy
    ✗ CocoaPods not installed.
        CocoaPods is used to retrieve the iOS platform side's plugin code that responds to your plugin usage on the Dart side.
        Without resolving iOS dependencies with CocoaPods, plugins will not work on iOS.
        For more info, see https://flutter.io/platform-plugins
      To install:
        brew install cocoapods
        pod setup
[✓] Android Studio (version 3.1)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] IntelliJ IDEA Ultimate Edition (version 2017.3.5)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] VS Code (version 1.23.1)
[!] Connected devices
    ! No devices available

! Doctor found issues in 5 categories.
```

明显`✓`代表是满足依赖，`✗`代表不满足依赖，`!`是警告。
每行后面都包含了解决问题的方式和方法。

根据提示方法解决依赖不足的问题。如下是解决后`flutter doctor`所提供的报告：

```bash
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel beta, v0.5.1, on Mac OS X 10.13.5 17F77, locale en-CN)
[✓] Android toolchain - develop for Android devices (Android SDK 27.0.3)
[✓] iOS toolchain - develop for iOS devices (Xcode 9.4.1)
[✓] Android Studio (version 3.1)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] IntelliJ IDEA Ultimate Edition (version 2017.3.5)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] VS Code (version 1.23.1)
[!] Connected devices
    ! No devices available

! Doctor found issues in 3 categories.
```

到此环境安装已结束。可以在`Android Studio`新建工程并开始体验热更新。
