---
title: VPS安全加固
description: 去TM的入侵
date: 2022-11-05
slug: linux-anquan
image: awa.png
categories:
    - linux
tags:
    - 保护VPS
keywords: "vpssy,vps安全"
lastmod: 2023-04-04 1:43:00
---

# VPS安全加固

VPS 用了快3个月，开始意识到之前 VPS 一点安全措施都没做。

> 由于一台 vps 就相当于一台拥有独立 IP 的，直接暴露于互联网之上的电脑，这在为你带来便利的同时也直接让你的 vps 与危险画上了等号，密码穷举、DDOS 攻击、各种各样你想到的想不到的攻击方法都在等待着你，不要相信你那惊悚的创意能想出多么强大的密码，统计学告诉我们——还是相信 RSA 吧！

不信，我们在机器上跑几个命令看看吧。

- 1.查看尝试暴力破解机器密码的人

`sudo grep "Failed password for root" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr | more`

或者 `cat /var/log/auth.log`  
​

- 2.查看暴力猜用户名的人

`sudo grep "Failed password for invalid user" /var/log/auth.log | awk '{print $13}' | sort | uniq -c | sort -nr | more`

看完倒吸一口可乐bushi

想破解我的 IP 有 14 个，看来必须行动起来了。

## 采取行动

> `root@vultr:~#` apt-get update  
> `root@vultr:~#` apt-get upgrade  
> 没用配置国内的源，速度嫌慢，就省略这步，否则需要等 20mins。  
> 对于自动每日更新，我们可以配置一些执行软件。至于是否必要，如果你的服务器只是用来安装一个 SS 或 SSR 应用，并没有其他非常重要的服务端服务，也许并不需要这种操作，因为这一过程实在太消耗时间和资源了。但是如果是有其他重要的数据服务，保证软件的安全补丁的及时更新还是十分必要的。

普通使用者，只在第一次安装，或在几个发行周期之后才去更新都没问题。

用户设置

启用公钥验证登入 ssh

在第一次部署 VPS 服务器没有勾选 SSH，删除重装其实最简单的办法。可是这样旧套餐就没了。只能在部署的服务器上手动添加 SSH key。

使用Xshell的密钥生成功能选择最高加密位数然后加上密码

关于 SSH key 的知识可参考  
[SSH key 的介绍与在 Git 中的使用](https://www.jianshu.com/p/1246cfdbe460)  

[ssh 官网](https://www.ssh.com/ssh/keygen/)

[~~vultr 一直被墙IP的VPS 官网~~](https://www.vultr.com/docs/how-do-i-generate-ssh-keys/)

2.将本地的公钥扔到服务器上去  

要注意的是，你想要这对密钥登录哪个账户，就放到哪个账户下，比如我要自动登录 root，那么我就会这样做：

> mkdir ~/.ssh //如果当前用户目录下没有 .ssh 目录，就先创建目录  
> chmod 700 ~/.ssh  
> mv id_rsa.pub ~/.ssh  
> cd .ssh  
> mv id_rsa.pub authorized_keys //因为是新系统，就直接改名。 如果本身就有 authorized_keys 文件，就需要 cat id_rsa.pub >> authorized_keys。将文件内容追加入 authorized_keys。  
> chmod 600 authorized_keys

关闭所有帐户 ssh 密码登录

> /etc/ssh/sshd_config

在如上目录找到 ssh 服务的配置文件，将如下字段进行修改：

> #PasswordAuthentication yes  
> //修改为如下内容：  
> PasswordAuthentication no //所有用户都不能以密码的方式登录了。  
> RSAAuthentication yes  
> PubkeyAuthentication yes  
> //使用命令重启 ssh 服务：  
> /etc/init.d/ssh restart  
> 另外，为了避免 Write failed: Broken pipe 错误，你也可以顺便在这个文档末尾加上下面一条命令，然后再重启 ssh 服务。  
> ClientAliveInterval 60

更换默认 ssh 端口

> /etc/ssh/sshd_config

进入以上文件，注销 22 端口并新增。

> 把#port 22  
> 改成port xxxxx //你定义的端口，任何 1024 – 65535 之间的数字。知名端口，常用于系统服务等，例如 http 服务的端口号是 80

一个没有防护的节点，就像带着流血的伤口，在鲨鱼出没的水域里游泳。俗称：肉鸡。  
网络虽然是自由的信息场，但也绝不是纯净的伊甸园。恩，得学会自我保护。