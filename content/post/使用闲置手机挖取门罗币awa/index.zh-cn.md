---
title: 使用闲置Android手机挖门罗币
description: 仅供学习安卓手机新用法
date: 2022-12-20
slug: Android-xmr
image:
categories:
    - 折腾手机
tags:
    - xmr
keywords: "手机挖取XMR币"
lastmod: 2023-04-04 06:24:34
---

# 利用闲置Android 机挖门罗币

## 什么是门罗币？

> 门罗币（Monero，缩写：XMR）是一个创建于2014年4月开源加密货币，它着重于私隐、分权和可扩展性。与自比特币衍生的许多加密货币不同，Monero 基于CryptoNote 协议，并在区块链模糊化方面有显着的算法差异。  

## 为什么是门罗币？

由于采用了反ASIC的挖取方式，无论是x86, x86-64, ARM 还是GPU 都可以获得不错的采掘效率。因此，废旧Android 手机多为ARM 架构，门罗币无疑是最适合采掘的货币。

## 为什么要在手机上挖矿？

这个问题实际上很难回答。不清楚其他人如何，但就我而言我拥有多部闲置的Android 手机。预期放在抽屉里吃灰，不如拿出来挖矿，榨干其最后一点剩余价值。

并且，相较于桌面级的通用处理器，手机所采用的处理器实际上拥有更高的效率。用手机评测中常讲的词汇来描述的话就是，它们的能耗比要远高于桌面级处理器。

做为参考，Snapdragon 845 满载仅5.3 W 的功耗，能够达到450 Hash/s，而I9-9880H 则在27W 功耗下取得了1.42 kH/s 的成绩。Snapdragon 845 仅使用后者20% 的电力即可获得超过后者30% 的性能。对比桌面级处理器时它们的效能优势将更加明显。

## 注意事项&免责声明

其实是常识性的事情，在手机在满负荷工作时将维持在较高的温度，而在高温时为手机充电则是相当消耗电池寿命的行为。长期保持这种状态将加速手机电池的老化。

当然，如果本来就是不会再使用的废旧手机的话自然不需要考虑这个问题，但如果您准备在您日常使用的手机上采掘的话，请注意这一点。

此外，本文章仅讨论如何采掘加密货币，并**不涉及任何加密货币投资建议**。

---

## 在开始之前

您需要拥有一个XMR 钱包。获取的方式有很多，您可以通过互联网检索来获取一个。

除此以外并没有什么其他需要准备的，只要拥有一部Android 手机和一个充电器即可。如果您有多部Android 手机准备用于采掘的话也许USB Charging Hub 会是个不错的选择。

## 编译一个挖矿程式

Android 毕竟是基于Linux 内核开发的操作系统，因此，任何Linux 客户端理论上来说都可以在Android 系统中运行。不过它们多数都没有Android 版本，甚至没有GUI，因此我们首先需要一个控制台程式。这里我选择的是Termux。此外，对于采掘程式，在这里选择了官方支援ARM 的XMRig 。

---

拥有了控制台，我们就可以从头开始编译一个Android 版的挖矿程式了。当然，这里的程式并不是我们在手机上常见到的那种拥有GUI 的「Application」，而是完全基于CLI 的「Program」。

---

Termux 提供了类似Ubuntu 的开发环境。我们可以首先运行 `apt update && apt upgrade` 来更新内置的dependencies。不过实际上不更新也完全不影响我们挖矿。

当然，仅靠Termux 内置的程式显然不足以完成编译，我们还需要安装一些其他的dependencies。使用pkg install 来安装它们。

`pkg install automake clang git vim cmake`

在开始安装之前系统会停下来向我们确认是否要继续，输入 `y` 并按 `Enter` 来确定。

---

待安装完成后，从GitHub 获取XMRig 的源码。

`git clone https://github.com/xmrig/xmrig`

获取完成后我们可以使用 `ls` 指令来确认目录。确认xmrig 目录存在后，移动到xmrig 目录。

`cd xmrig`

---

接下来创建build 目录并移动到build 目录。

`mkdir build && cd build`

在build 目录下创建Makefile。由于Termux 没有提供HWLOC 库的支援，因此需要选择将HWLOC 除外：

`cmake .. -DWITH_HWLOC=OFF`

---

创建好Makefile 后即可在本地编译XMRig 程式。

`make -j$(nproc)`

### Tips

Termux 是支援Copy & Paste 的。您不必手动输入例如网址等繁琐的指令，仅需Copy 它，并在Termux 中任意位置长按并选择Paste 即可。

## 开始挖矿吧

至此，已经可以开始在Android 手机上挖矿了！

如果您仍停留在build 目录下的话，可以直接使用 `./xmrig -o xmr.pool.minergate.com:45700 -u se17vk4ar7nsrtx -t 8` 指令开始挖矿，不要忘记将矿池和User ID 替换成您使用的矿池和User ID。这些参数的使用方法与其他平台的XMRig 是完全相同的，可以在[这个页面](https://xmrig.com/docs/miner/command-line-options)查看。

而如果您已经退出了Termux，可以使用指令 `cd xmrig/build` 来重新前往build 目录，或使用 `~/xmrig/build/xmrig` 来替代`./xmrig`。

### Tips

- 终止挖矿的操作与其他控制台相同，使用 `ctrl+c` 组合键。Termux 提供了虚拟 `ctrl` 按钮，按下后再按键盘的 `c` 键即可。
- 如果只是暂停挖矿，XMRig 提供了快捷键`p`。按下即可暂停。
- `h`键可以查看当前挖矿速率。
- 要完全终止Termux，需要在通知栏中点击Exit。返回桌面或在多任务中划掉它都不会终止正在运行的终端程式。
- Termux 与XMRig 并不会检测您的电源状态。如果您移除电源的话XMRig 将会快速耗尽您全部的电池余量。在移除电源后（或前）请一定记得暂停或停止挖矿活动。

---

诚然，尽管使用手机进行挖矿是一件非常「高效率」的事情，但从获得的XMR 数量来说并不够「高效率」。相同电力下它们能够达到更高的计算速度，这并不意味着它们很快。它们毕竟是手机处理器，计算速度还是非常慢的。因此您需要坐和放宽，甚至是忘掉它的存在——本就是闲置的Android 手机，相信您也不会常拿起它来。

您接下来只需要等着接收XMR 即可，尽管接收的周期可能会非常，非常的长。不过有总归是比没有要好的。