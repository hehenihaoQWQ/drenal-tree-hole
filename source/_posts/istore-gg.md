---
title: "[Tank All in One] iStore 应用商店踩坑篇"
date: 2024-04-07 02:25:57
tags:
- Tank All in One
- ESXi
- Openwrt
categories: 
- 技术
---
# [Tank All in One] iStore 应用商店踩坑篇

之前刚搞定了 AP 问题，于是我开始思考 NAS 选用什么。我打算先用 iStore 应用商店里的 NAS 软件试一试水，然后打开了 iStore 想随便下载几个插件试试，于是：

> curl: (6) Could not resolve host: istore.linkease.com

我就很头疼啊，刚解决了 AP 的问题结果又搞出来幺蛾子。我开始 Ping `istore.linkease.com` 这个网址，发现报 `ping: bad address`。

百度一搜，说是 DNS 问题。于是我跑到 LAN 接口那去设置了阿里的 DNS，结果还是不行，我发现外网 IP 都 Ping 不出去。最后我试着把网关设置为 `192.168.0.1` (路由器地址)，最后终于可以安装了。但我不清楚我这 Openwrt 是不是给做成旁路由了，回头再研究研究。

