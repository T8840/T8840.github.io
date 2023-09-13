---
title: RemoteControl
cover: 'https://images.unsplash.com/photo-1676914730169-ade8d85e9db7?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxfDB8MXxyYW5kb218MHx8fHx8fHx8MTY5NDU5NjU4OQ&ixlib=rb-4.0.3&q=80&w=1080'
abbrlink: e7b1adcd
date: 2023-09-13 17:12:41
updated: 2023-09-13 17:12:41
tags: 网络安全
categories: 网络安全
---

## 远程控制工具

### GotoHTTP
GotoHTTP是一款基于https的远控工具。
- 优势
B2C 模式，无需安装控制端软件，有浏览器就可以远控。
流量走 https 协议，只要目标放行 443 端口出口就可以实现内网穿透。
- GoToHTTP安装
官网地址：http://www.gotohttp.com/
- 在Linux中运行
```
wget http://gotohttp.com/gotohttp_gui_x64.tar.gz
tar -xvf gotohttp_gui_x64.tar.gz
sudo ./gotohttp_gui_x64/gotohttp
```
如上，我们运行软件得到了控制Linux的id和控制码。
在浏览器输入远程电脑ID及控制码立即开始控制。
这时候，通过浏览器便可以直接控制Linux。
当然，用手机浏览器也是可以连接的。
- 控制安卓手机
安装手机需要运行，需要adb执行下面命令
adb tcpip 5555
- 在Windows运行
下载对应的Windows安装包，直接运行即可。连接方式和上面一样。
