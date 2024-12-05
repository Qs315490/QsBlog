---
title: openwrt小技巧
date: 2024-12-05 15:32:54
tags:
categories:
---
# 更新需升级的软件包
记录 OpenWrt/LEDE 下更新所有待更新软件包的方法。
```sh
opkg update
opkg list-upgradable | cut -f 1 -d ' ' | xargs opkg upgrade
```