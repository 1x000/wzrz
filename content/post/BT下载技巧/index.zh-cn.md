---
title:  vps服务器中安装frp开放端口
description:  vps服务器中安装frp内网穿透软件为比特彗星BT种子下载提供公网IP绿灯开放端口
date: 2023-01-08
slug: frp-bt
image:
categories:
    - tool
tags:
    - BT下载穿透
keywords: "BT下载穿透"
lastmod: 2023-04-04 20:53:12
---

# vps服务器中安装frp开放端口

## 安装GOST

创建Scock5隧道

```bash
wget --no-check-certificate -O gost.sh https://raw.githubusercontent.com/KANIKIG/Multi-EasyGost/master/gost.sh && chmod +x gost.sh && ./gost.sh
```

## 安装Frp

### 安装

```bash
yum -y install wget screen
wget https://github.com/fatedier/frp/releases/download/v0.46.0/frp_0.46.0_linux_amd64.tar.gz -O frp.tar.gz
tar xzf frp.tar.gz
cd frp_*
```

### 修改配置文件

```bash
vi /root/frp_*/frps.ini
```

### frps.ini内容为

```ini
[common]
bind_port = 9127
bind_udp_port = 9127
bind_addr = 0.0.0.0
allow_ports = 1001-1500,10001-22500
token = www.xpdbk.com
tls_only = true
```

### 启动软件

```bash
cd /root/frp_*;screen -dmS awa ./frps -c ./frps.ini
```

### 查看是否启动成功

```bash
netstat -apn | grep frps
```

### 设置为开机启动

```bash
echo "cd /root/frp_*;screen -dmS awa ./frps -c ./frps.ini" >> /etc/rc.d/rc.local
```

### centos 7需要设置执行权限

```bash
chmod +x /etc/rc.d/rc.local
ll /etc/rc.d/rc.local
```

## 检查防火墙与服务器安全组放通端口

### 将所有iptables以序号标记显示，执行：

```bash
iptables -L -n --line-numbers
```

#### 清空命令

```bash
iptables -t nat -F
iptables -t nat -X
iptables -t nat -P PREROUTING ACCEPT
iptables -t nat -P POSTROUTING ACCEPT
iptables -t nat -P OUTPUT ACCEPT
iptables -t mangle -F
iptables -t mangle -X
iptables -t mangle -P PREROUTING ACCEPT
iptables -t mangle -P INPUT ACCEPT
iptables -t mangle -P FORWARD ACCEPT
iptables -t mangle -P OUTPUT ACCEPT
iptables -t mangle -P POSTROUTING ACCEPT
iptables -F
iptables -X
iptables -P FORWARD ACCEPT
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
iptables -t raw -F
iptables -t raw -X
iptables -t raw -P PREROUTING ACCEPT
iptables -t raw -P OUTPUT ACCEPT
service iptables save
```

### centos 7中关闭firewalld防火墙

启动  
`systemctl start firewalld`

启用开机自启动  
`systemctl enable firewalld`

关闭  
`systemctl stop firewalld`

关闭开机自启动  
`systemctl disable firewalld`

查看运行状态  
`firewall-cmd --state`

#### 一键关闭也可以

```bash
systemctl stop firewalld;systemctl disable firewalld;service ip6tables stop;service iptables stop;chkconfig iptables off
```

## Windows Frp客户端下载

https://github.com/fatedier/frp/releases/download/v0.46.0/frp_0.46.0_windows_386.zip

### 解压并且用记事本打开修改配置文件frpc.ini

#### frpc.ini内容为，x.x.x.x改成frp的服务器ip

```ini
[common]
server_addr = x.x.x.x
server_port = 8000
protocol = websocket
token = www.itzmx.com
tls_enable = true

[Remote Download]
type = tcp
local_ip = 127.0.0.1
local_port = 1235
remote_port = 1235

[BT Listen TCP]
type = tcp
local_ip = 127.0.0.1
local_port = 22223
remote_port = 22223

[BT Listen UDP]
type = udp
local_ip = 127.0.0.1
local_port = 22223
remote_port = 22223
```

新建一个 `awa.bat` 文件内容为

```batch
@echo off
title=开始启用内网穿透
mode con cols=100 lines=54&color 2&color 5B
echo 准备启用内网穿透服务
pause
echo 内网穿透服务启动中，预计3秒，请稍候……
frpc.exe -c frpc.ini
echo 如有报错请复制文件记录输出数值以便交流讨论。
pause
```

此时端口已经可以正常为TCP端口打通，但是由于BT客户端，tracker发起对外请求时候，还是用的是自身网络，没有经过frp，所以他人获取的ip是自身未经过内网透传的ip而不是frp服务器ip  
**解决办法一** 
需要在服务器中同时安装其它代理，或者隧道软件，例如v2，并且v2客户端打开本地监听端口功能

如果BT客户端不支持设置代理功能，可以使用另一款软件，netch为整个exe进程进行加速，这种方式会消耗较多vps服务器的流量，此时建议换比特彗星这种支持内置代理功能的BT客户端

使用比特彗星客户端scock5代理功能设置

仅用于穿透