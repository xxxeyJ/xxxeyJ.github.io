---
layout: post
title: "BurpSuite 与 Xray 的搭配公式"
tags: [Tricks, BugScan, Xray]
comments: true
---

## 前言

> [#] Author: xxxeyJ   
> [#] Blog: https://tricksongs.com/

在对目标展开测试时，使用 Xray 能够以**极高的效率以黑盒的形式快速定位目标资产**所存在的问题，但是如果只是通过漏扫的形式定位问题，并不能完全发现问题所在，我认为只有当工具和手工搭配起来才能以更高的效率完成审计工作；

这时就需要使用 BurpSuite 与 Xray 进行组合，C位原地出道，我在 Xray 社区中看到之前有师傅发过类似的文章，但是那篇文章讲的是将 **BurpSuite 作为中继将浏览器的请求经由 BurpSuite 转发给 Xray**，但是这样的话并不能看到 Xray 所发起的请求，会对审计目标资产产生一些困扰；

今天这篇文章就写一下我是怎么实现将 Xray 作为中继将流量通过 BurpSuite 并延展出一整条链的，大致结构如下 ( **Proxy 可根据实际情况选择是否添加** )；

> Browser <-> Xray <-> BurpSuite <-> Proxy <-> Internet	

## 实际操作

首先使用 Xray 起一个 webscan 并监听指定端口；   

![](https://tricksongs.com/images/BAX/31337.PNG)

在浏览器配置相关代理设置，我这里监听的端口是 **31337**，所以写入127.0.0.1:31337；

![](https://tricksongs.com/images/BAX/Firefox.PNG)

修改 Xray 的配置文件 **config.yaml**，给 Xray 添加 BurpSuite 的代理，BurpSuite 默认监听 **8080** 端口，这里可以根据读者实际情况进行修改；

![](https://tricksongs.com/images/BAX/Proxy.PNG)

当这些操作完成后，Xray 的流量已经可以经过 BurpSuite 了，访问一下 http://testphp.vulnweb.com/ 测试一下；

![](https://tricksongs.com/images/BAX/testphp.PNG)

可以看到，Xray 所发出的请求都经由 BurpSuite 转发给了目标；

![](https://tricksongs.com/images/BAX/BPR.PNG)

可以正常使用；

![](https://tricksongs.com/images/BAX/Vuls.PNG)

当然，也可以在 BurpSuite 中设置上层代理， 进入 **BurpSuite** 中的 **User Options** 模块，可以选择 **Upstream Proxy Servers** 或者 **SOCKS Proxy**，这里值得一提的是 **Upstream Proxy Servers** 是基于 HTTP 协议的代理；

**Upstream Proxy Servers** 格式如下，**Destination host** 一般设置为通配符即可，表示对 **everything** 都使用代理；

![](https://tricksongs.com/images/BAX/UPS.PNG)

**SOCKS Proxy** 格式如下；

![](https://tricksongs.com/images/BAX/SOCKSPROXY.PNG)

### **:)**
