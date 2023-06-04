---
title: 博客常用命令（离线用）
description: 记录一些常用命令作为无网络的编辑
date: 2022-12-28
slug: blog-ml
image:
categories:
    - kankan
tags:
    - blog
keywords: "博客命令"
lastmod: 2023-04-04 12:20:12
---

# 博客命令记录

## 主题相关

FrontMatter 字段含义

```html
title: 标题
description: 描述
image: 显示的特色图片
comments: 显示 / 隐藏评论区(T/F)
license: 文章协议 输入 false 可以隐藏
hidden: 隐藏文章（不在首页，归档等页面显示，但是可以直接通过链接访问）
math: 是否加载 KaTeX 脚本
slug: 固定链接
draft: 是否为草稿
categories: 分类
tags: 标签
lastmod: 上次更新时间
```

## MarkDown相关

```html
# 我是一级标题
## 我是二级标题
可对应1-6级标题

*斜体文本*
**粗体文本**
***粗斜体文本***

一行中用三个以上的星号*、减号-、底线_来建立一个分隔线
~~删除线~~
<u>带下划线文本</u>

创建脚注格式类似这样 [^JiaoZhu]。
[^JiaoZhu]: 脚注的内容

列表：*、+、-、1.
> 区块
[链接文本](网址) 图片在前加!

|  表头   | 表头  |
|  ----  | ----  |
| 单元格  | 单元格 |
| 单元格  | 单元格 |
-: 设置内容和标题栏居右对齐。
:- 设置内容和标题栏居左对齐。
:-: 设置内容和标题栏居中对齐。
```

# 简码

Stack 带有一组[简码](https://gohugo.io/content-management/shortcodes/)，您可以在您的内容中使用这些简码。

此页面仅包含特定于 Stack 的简码。Hugo 的内置短代码记录[在此处](https://gohugo.io/content-management/shortcodes/#use-hugos-built-in-shortcodes)。

## 哔哩哔哩视频

嵌入[哔哩哔哩](https://www.bilibili.com/)视频。

```html
{{< bilibili VIDEO_ID PART_NUMBER >}}
```

可以`Video_ID`在视频的 URL 中找到。例如 的视频ID`https://www.bilibili.com/video/av12345678`为`av12345678`。`AV`和均受`BV`支持。

是`PART_NUMBER`可选的。它可用于指定要播放的视频部分。例如，零件号`https://www.bilibili.com/video/av12345678?p=2`是`2`。

## 腾讯视频

嵌入[腾讯视频](https://v.qq.com/)视频。

```html
{{< tencent VIDEO_ID >}} 
```

可以`Video_ID`在视频的 URL 中找到。例如 的视频ID`https://v.qq.com/x/cover/hzgtnf6tbvfekfv/g0014r3khdw.html`为`g0014r3khdw`。

## Youtube 视频

嵌入[YouTube](https://www.youtube.com/)视频。

```html
{{< youtube VIDEO_ID >}}
```

可以`Video_ID`在视频的 URL 中找到。例如 的视频ID`https://www.youtube.com/watch?v=VIDEO_ID`为`VIDEO_ID`。

## 通用视频文件

嵌入视频文件

```html
{{< video VIDEO_URL >}}

{{< video src="VIDEO_URL" autoplay="true" poster="./video-poster.png" >}}
```

可以`VIDEO_URL`是 URL 或相对于目录的路径`static`。例如，`src="/video/my-video.mp4"`将嵌入您站点文件夹的视频文件`static/video/my-video.mp4`。

该`autoplay`属性是可选的。它可用于指定是否应自动播放视频。该`poster`属性是可选的。它可用于指定视频的海报图像。

## GitLab

嵌入[GitLab](https://gitlab.com/)片段。

```md
{{< gitlab SNIPPET_ID >}}
```

可以`SNIPPET_ID`在片段的 URL 中找到。例如，代码段 ID`https://gitlab.com/-/snippets/1234567`是`1234567`。

## 引用

```html
{{< quote author="A famous person" source="The book they wrote" url="https://en.wikipedia.org/wiki/Book">}}
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
{{< /quote >}}
```

通过上述组织，您可以像这样在您的内容中插入图像：

```md
--- content/post/my-first-post/index.md ---
![Image 1](image1.png)
![Image 2](image2.png)
```

## 插入图库

要插入图片库，您需要为图片库创建一个页面包。例如：

```md
content
└── gallery
    └── my-first-gallery
        ├── index.md
        ├── image1.png
        ├── image2.png
        └── image3.png
```

```md
--- content/gallery/my-first-gallery/index.md ---
![Image 1](image1.png) ![Image 2](image2.png)
![Image 1](image3.png)
```

它将呈现两行，第一行有两张图像，第二行有一张图像。

### 高斯模糊

在你的文章中直接输入

```html
<span class="blur">是高斯模糊呐~ </span>
```

### 黑幕效果

在你的文章中直接输入

```html
<span class="shady">诶嘿嘿，我藏好了哦</span>
```

### 坑

这样一搞的话回车键/Tab的换行就完蛋了= =, 可以用`<br>`放在行末尾来换行

```html
<span class="blur">行1 <br>
行2 <br>
行3</span>
```

## 因为它没用= = 即答)

~~别打我 逃)~~

https://github.com/Li4n0/hugo_encryptor

https://xuzi.vercel.app/archive/1638860110/

## 安装hugo\_encryptor

[hugo\_encryptor](https://github.com/Li4n0/hugo_encryptor)

## 安装依赖

```
pycrypto==2.6.1
beautifulsoup4==4.7.1
lxml==4.6.3
```

安装过程中`pycrypto`是最难安装的，折腾了我好久，因为它需要vc环境进行编译。

## 放置软链接

安装完成后把`\hugo_encryptor\shortcodes\hugo-encryptor.html`复制到博客的`layouts\shortcodes`路径下

## 加密文章编写

将希望加密的内容放置在`hugo-encryptor`标签里面

## 文章加密

> 加密原理其实就是对生成的public文件夹的静态文件中打上了`hugo-encryptor`标签的内容进行加密

## hugo生成静态页面

执行hugo即可,注意生成时应该先将旧的public文件夹的内容删除

## 进行加密

运行hugo-encryptor.py即可，注意运行hugo-encryptor.py文件要与public文件夹在同一级目录下 `还要把解密的decrypt.js放置到public的根目录下`

## 上传静态文件进行博客部署

将public发布

> 因为目前采取的是自动化部署，所以加密效果是不行的。

想看效果可以访问这个页面：

[解密效果查看](https://ziyan1215.github.io/daily/1638860110/)

## 效果展示

输入`PASSWORD`进行查看文章

## 本文链接：

[https://xuzi.vercel.app/archive/1638860110/](https://xuzi.vercel.app/archive/1638860110/)

## 前言

无意间看到了 [hugo\_encryptor](https://github.com/Li4n0/hugo_encryptor) 这个项目，为 Hugo 带来文章加密功能，虽然静态博客的加密可能不是那么完美，不过感觉这个也能拦住不少人了。所以尝试了一下，引入到自己博客中来。

[效果演示](https://blog.0e1.top/post/2019/03/this-is-hugo-encryptor/)

## 安装

安装过程项目里面作者介绍的挺清楚的，我直接搬过来好了（[原地址](https://github.com/Li4n0/hugo_encryptor/blob/master/README-zh_CN.md)）。我自己开始在 Windows 中尝试了，但是出现了各种乱七八糟的问题，边谷歌边解决但是最终还是有问题，我太菜了 ![😔](https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/svg/1f614.svg) 。没办法，只好进 Linux 解决了，虽然不熟却也没有遇到奇奇怪怪的问题。

**环境依赖**：Python3

1.  下载 Hugo-Encryptor 并安装其所需要的依赖库
    
    ```
    $ git clone https://github.com/Li4n0/hugo_encryptor.git
    $ cd hugo-encryptor
    $ pip install -r requirements.txt
    ```
    
2.  将 `hugo-encryptor.py`放入博客根目录
    
    ```
    $ cp hugo-encryptor.py /path/to/your/blog/hugo-encryptor.py
    ```
    
3.  将`shortcodes/hugo-encryptor.html`放入博客`shortcodes`目录
    
    ```
    $ mkdir -p /path/to/your/blog/layouts/shortcodes
    $ cp shortcodes/hugo-encryptor.html /path/to/your/blog/layouts/shortcodes/hugo-encryptor.html
    ```
    

## 使用方法

1.  使用 `hugo-encryptor`标签包裹需要加密文章内容[![](https://www.10101.io/usr/uploads/2021/06/1320048532.png)](https://www.10101.io/usr/uploads/2021/06/1320048532.png)
    
2.  生成网站
    
    ```
    rm -rf public #删除原来的 public 文件夹
    hugo #重新生成 public 文件夹
    ```
    
3.  进行加密
    
    ```
    python3 hugo-encryptor.py
    ```
    

至此，文章就已经被加密好了，然后把 public 文件夹内容上传到服务器就好了。

## 部署到 Netlify

因为我的博客托管在 Netlify 上，并没有用自己的服务器，所以想直接弄到 Netlify 上。仔细看了 Netlify 的文档，自己稍微折腾了一下弄成功了。

我在 Netlify 上选择的 `Build imge` 是 `Ubuntu Xenial 16.04 (default)`。

### 放置依赖下载文件及设定版本号

1.  博客项目根目录下放置`runtime.txt`文件，内容设定为 Python 版本号，例如 `3.7` ；
    
2.  项目根目录下放置 `requirements.txt` 文件，内容为
    
    ```
    pycrypto==2.6.1
    beautifulsoup4==4.7.1
    lxml==4.3.3
    ```
    
3.  Netlify UI 中，在 **Settings** -> **Build & deploy** -> **Build environment variables** 中添加新的变量 `RUBY_VERSION`，值为 `2.6.2`（该 image 默认值，减少依赖下载）。
    

### 放置 `py` 及 `html` 文件

该步骤和上面[安装](https://www.10101.io/2019/04/17/encrypt-content-in-hugo#%E5%AE%89%E8%A3%85) 2、3 步相同，将对应文件放置在对应位置即可。

### 创建 `deploy.sh` 脚本

1.  在博客项目根目录下创建该脚本，内容为
    
    ```
    #!/bin/bash
    rm -rf public #删除 public 文件夹
    hugo #生成 public
    python3 hugo-encryptor.py #加密文章
    npm run algolia #更新 algolia 数据
    ```
    
    algolia 为博客提供搜索（详见[采用 ALGOLIA 作为 HUGO 搜索方案](https://www.10101.io/2018/11/23/search-with-algolia-in-hugo/)），顺便一起加在里面，每次部署都更新索引数据。
    
2.  在 Netlify UI 中，找到 **Settings** -> **Build & deploy** -> **Continuous Deployment** -> **Build settings** -> **Build command**，将其改为 `./deploy.sh`。
    

这样，每次都能自动部署了。当然了，除了这种办法，还可以手动拖动 `public` 文件夹至 Netlify UI 的 **Deploy** 页面或者通过 Netlify-cli 直接部署 `public` 文件夹内容。相比较而言，上面的方式更加省事、自动化。

## 问题及解决

在部署成功后，发现表格内容被 `hugo-encryptor` 标签包裹之后无法正常解析。因此，我提交了一个 [issue](https://github.com/Li4n0/hugo_encryptor/issues/1)，最终在作者的帮助下找到了[解决方法](https://github.com/Li4n0/hugo_encryptor/issues/1#issuecomment-484164103)。如遇到相同情况的可以参考一下。

awa

mermaid: true #是否开启mermaid