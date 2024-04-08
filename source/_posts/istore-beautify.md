---
title: "[Tank All in One] iStore 美化篇"
date: 2024-04-08 20:04:16
tags:
- Tank All in One
- ESXi
- Openwrt
categories: 
- 技术
- 瞎坤八折腾
cover: https://s2.loli.net/2024/04/08/3wWdS8sAjcrEZUv.jpg
---
# [Tank All in One] iStore 美化篇

## 一、给 iStore 登录页面换个背景图片

之前安装好 iStore 以后，发现登录页面的背景图片非常朴素。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://s2.loli.net/2024/04/08/N96gSWrybVQH5q8.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图1 原本的壁纸</div>
</center>

作为一个颜狗，这种事情是绝对不允许发生的。

于是，我就开始瞎琢磨这玩意该怎么换。

------分割线------

首先，我得去安装 iStore OS 的虚拟机下找他这个 Web UI 到底放哪了，才能找到在哪更换背景图片。

然后我就找到了：`scp bg1.jpg root@192.168.0.118:/www/luci-static/argon/img
`，这个路径下有个叫 `bg1.jpg` 的图片，显然他长得很像背景图片。

接着再删除之前的背景图片：

```bash
cd scp bg1.jpg root@192.168.0.118:/www/luci-static/argon/img
rm bg1.jpg
```

最后再通过 `scp bg1.jpg root@192.168.x.x:/www/luci-static/argon/img` 把我自己电脑桌面上的 `bg1.jpg` 传到我这个 iStore 虚拟机上的指定位置。

最终效果图：

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://s2.loli.net/2024/04/08/1KCY6ucyD5rJPbq.jpg">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图2 现在的绝美壁纸</div>
</center>

后续的话可能会考虑换成随机图片，到时候会继续更新。
