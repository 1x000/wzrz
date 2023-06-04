---
title: "自建tracker方法"
slug: "bt-zyzf"
description: "自建tracker服务器用于文件分享" 
date: 2023-01-10
image:
categories:
    - tool
    - linux
tags:
    - bt下载
keywords: "bt,私人tracker搭建"
lastmod: 2023-04-04 13:16:00
---

# 自建tracker服务器用于文件分享

众所周知bittorrent是一款P2P文件传输协议，能够让两台内网机器实现点对点的文件传输。

随着各种资源的丰富，bit下载也越来越为大众所熟知。

我们都知道，两台内网机器，直接使用http或者udp是无法进行文件分享的。

要么使用网盘，有限速和和谐风险。

要么使用服务器FTP转发，但是硬盘和带宽成本比较高。

要么使用P2P同步工具，但是要安装专用的软件。

故这里尝试使用BT协议做文件分享，即将要分享的文件打包成种子，然后将种子传给对方。大部分人都是有BT下载工具的。

由于共有tracker存在安全隐患，本篇文章就是介绍如何搭建私有tracker服务器。

## 在VPS上使用bittorrent-tracker

这是`webtorrent`项目的一个部分。使用`nodejs`进行搭建部署。

[官方](https://github.com/webtorrent/bittorrent-tracker)有英文文档，这里我简单说一下如何使用：

首先要下载`nodejs`和`npm`：

```bash
apt install nodejs
apt install npm
```

然后安装`bittorrent-tracker`包：

```bash
npm install bittorrent-tracker
```

新建一个`server.js`文件，设置一个简单的服务器，更多可以参考官方文档

```js
    // Create a web sockets signaling server
    let lookup = {}

    const hostname = '0.0.0.0';
    const port = process.env.PORT || 3001;


    var Server = require('bittorrent-tracker').Server

    var server = new Server({
        udp: false, // enable udp server? [default=true]
        http: true, // enable http server? [default=true]
        ws: true, // enable websocket server? [default=true]
        stats: true, // enable web-based statistics? [default=true]
    })

    server.on('error', function (err) {
        // fatal server error!
        //console.log(err.message)
    })

    server.on('warning', function (err) {
        // client sent bad data. probably not a problem, just a buggy client.
        //console.log(err.message)
    })

    server.on('listening', function () {
        // fired when all requested servers are listening
        console.log('Signal server http port:' + server.http.address().port)
        console.log('Signal server ws port:' + server.ws.address().port)
    })

    // listen for individual tracker messages from peers:
    server.on('start', function (addr) {
        console.log('got start message from ' + addr)
        Object.keys(server.torrents).forEach(hash => {
            lookup[server.torrents[hash].infoHash] = server.torrents[hash].peers.length
            //console.log("peers: " + server.torrents[hash].peers.length)
        })
    })

    server.on('complete', function (addr) {})
    server.on('stop', function (addr) {})

    // start tracker server listening! Use 0 to listen on a random free port.
    server.listen(port, hostname, 'listening')
```

然后运行即可：

```bash
node server.js
```

默认的使用端口为`3001`

```bash
root@lelfeng:~/tracker# node server-test.js
Signal server http port:3001
Signal server ws port:3001
```

访问对应地址，返回：

```bash
d14:failure reason33:invalid action in HTTP request: /e
```

这种方法不如`Docker`然后也老
