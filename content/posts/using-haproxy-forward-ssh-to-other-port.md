---
title: "使用Haproxy转发内网其他主机的ssh端口到公网的指定端口"
date: 2018-06-13T16:19:51+08:00
categories: ["网管"]
tags: ["代理", "端口转发"]
---

> 需求情况如下，公司公网 IP 只有一个，为了充分利用这个 IP 实现多个内部服务器可以进行外部访问。

<!--more-->

首先作为反向代理的服务器需要如下准备：

- 操作系统安装(\*Linux)
- Haproxy 软件安装
- 双网卡，一个获取为公网 IP 地址，另一个为内网地址

Haproxy 软件安装很简单，因为我用的 Ubuntu，安装命令如下：

```bash
sudo apt install haproxy
```

安装完成后直接编辑`/etc/haproxy/haproxy.cfg`，在其最后回车后添加如下参数：

```bash
listen example #example是为这条记录取的别名，任意起名，方便记录
    bind 0.0.0.0:2223 #监听所有来源的2223端口请求
    mode tcp #tcp模式
    server example-server 192.168.100.100:22 #example-server 是自己本地所指向的服务器进行取名，同样是方便记录，后面的IP和端口是你想要转发的目标IP和端口。
```

保存退出后直接，`service haproxy restart` 即可生效。

现在就可以通过公网 IP（例如：203.33.13.24:2223）进行 ssh 访问，使用内网的用户名和密码，可正常登录。
