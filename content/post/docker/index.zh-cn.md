---
title: "Docker 资源整合"
slug: "docker-zy"
description: "学习awa" 
date: 2023-01-11
image: docker.png
categories:
    - 折腾电脑
    - linux
tags:
    - docker
keywords: "docker"
lastmod: 2023-04-04 13:16:00
---

# Docker

> 记录玩Docker的过程 

## Docker 安装

### 使用官方安装脚本自动安装

安装命令如下：

```bash
curl -fsSL https://get.docker.com | bash -s docker
```

也可以使用国内 `daocloud` 一键安装命令：

```bash
curl -sSL https://get.daocloud.io/docker | sh
```

## Docker基本命令

> Docker 镜像是由 Dockerfile 和一些必要的依赖项组成的，Docker 容器是动态的 Docker 镜像。要使用 Docker 命令，首先需要知道您是在处理镜像还是容器。一旦你知道你所处理的是镜像还是容器之后，你才可以找到正确的命令。

### 命令的共性

你需要知道一些关于 Docker 命令的规律：

- Docker CLI 管理命令以 docker 开头，然后是空格，接着是管理类别，然后是空格最后是命令。例如，docker container stop 这一命令可以停止容器。

- 引用特定容器或镜像的命令需要该容器或镜像的名称或 ID。

举个例子，docker container run my_app 是用于构建和运行名为 *my_app* 的容器的命令。在本文示例中，我将使用名称 my_container 来引用通用容器。同理，my_image、my_tag 也是如此。

我将单独提供命令和通用标志。前面有两个破折号的标志是标志的全名。带有一个破折号的标志是完整标志名称的缩写。例如，-p 是–port 标志的缩写。

本文的目标是让您牢牢记住这些命令和标记，并希望您可以在创建容器或构建镜像的时候可以将本指南作为参考。

### 容器命令

使用 docker container my_command

- create — 从镜像中创建一个容器

- start — 启动一个已有的容器

- run — 创建一个新的容器并且启动它

- ls — 列出正在运行的容器

- inspect — 查看关于容器的信息

- logs — 打印日志

- stop — 优雅停止正在运行的容器

- kill — 立即停止容器中的主要进程

- rm — 删除已经停止的容器

### 镜像命令

使用 docker image my_command

- build — 构建一个镜像

- push — 将镜像推送到远程镜像仓库中

- ls — 列出镜像

- history — 查看中间镜像信息

- inspect — 查看关于镜像的信息，包括层

- rm — 删除镜像

### 容器 &镜像

- docker version — 列出关于 Docker 客户端以及服务器版本的信息

- docker login — 登录到 Docker 镜像仓库

- docker system prune — 删除所有未使用的容器、网络以及无名称的镜像（虚悬镜像）

### 容器命令详解

#### 启动容器

术语“创建”，“开始”和“运行”在日常生活中都具有相似的语义，但每个都是一个独立的 Docker 命令，用于创建并/或启动容器。让我们先看看创建容器的命令。

**docker container create my_repo/my_image:my_tag** — 从一个镜像中创建容器

我将在下文中把 my_repo/my_image:my_tag 缩写为 my_image。

你可以通过传递许多标志来 create。

**docker container create -a STDIN my_image**

-a 是—attach 的缩写，指将容器连接到 STDIN，STDOUT 或 STDERR。

既然我们已经创建了一个容器，那么让我们来启动它。

**docker container start my_container** — 启动一个已有的容器

请注意，容器可以通过容器的 ID 或容器的名称来引用。

**docker container start my_container**

既然你知道如何创建和启动一个容器，让我们来看看最常见的 Docker 命令。它将 create 和 start 结合到一个命令中：run。

**docker container run my_image** — 创建一个新容器并且启动它。这一命令同样也有许多选项。让我们看看其中几个。

**docker container run -i -t -p 1000:8000 --rm my_image**

-i 是—interactive 的缩写，即使未连接，也要保持 STDIN 打开；-t 是—tty 的缩写，它会分配一个伪终端，将终端与容器的 STDIN 和 STDOUT 连接起来。

你需要指定-i 和-t 通过终端 shell 与容器交互。

-p 是 –port 的缩写。端口是与外部世界的接口。1000：8000 将 Docker 端口 8000 映射到计算机上的端口 1000。如果你有一个 app 输出了一些内容到浏览器，你可以将浏览器导航到 localhost:1000 并且查看它。

–rm 自动删除停止运行的容器。

让我们再来看看 run 的几个例子。

**docker container run -it my_image my_command**

sh 是你可在运行时指定的命令，它将在容器内部启动 shell 会话，你可以通过终端与其交互。对于 Alpine 镜像，sh 优于 bash，因为 Alpine 镜像不随 bash 一起安装。键入 exit 以结束交互式 shell 会话。

请注意，我们将-i 和-t 结合为-it。

**docker container run -d my_image**

-d 是—detach 的缩写，指在后台运行容器，允许您在容器运行时将终端用于其他命令。

#### 检查容器状态

如果你有许多运行中的 Docker 容器并且想要找到与哪一个互动，那么你需要列出它们。

**docker container ls** — 列出运行中的容器，同时提供关于容器有用的信息。

**docker container ls -a -s**

-a 是–all 的缩写，列出所有容器（不仅仅是正在运行中的容器）

-s 是—size 的缩写，列出每个容器的大小。

**docker container inspect my_container** — 查看有关容器的信息

**docker container logs my_container** — 列出容器日志

#### 终止容器

有时你需要停止一个正在运行中的容器，你需要用到以下命令：

**docker container stop my_container** — 优雅地停止一个或多个正在运行的容器。在容器关闭之前提供默认 10 秒以完成任何进程。

如果你觉得 10 秒太长的话，可以使用以下命令：

**docker container kill my_container** — 立即停止一个或多个正在运行的容器。这就像拔掉电视上的插头一样。但是在大多数情况下，建议使用 stop 命令。

**docker container kill $(docker ps -q)** — 终止所有运行中的容器

你需要删除容器可以使用以下命令：

**docker container rm my_container** — 删除一个或多个容器

**docker container rm $(docker ps -a -q)** — 删除所有不在运行中的容器

以上就是 Docker 容器的关键命令。接下来，我们来看看关于镜像的命令。

### 镜像命令详解

以下是 Docker 镜像使用的 7 条命令

#### 构建镜像

**docker image build -t my_repo/my_image:my_tag .** 在指定路径或 url 的 Dockerfile 中构建一个名为 *>my_image* 的 Docker 镜像。

-t 是 tag 的缩写，是告诉 docker 用提供的标签来标记镜像，在本例中，是 *my_tag*。

在命令末尾的句号（.）是告诉 Docker 根据当前工作目录中的 Dockerfile 构建镜像。

当你构建好镜像之后，你想要推送它到远程仓库中以便它可以共享并且在有需要的时候被拉取。那么下一个命令十分有用，尽管并非是镜像命令。

**docker login** — 登录到 Docker 镜像仓库，根据提示键入你的用户名和密码

**docker image push my_repo/my_image:my_tag** — 推送一个镜像到仓库。

你拥有了这些镜像之后，你可能想要检查他们。

#### 检查镜像

**docker image ls** — 列出你的镜像以及每个镜像的大小

**docker image history my_image** — 显示镜像的中间镜像，包括大小及其创建方式

**docker image inspect my_image** — 显示关于镜像的细节，包括组成镜像的层

有时候你还需要清理你的镜像。

#### 清理镜像

**docker image rm my_image** — 删除指定镜像。如果镜像被保存在镜像仓库中，那么该镜像在那依旧可用。

**docker image rm $(docker images -a -q)** — 删除所有镜像。必须小心使用这一命令。请注意已经被推送到远程仓库的镜像依然能够保存，这是镜像仓库的一个优势。

以上就是大部分与 Docker 镜像相关的重要命令。

要在使用 Docker 时查看 CLI 引用，只需在命令行中输入命令 docker 即可。Docker 文档请参阅：

[Use the Docker command line | Docker Documentation](https://docs.docker.com/engine/reference/commandline/cli/)  

现在你已经掌握了运行 Docker 的关键命令，可以用 Docker 了！

## Docker可视化 Portainer安装

**Portainer是Docker的可视化管理工具**，镜像、容器、日志显示等操作，Swarm集群、服务等集中管理和操作、登录用户管理和控制等功能。

### Portainer官网

Portainer官网：[https://www.portainer.io/](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.portainer.io%2F)

Portainer官网提供的安装文档：

1. [https://www.portainer.io/installation/](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.portainer.io%2Finstallation%2F)
2. [https://portainer.readthedocs.io/en/stable/deployment.html](https://links.jianshu.com/go?to=https%3A%2F%2Fportainer.readthedocs.io%2Fen%2Fstable%2Fdeployment.html)

### Docker上安装Portainer

**1、利用Docker拉取portainer镜像**

```bash
docker pull portainer/portainer
```

**3、运行启动portainer**

> **温馨小提示**： 这里需开放9000端口来映射portainer  
> /var/run/docker.sock:/var/run/docker.sock -> 映射本地docker路径  
> /home/docker/portainer:/data -> 实现数据持久化【原因：portainer的数据存储在容器内部的/data目录，容易导致容器重启的时候数据丢失，作用：将portainer数据映射到本地】  

```java
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v /home/docker/portainer:/data --name portainer --restart=always portainer/portainer
```

启动后通过 服务器地址:9000 即可访问portainer，第一次访问注意需要设置账号密码

### 总结

1. 不同的环境安装可根据官网给出的文档进行操作
2. 另外Docker的其它可视化管理工具还有：DockerUI、Shipyard、Rancher 等

## 使用 Docker 自建支持 DoH、DoT 的 DNS 服务器

DoH (DNS over HTTPS) 和 DoT (DNS over TLS) 有效避免了运营商的 DNS 监听和劫持，本文记录在公网搭建一个可以去广告的 DNS 服务器（借助 Adguard Home）。

### 前期准备

- 公网服务器一台
- 域名一个，并且指向该公网服务器
- 域名指向该公网服务器，并申请好 TLS 证书（可使用 Let’s Encrypt 申请）

### Docker 部署

Docker Hub 镜像地址：[Docker](https://hub.docker.com/r/adguard/adguardhome)

安装好 Docker 后下载镜像，以下任选其一

| 1  <br>2 | docker pull adguard/adguardhome:latest  # 稳定版  <br>docker pull adguard/adguardhome:edge  # 最新的版本，可能不稳定 |
| -------- | ------------------------------------------------------------------------------------------------------ |

新建配置目录和工作目录

    mkdir /root/adguardhome/workdir 
    mkdir /root/adguardhome/confdir

启动镜像，完成后进入配置页面：`HostIP:3000`

| 1  <br>2  <br>3  <br>4  <br>5  <br>6 | docker run --name adguardhome \  <br>  -v /root/adguardhome/workdir:/opt/adguardhome/work \  <br>  -v /root/adguardhome/confdir:/opt/adguardhome/conf \  <br>  --restart always \  <br>  -p 443:443 -p 853:853 -p 3000:3000 -p 53:53/udp -p 53:53/tcp \  <br>  -d adguard/adguardhome:edge |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

注意，第一次进入配置页面需要配置监听端口，配置好后注意修改容器启动时端口的映射，并以新的命令重新启动容器

### 上游服务器设置

进入 Adguard Home 控制台，找到「设置 - DNS 设置」

「上游 DNS 服务器」可填入公共 DNS 服务器

国内常用的公共 DNS 如下：

| DNS 服务器地址                          | 类型             | 提供商     |
| ---------------------------------- | -------------- | ------- |
| `https://dns.alidns.com/dns-query` | DNS over HTTPS | 阿里 DNS  |
| `tls://dns.alidns.com`             | DNS over TLS   | 阿里 DNS  |
| `https://doh.360.cn/dns-query`     | DNS over HTTPS | 360 DNS |
| `119.29.29.29`                     | 常规 DNS         | DNSPod  |
| `182.254.116.116`                  | 常规 DNS         | DNSPod  |
| `223.5.5.5`                        | 常规 DNS         | 阿里 DNS  |
| `223.6.6.6`                        | 常规 DNS         | 阿里 DNS  |
| `114.114.114.114`                  | 常规 DNS         | 114 DNS |

「Bootstrap DNS 服务器」填入一到两个常规 DNS 服务器就可以，如 `223.5.5.5` 和 `119.29.29.29`

在 「DNS 服务设定中」，勾选「使用客户端的子网地址（EDNS）」，可以使查询的客户端得到离其最近的网站 IP 解析。

### DoH & DoT 设置

进入「设置 - 加密设置」

勾选「启用加密」，填写准备的域名，「HTTPS 端口」和「DNS-over-TLS 端口」均可自定义，如有改正记得重新修改容器启动时的端口映射配置。

证书可以选择「设置证书路径」和「粘贴证书内容」，按要求配置。

配置完成后可以使用「`https://域名`」访问控制台

### 去广告设置

进入「过滤器 - DNS 封锁清单」

附一些常用的规则：

- HalfLife，规则合并自 EasylistChina、EasylistLite、CJX’sAnnoyance 合并规则：`https://gitee.com/halflife/list/raw/master/ad.txt`

- EasyPrivacy，隐私保护：`https://easylist-downloads.adblockplus.org/easyprivacy.txt`

- I don’t care about cookies，我不关心 Cookie 的问题，屏蔽网站的 cookies 相关的警告：`https://www.i-dont-care-about-cookies.eu/abp/`

- anti-AD，自称是目前中文区命中率最高的广告过滤列表：`https://gitee.com/privacy-protection-tools/anti-ad/raw/master/easylist.txt`

- 乘风广告、视频过滤规则：`https://gitee.com/xinggsf/Adblock-Rule/raw/master/rule.txt`、`https://gitee.com/xinggsf/Adblock-Rule/raw/master/mv.txt`
  
  **至此配置完成**

使用普通 DNS 解析，在客户端填入服务器的 IP 即可，若要使用 DoH 和 DoT 则需使用另外的工具。

最新版的 Chrome 已经支持使用 DoH 解析，在 Chrome 中找到「设置 - 安全 - 使用安全 DNS」，按规则填入域名即可

## Linux 53 DNS端口被 systemd-resolve 占用

1.停用 systemd-resolved 服务

```bash
systemctl stop systemd-resolved
```

编辑 `/etc/systemd/resolved.conf` 文件

```bash
vi /etc/systemd/resolved.conf
```

换下面说明更改，然后按一下“esc”键，再输入“:wq”（不要输入引号），回车保存

```ini
[Resolve]
DNS=8.8.8.8 #取消注释，增加dns
#FallbackDNS=
#Domains=
#LLMNR=no
#MulticastDNS=no
#DNSSEC=no
#Cache=yes
DNSStubListener=no #取消注释，把yes改为no
```

最后输入下面命令即可

```bash
ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

## 使用Docker安装OpenTracker，自建BT Tracker服务器

OpenTracker用于搭建Tracker服务器，据说该工具性能很强，就连全球最大的BT服务器海盗湾也在使用，有人专门制作了一个OpenTracker的Docker镜像，基本不到1分钟就可以搭建好一个BT Tracker服务器，有兴趣的也可以玩玩。

### 安装OpenTracker

### 运行命令：

```bash
docker run --name tracker-server -d -p 6666:6666 -p 6666:6666/udp moerats/tracker-server:latest
```

如果你服务器上的`6666`端口正在被使用，而`7777`没被使用，那就改成`7777:6666`参数后再运行。

对于某些系统可能还需要开启防火墙端口，比如CentOS 7，开启命令：

```bash
firewall-cmd --zone=public --add-port=6666/tcp --permanent
```

```bash
firewall-cmd --zone=public --add-port=6666/udp --permanent
```

```bash
firewall-cmd --reload
```

### 使用方法

我们在制作种子添加`Tracker`服务器时，可以填：

1. `http://IP:6666/announce`
2. 或者：
3. `udp://IP:6666/announce`

也可以同时添加http和udp。

统计地址：`http://IP:6666/stats`

更详细的统计地址：`http://IP:6666/stats?mode=everything`。

## Docker部署Github Proxy代理为Github文件下载加速

当前网络环境下Github要么不能访问，要么下载速度极慢，毫无体验可言，平时拉库等操作会失败，所以找到此办法，可有效解决以上问题。

### 必备条件：

一台可以无限上网的linux主机，比如国外的服务器，或者阿里云、腾讯云的海外/香港云主机

这是必须的前提

### Docker部署

```bash
docker run -d --name="gh-proxy-py" 
  -p 0.0.0.0:9183:9183 
  --restart=always 
  hunsh/gh-proxy-py:latest
```

第一个`9183`就是需要对外的端口，需要打开或映射出去

完成后可以浏览器打开：服务器地址:端口，如果出现页面，即代表成功

### 如何使用

部署成功后就可以使用了，方法也很简单，直接Github地址之前加你的服务器地址：端口（80可以省略）

比如服务器地址：`IP`   

端口是`9183`

就可以这样输入：`IP:9183/https://github.com/Cyberbolt/baota.git`
