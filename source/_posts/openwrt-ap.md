---
title: "[Tank All in One] ESXi + Openwrt AP 篇"
date: 2024-04-07 00:45:07
tags:
- Tank All in One
- ESXi
- Openwrt
categories: 
- 技术
cover: https://s2.loli.net/2024/04/07/C8grlQyY7qPaGeW.png
---
# [Tank All in One] ESXi + Openwrt AP 篇

## 前言

刚来香港的时候，跟 Semilt 一起入手了一个 D-1518 的主板 Tank，经过一系列的去华强北买东西折腾，终于凑齐了一台 16 核心 32 线程，32 G 内存，512 G 固态硬盘的机子，并且最高可以支持 128 G 内存，另外还便宜收了一块 3060 安在上面，爽得我原地打滚。但是由于我们租的屋子的路由器离我屋太远，导致一直没办法好好搞一搞。后来机缘巧合之下，我突然想到引一根网线直接把我的 Tank 变成AP，岂不美哉，于是我开始了我的漫漫 All in One 之路......

有一说一，最一开始的计划是用 Ubuntu + Docker 实现 Openwrt、黑群晖等一系列系统做 All in One。后来因为种种原因放弃了，而且 Semilt 又告诉我一个新方案——ESXi，也就是一个虚拟机系统，把我所有想要安装的系统作为虚拟机安装到这上面，就真的能 All in One 了。于是，在经过了复杂的折腾以后，我终于装上了 ESXi，开启了我的 All in One 之路。不过没想到，这才只是折磨的刚刚开始......

## 一、网卡的坑

我跟 Semilt 之前在华强北第一次给 Tank 选择配置的时候，使用的是 Intel 的 AX 200。当时买的时候完全没考虑未来它要承担起作为 AP 的重任。于是，就翻车了。因为 Intel 的无线网卡作为 AP 异常别扭，因为某种未知的原因，不是网速慢，就是因为国家/地区信息频繁重置导致 AP 崩溃，甚至当我使用 ESXi 进行直通后，进入 Openwrt 也识别不到它是一款无线设备。于是就我的 Openwrt AP 计划就被搁浅了。直到我的小组作业们好不容易取得阶段性胜利后，我才有时间再去华强北购置了一块 MT 7922。于是上机后，我开始了折腾。

首先我得把我新买的网卡直通到 Openwrt 里，结果发现直通后，虚拟机根本启动不了，提示：

> 由于硬件或软件支持不可用，因此无法为9:0:0注册设备pciPassthru2

我百思不得其解，于是直接百度，找了很久以后找到了一个对于我最有帮助的帖子：[esxi直通无线网卡给openwrt报错无法注册设备](https://hqyman.cn/post/5637.html)。终于按照其中的解释成功将虚拟机启动。我猜应该是因为我之前的网卡用的也是这个所谓的 `9:0:0`，导致我新网卡的直通映射还保留着以前网卡的记录所以没能对应上（是我猜的，不太确定——不过问题最终是解决了）。

## 二、 Openwrt 中设置 AP

Openwrt固件我使用的是 iStore，由于屋里网一直太差，我一直都用的是流量，于是我心情非常激动地就开始进行了设置。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://s2.loli.net/2024/04/07/TzBVPDvycRKShfl.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图1</div>
</center>

一开始我用的是 Legacy 模式，虽然也是 5 Ghz 的频率，但是网速也是慢的一批。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://s2.loli.net/2024/04/07/sYZNAgB1p2r4tw6.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图2</div>
</center>

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://s2.loli.net/2024/04/07/IyVdJ8KRmjWn2ar.jpg">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图3</div>
</center>

于是我改成了 AX，信道设置了40，通道宽度 80 MHz，终于达到了我满意的网速。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://s2.loli.net/2024/04/07/JD5ONXx6t4KpMdo.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图4</div>
</center>

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://s2.loli.net/2024/04/07/i9SZbP8HFKIjYTq.jpg">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图5</div>
</center>

Openwrt AP 的设置就先告一段落了，我后续或许会继续搞 Tank 的 Openwrt 作为旁路由或软路由的其他有意思的操作，届时也会记录下来。