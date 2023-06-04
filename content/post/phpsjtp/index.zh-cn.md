---
title: Shortny PHP 短网址汉化版搭建
slug: dwzdj
description: Shortny PHP 短网址搭建（学习）
date: 2023-02-25
image:
categories:
    - web
tags:
    - 短网址搭建
    - 网站搭建
    - 技术探讨
keywords: "短网址搭建"
lastmod: 2023-04-04 10:43:58
---

# Shortny PHP 短网址汉化版搭建

> 不要用来违法，只用于学习网站搭建方法

## 下载源码

> 压缩密码：`J0OK0JwCDSDVfw`
> 
> [蓝奏云](https://pvphack.lanzoue.com/itfhZ0oian8j) 访问密码:`2b65`
> 
> 声明：本源码是学习汉化版的，请学习完了之后删掉源码或者去买正版

## 搭建教程

### 安装环境

**环境要求**：`PHP`、`Mysql`、`Nginx`/`Apache`

`web`**服务器**：`Apache`，`nginx`，`IIS`和`lighttpd`（首选Ngnix）

**PHP**要求：> =` 5.5.9`

**MariaDB**和**MySQL**> = `5.5`，或使用祖传`SQLite`

### 安装要求

**内存**：`512`M以上

### 伪静态设置

**Apache**需要编辑.htaccess文件，将本地地址修改成自己的域名

```php
RewriteEngine on
RewriteRule ^about about.php [L]
RewriteRule ^api-about api-about.php [L]
RewriteRule ^contact contact.php [L]
RewriteRule ^tos tos.php [L]
RewriteRule ^([^/.]+)/?$ link.php?id=$1 [L]
RewriteRule ^404 404.php [L]




Options -Indexes
ErrorDocument 404 http://你的域名/404
ErrorDocument 403 htNginxtp://你的域名/404
```

**Nginx**点击域名设置-伪静态，然后将下面的域名改成自己的，再复制

```php
rewrite ^/about /about.php last;
rewrite ^/api-about /api-about.php last;
rewrite ^/contact /contact.php last;
rewrite ^/tos /tos.php last;
rewrite ^/([^/.]+)/?$ /link.php?id=$1 last;
rewrite ^/404 /404.php last;
error_page 404 http://你的域名/404;
error_page 403 http://你的域名/404;
```
