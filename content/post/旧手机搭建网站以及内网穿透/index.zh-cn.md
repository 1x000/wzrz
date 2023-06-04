---
title: 旧手机来做网站服务器
description: 仅供旧手机awa
date: 2022-12-20
slug: phone-web
image:
categories:
    - 折腾手机
tags:
    - frp
keywords: "手机做网站"
lastmod: 2023-04-04 11:14:16
---

# 旧手机来做网站服务器

> 仅供个人学习用

有旧手机的朋友，先别急着拿去换不锈钢脸盆，不妨用旧手机来当做网站的服务器，是不是感到有那么一丝丝的牛批

准备工作

## 1. 一部闲置旧手机

## 2. 下载并安装以下软件

KSWEB3.963 破解版  
https://pan.baidu.com/s/1Pu1i3pSzC2PCs-yC3u7N2g 提取码：504q  
JuiceSSH  
https://pan.baidu.com/s/1iXLYrG-UhzJG5NSdK5pIFQ 提取码：1jhz  
FRP 安卓客户端  
https://pan.baidu.com/s/1UQpn2YtodPxuv9LcKfCiWA 提取码:t4h2

## 3. 一台服务器，网上有免费的内网穿透服务，不过体验太差，我这里买了一台写教程，写完之后当然是退款了

### 1. 建立本地 php 环境，安装程序

打开 KSWEB 软件，并等待自动安装环境  
划到最右边设置 KSWEB：保护服务器非授权终止，锁定 wifi划到到 NGINX, 开启服务并新建一个主机划到工具，点击 phpMYAdmin 并选择 Nginx划到最左边的状态，点击开关重启一遍，以防止刚才添加的配置发生不生效的情况访问 phpMYAdmin, 我这里的本地链接为 [http://localhost:8002/](http://localhost:8002/)  
默认密码为空，登陆进去设置一个秘密，顺便新建一个数据库  
把你的网站源码用文件管理器解压至 htdocs 目录，访问本地链接并进行安装，我这里的地址为 [http://localhost:8001/](http://localhost:8001/)（这一步也可以在内网穿透绑定域名后进行）  

### 2. 进行内网穿透，绑定域名让他人可以访问

将域名 A 记录解析至你服务器的 ip  

#### 注意：在这之前，你可能需要在服务器上放行下方所用到的端口

打开 JuiceSSH 连接上你的服务器  
下载 FRP 服务端

```
wget https://github.com/fatedier/frp/releases/download/v0.31.2/frp_0.31.2_linux_amd64.tar.gz
```

下载完成后输入下方指令回车对文件进行解压

```
tar -zxvf frp_0.31.2_linux_amd64.tar.gz
```

输入下方指令回车进入 FRP 解压后的目录

```
cd frp_0.31.2_linux_amd64
```

输入下方指令回车编辑 frps.ini 配置文件

```
vi frps.ini
```

配置为如下内容：

```
[common]
#内网穿透服务器端监听的IP地址，可以省略，默认为127.0.0.1
bind_addr = 0.0.0.0
#服务器端监听端口，默认是7000，可自定义
bind_port = 7000
#该端口就是以后访问web服务需要用到的端口
vhost_http_port = 80
```

按下 ESC 键，输入下方指令回车，保存编辑的内容并退出编辑

```
:wq
```

输入下方指令并回车后台运行 FRP 服务端，就可以关闭 JuiceSSH 了

```
nohup ./frps -c frps.ini >/dev/null 2>&1 &
```

打开 FRP 客户端软件，根据下方配置，添加到 FRPC，点击右下角小飞机即可

```
[common]
#你的服务器ip地址
server_addr = 91.206.92.71
#监听端口，可自定义，必须和服务端一样
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
#此处可自定义
remote_port = 888

[web01]
#可选择http或者https
type = http
#KSWEB上的监听端口
local_port = 8001
#你的域名
custom_domains = dwz.bar

[web02]
#可选择http或者https
type = http
#KSWEB上的监听端口
local_port = 8002
#你的域名
custom_domains = s.dwz.bar

#如需建立多个网站，[we01],[we02],[we03]以此类推
```

这时候你会发现你已经成功了，打开浏览器，开始访问你的域名吧

科普及注意事项

### 1.frps 服务端，也就是服务器上运行的，frpc 客户端，同理为手机上需要运行的

### 2.KSWEB 和 FRP 两个软件需要保持后台运行，否则你将不能通过域名访问你的网站

### 3. 访问速度以及质量取决于你服务器的地区节点以及带宽和自家的 wifi 带宽

### 4. 以教程为列子，服务器需要放行的端口有 22,80,7000,888

### 5. 如果是国内服务器的话，你可能需要备案

### 6. 没了，暂时就这样吧，有问题可以评论回复

## 补充一些可能用到的：

### 关闭 FRP 服务端

输入以下指令回车找到 FRP 的进程

```
ps -aux|grep frp| grep -v grep
```

会提示大概如下，其中 3600 为 FRP 的进程

```
root      3600  0.1  0.1 110188  9484 pts/0    Sl   15:04   0:00 ./frpc -c ./frpc.ini
```

输入以下指令回车停止 FRP 的运行

```
kill -9 3600
```

### 某些需要指令才能放行端口的 vps

```
#放行端口7000
firewall-cmd --zone=public --add-port=7000/tcp --permanent
#关闭7000端口
firewall-cmd --zone=public --remove-port=7000/tcp --permanent
#配置立即生效
firewall-cmd --reload
#查看防火墙所有开放的端口
firewall-cmd --zone=public --list-ports
#关闭防火墙（放行所有端口）
systemctl stop firewalld.service
#查看防火墙状态
firewall-cmd --state
#查看监听端口
netstat -lnpt
#PS:centos7默认没有 netstat 命令，需要安装 net-tools 工具，yum install -y net-tools
#重启防火墙
service iptables restart
```