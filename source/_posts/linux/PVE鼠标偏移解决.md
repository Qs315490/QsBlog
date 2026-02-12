---
title: PVE鼠标偏移解决
tags: [PVE, spice, 鼠标偏移]
categories: [linux, PVE]
description: 'spice鼠标偏移，在虚拟机中鼠标会飘'
date: 2026-02-12 21:53:37
updated: 2026-02-12 21:53:37
---
# PVE鼠标偏移解决
spice鼠标偏移，在虚拟机中鼠标会飘，在 `/etc/pve/qemu-server/xxx.conf` 中加入：
```yml
tablet: true
```
或shell中执行：
```shell
qm set xxx -tablet true
```
然后重新启动即可。 `xxx` 是虚拟机 id
