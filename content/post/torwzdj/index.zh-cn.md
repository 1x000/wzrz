---
title: Tor网站搭建
slug: torwzdj
description: Tor网站搭建和Tor域名免费申请方法
date: 2023-02-25
image:
categories:
    - web
tags:
    - tor网站搭建
    - 网站搭建
    - 技术探讨
keywords: "tor服务搭建测试"
lastmod: 2023-04-04 07:43:58
---

# 暗网网站搭建和暗网域名申请方法

> 不要用来违法，只用于学习网站搭建方法

## 安装Tor服务

`Debian`和`Ubuntu`可以通过`apt`直接安装`tor`服务

```bash
apt install tor
```

如果用不了请进行以下方法操作

## 手动安装

### 修改一下`source.list`

```bash
sudo vi /etc/apt/sources.list  
```

### 最后面加下面两行

```ini
deb https://deb.torproject.org/torproject.org stretch main
deb-src https://deb.torproject.org/torproject.org stretch main
```

### 输入命令添加密钥

```bash
curl https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc | gpg --import
gpg --export A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89 | apt-key add -
```

### 安装Tor

```bash
apt update
sudo apt-get install tor
```

### dns 选择




| 默认                 | 备选                                                                 |
| ------------------ | ------------------------------------------------------------------ |
| deb.torproject.org | deb-master.torproject.org和mirror.netcologne.de和tor.mirror.youam.de |

## 编辑 tor 配置文件

```bash
vi /etc/tor/torrc
```

### 添加一下

```ini
HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort 80 127.0.0.1:80
```

### 启动 tor服务

```bash
sudo service tor start
```

**启动了之后，你的域名也就生成了**

### 查看域名

```bash
cat /var/lib/tor/hidden_service/hostname
```

### 浏览网站

之后你就可以通过在 tor 浏览器客户端去访问你的网站了

## 暴力运算获取好看的 Onion 地址

`.onion`的域名生成是私钥 →（RSA）→ 公钥 →（SHA1）→ 杂凑值 →（BASE 32 编码）=得到很丑的地址.onion。这跟BTC（比特币）生成的地址是同样的，因为计算量太大所以大家都会折衷，只要地址前几位顺眼就好。

原理是，使用高性能运算，不停的尝试不同的私钥输入，直到出现满意的结果，例如 **Facebook**的洋葱地址 `facebookcorewwwi.onion`。相当的整齐，要爆破这么多，大概也只有 **Facebook** 等拥有大规模服务器集群的厂商可以做得到，因为在密码学上的评估要暴力解出这样的结果以 `1.5Ghz` 处理器要 `260` 万年，惊为天人 **Facebook** 展现出庞大运算的惊人实力，这是暗网目前最漂亮的域名了。

**GITHUB**项目地址：

https://github.com/katmagic/Shallot

[GitHub - lachesis/scallion: GPU-based Onion Hash generator](https://github.com/lachesis/scallion)

[GitHub - cathugger/mkp224o: vanity address generator for tor onion v3 (ed25519) hidden services](https://github.com/cathugger/mkp224o)

如果运行过程中缺了什么套件沒有办法运行

可以输入以下命令：

```bash
sudo apt-get install libssl-dev
```


