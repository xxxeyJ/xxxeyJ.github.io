---
layout: post
title: "Golang Cross Compiling"
tags: [Golang, Program,Tricks]
comments: true
---

## 前言

当我们在生成一个由 **Golang 实现的且对外发布供用户使用**的程序的时候，为了便于用户使用，往往要根据**不同用户群体**所使用的不同操作系统及不同架构生成相应的二进制版本，这时就需要用到**交叉编译**这一功能，Golang 这门编程语言其中亮点之一就是它自带了交叉编译这一功能，为我们提供了很大的**便捷性**，但是当我想要使用这一功能时，我检索了一下互联网上的**中文资料**，却发现很多人给出的方法完全是带有**误导性**的，通过此类方法并**不可以交叉编译**，那么这篇文章就来讲一下我是怎么实现交叉编译的；

## 实现

**本地环境**:

Windows 10 x64   
Golang 1.15.3

> 查看 Golang 环境变量( 默认配置 ):   
> Go env   
> Output:   
```bat
set GO111MODULE=
set GOARCH=amd64
set GOBIN=
set GOCACHE=C:\Users\xxxeyJ\AppData\Local\go-build
set GOENV=C:\Users\xxxeyJ\AppData\Roaming\go\env
set GOEXE=.exe
set GOHOSTARCH=amd64
set GOHOSTOS=windows
set GOINSECURE=
set GOMODCACHE=C:\Users\xxxeyJ\go\pkg\mod
set GONOPROXY=
set GONOSUMDB=
set GOOS=windows
set GOPATH=C:\Users\xxxeyJ\go
set GOPRIVATE=
set GOPROXY=https://proxy.golang.org,direct
set GOROOT=c:\go
set GOSUMDB=sum.golang.org
set GOTMPDIR=
set GOTOOLDIR=c:\go\pkg\tool\windows_amd64
set GCCGO=gccgo
set AR=ar
set CC=gcc
set CXX=g++
set CGO_ENABLED=1
set GOMOD=C:\Tools\Golang\Simple\go.mod
set CGO_CFLAGS=-g -O2
set CGO_CPPFLAGS=
set CGO_CXXFLAGS=-g -O2
set CGO_FFLAGS=-g -O2
set CGO_LDFLAGS=-g -O2
set PKG_CONFIG=pkg-config
set GOGCCFLAGS=-m64 -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=C:\Users\XXXeyJ~1\AppData\Local\Temp\go-build777295706=/tmp/go-build -gno-record-gcc-switches
```

此类问题的中文资料大多解决方案是这样子的；

![](https://tricksongs.com/images/Golang_Cross_Compiling/Net.PNG)

> **存在怎样的问题？**

执行前:

![](https://tricksongs.com/images/Golang_Cross_Compiling/up.PNG)

键入命令:

![](https://tricksongs.com/images/Golang_Cross_Compiling/ZH.PNG)

执行后:

![](https://tricksongs.com/images/Golang_Cross_Compiling/down.PNG)


> **SET 指令是 Windows 中用于配置临时环境的变量的指令，无法更改 Go env 中的数值，此类方法并不适用于 Windows 平台；**

> **那么如何在 Windows 平台下进行交叉编译?**

使用 **-w** 选项添加键值对；

```BAT
go env -w GOOS=linux
go env -w GOARCH=amd64
go env -w CGO_ENABLED=0
```

![](https://tricksongs.com/images/Golang_Cross_Compiling/WWW.PNG)

执行后结果为

![](https://tricksongs.com/images/Golang_Cross_Compiling/GOOS.PNG)

**CGO_ENABLED 最好设置为 0**，由于我的测试程序本质上实现的是一个提供基础输出功能的程序，所以 CGO_ENABLED 设置为 1 也没有什么特别大的影响，但是如果是将其用于编译**多平台程序**的时候还是要将其设置为 0 的，这是因为如果将 CGO_ENABLED 设置为 1 时可以在用于构建程序的操作系统上动态加载本机库，在考虑到**多平台交叉编译时**，将 **CGO_ENABLED** 设置为 1 并不是一个特别好的想法；

## 之余

分享一枚用于编译程序的小 **Tricks**，懂的自然懂；

```Go
go build -ldflags "-w -s" main.go
```

## END
**:)**

