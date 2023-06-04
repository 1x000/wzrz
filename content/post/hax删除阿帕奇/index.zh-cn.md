---
title: 卸载Apache进阶版
description: 使用HAX的坑
date: 2022-11-11
slug: web-hax
image: hax.png
categories:
    - web
    - linux
tags:
    - apache fk
keywords: "apache,uninstall"
lastmod: 2023-04-04 13:16:00
---

# 删除apache

[hax](https://hax.co.id/)的[openvz](https://openvz.org/)，系统默认安装了apache2现在就来卸载此服务。

## 1、查找web服务

使用如下命令：

> `dpkg -l | grep apache2`

## 2、删除apache2

删除命令如下：

`apt-get --purge remove apache2`

`apt-get --purge remove apache2-doc`

`apt-get --purge remove apache2-utils`

`apt-get --purge remove apache2-bin`

`apt-get --purge remove apache2-data`

## 3、**删除多余的文件**

上面的执行完之后，执行如下的命令：

`find /etc -name "apache" |xargs  rm -rf`

`rm -rf /var/www`

`rm -rf /etc/libapache2-mod-jk`

## 4、最后

80端口释放，没有问题了。
