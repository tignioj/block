---
title: 手动编译frp
date: 2018-06-16 00:16:42
tags: [frp, 编译, 内网穿透, go]
categories: 内网穿透
---
# 今天我们来学习下如何编译frp0.20.0

### 一，在archlinux 下编译
**安装go**
`pacman -S go`
**获取frp源码**
`go get github.com/fatedier/frp`
进入家目录会发现多了个go目录

编译`frps`和`frpc`
`cd $HOME/go/src/github.com/fatedier/frp/cmd/frpc && go build`
`ls`
![frp0.20directiory.png](/blog/myimg/build/frp/frpc.png)
`cd $HOME/go/src/github.com/fatedier/frp/cmd/frps && go build`
`ls`
![frp0.20directiory.png](/blog/myimg/build/frp/frps.png)
## 二，Termux 下编译
```
pkg install golang
go get github.com/fatedier/frp
cd go/src/github.com/fatedier/frp/cmd/frpc/ && go build
cd go/src/github.com/fatedier/frp/cmd/frps/ && go build
```
步骤一样的，只是安装的go package不一样，termux 的包为golang, 而archlinux安装go就行了
