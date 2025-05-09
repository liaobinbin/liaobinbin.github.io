---
title: "懒猫相册AI增强NAS照片存储"
date: 2025-05-09T11:15:04+08:00
categories: ["Linux"]
tags: ["懒猫微服", "教程", "NFS"]
---

## 起因

一直心心念念懒猫的相册AI搜索功能，但是一直没有去将原 `群晖` Nas 中的照片进行传输到懒猫中，原因有以下几点

- 我的懒猫是低配，只有 `2TB` 存储空间
- 个人觉得 `3.5` 寸的企业级机械硬盘的稳定性可能会好过 `2.5` 的机械硬盘
- 懒 `🤦🏻 🤦🏻 🤦🏻 🤦🏻`

## 准备工作

- 需要能够 `ssh` 访问懒猫微服机器 *`没有的可以联系 VIP 群开通`*
- 你的 `NAS` 可以支持文件共享 🧎‍♂️ 🧎‍♂️ 🧎‍♂️ `应该没有不支持的吧`

## NAS的设置

我这里以群晖进行举例，其他的 `NAS` 系统请自己想办法

我选择的是基于 `NFS` 的文件共享协议，全称为 `Network File System` 也就是 `网络文件系统`。 根据网上查到的不确定消息，在 `NFS 4.x` 版本过后，`Linux` 系统间使用此协议共享文件会有较好的性能，特别是**小文件**的传输。所以用来共享图片我觉得是非常合适的。当然你也可以选择使用 `Samba`, `WebDav` 这些方式，我就不过多赘述，请自行查阅资料。

### 启用 NFS 共享功能

`群晖` -> `控制面板` -> `文件服务` -> `NFS` -> `启用NFS服务` -> `最大NFS协议` -> `NFS4.1` -> `应用`


![NFS配置](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/336/40a5ce92-23fd-4968-a6fa-a619da2b6f31.png "NFS配置")

### 配置共享文件夹NFS权限

`群晖` -> `控制面板` -> `共享文件夹` -> `你需要共享的文件夹(我目前共享的photo)` -> `右键编辑` -> `NFS权限` -> `新增规则` -> `填写对应条目` -> `应用`

> **注意** ：服务器名称或者IP地址用于授权对应主机可以访问，我这里是期望我的局域网可以访问，所以填写的当前局域网的网段地址，如果单指懒猫的IP地址的话，应该会有更好的安全性。否则其他连入你网络的设备可能会泄漏你的数据。
> 例如：WIFI密码泄漏  🤦🏻 🤦🏻 🤦🏻
> 当然，更多网络隔离相关的知识请自行问AI，我这里就不多BB了

![NFS权限](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/336/9659cbbd-9cf0-439b-b678-b3e0827db4a3.png "NFS权限")

上述的配置大功告成后，就可以进入懒猫的配置了

## 懒猫的配置

首先 `ssh` 到你的懒猫微服

```bash
ssh root@你的微服名字.heiyu.space
```


### 挂载

提前在懒猫网盘中 找到 `图片文件夹` 创建一个子文件夹，因为后续的挂载需要文件夹的承接

由于懒猫缺少 `NFS` 相关的支持，需要通过以下命令安装

```bash
apt install nfs-common
```

完成安装过后，使用以下命令进行挂载

```bash
   mount -t nfs 你的NASIP:/装载路径 /data/document/你的用户名/Pictures/你创建的文件夹
```
关于装载路径的说明，这里可以在共享文件夹，如下图可以找到

![装载路径](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/336/d5ad9333-8623-4545-9bb5-56e3e414afdb.png "装载路径")

如果需要更佳准确的只挂载该共享文件夹下的子目录，则可以在 `装载路径` 后面继续拼接对应的路径，以确保分散的图片文件能够可以在 `Pictures` 下可以被访问到，当然，值得注意的是，一个挂载路径应该对应一个提前创建好的子文件夹

没有收到任何错误提示的情况就表示挂载成功了。
然后就可以爽了，懒猫相册需要一定的时间去扫描文件夹的图片

### 自动化挂载

由于上述方式的挂载方式是临时的，在懒猫重启后会挂载会失效

所以可以想到的是，在懒猫系统启动后自动帮你执行挂载命令

首先我想到的是 `fstab` ，结果测试了一下，修改了过后，重启懒猫的话配置文件会被重置。所以这些方案都不太行

#### 懒猫的启动自定义脚本

[如何在开机时启动自定义的脚本](https://developer.lazycat.cloud/faq-startup_script.html) 和 [Dockerd 开发模式](https://developer.lazycat.cloud/dockerd-support.html#%E8%8E%B7%E5%8F%96%E5%B9%B6%E5%AE%89%E8%A3%85dockge%E5%BA%94%E7%94%A8) 这两篇得好好看看，整体流程有点麻烦

首先 创建一个 `shell` 脚本，我试过很多方式在容器里面执行挂载了 `NFS` 后，容器内是可以访问到文件了，但是宿主机也就是懒猫是没有办法访问到文件的。
所以我想到的是曲线救国通过容器内 `ssh` 回宿主机执行
```shell
# 安装 ssh 客户端
apt install openssh-client -y &&
# 安装 sshpass 自动输入密码
apt install sshpass -y &&
# 通过 sshpass 连接到懒猫并且执行命令
# 执行的命令就是 上一章节提及的 手动挂载的方式
sshpass -p "你的ssh密码" ssh -o StrictHostKeyChecking=no root@你的微服名字.heiyu.space "
apt install nfs-common -y &&
mount -t nfs 192.168.1.3:/volume2/photo /data/document/boiaoch/Pictures/nas-photo &&
mount -t nfs 192.168.1.3:/volume2/homes/BoiaoCh /data/document/boiaoch/Pictures/boiao
"
```
然后创建一个 `.sh` 脚本文件，放在自己懒猫网盘的某个目录下

```yaml
services:
  Debian:
    #服务名称可自行修改
    image: registry.lazycat.cloud/debian:autostart_mod
    #此镜像可直接在懒猫微服中进行拉取，无需配置代理
    privileged: true
    #注意，如脚本无需对系统进行修改则不要添加
    restart: always
    entrypoint: /bin/init
    #这里需要使用init来防止脚本结束后容器重启
    command: sh /data/document/boiaoch/shell/autoMountNfs.sh #脚本地址改为自己的对应的文件路径，该脚本会在容器内执行
    #设置启动时的指令，需要注意脚本在此容器中的路径
    volumes:
      - /data/document/boiaoch:/data/document/boiaoch:ro
      #此配置将网盘目录挂载到容器的/data/document/<用户名>目录下。后面的":ro"用于防止对网盘目录进行修改。该字段可视情况进行修改
      - /data/playground/docker.sock:/var/run/docker.sock
      #此配置可以将playground-docker的socket文件绑定到容器内，允许容器对playground-docker进行修改。
    network_mode: host
    #添加该字段并配合privileded可以控制微服系统的网络设备
networks: {}
```

最终，没了，是真的没了。