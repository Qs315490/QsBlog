---
title: PVE修改Logo
tags: [KVM, PVE, Logo]
categories: [linux, PVE]
description: '修改虚拟机启动时显示的Logo与web界面Logo'
date: 2026-03-14 13:00:31
updated: 2026-03-14 13:00:31
---
# 修改虚拟机启动时显示的Logo
替换 `/usr/share/qemu-server/bootsplash.jpg` 文件
# 修改web界面Logo
修改`/usr/share/pve-manager/images/`下文件
文件名|显示位置
-|-
favicon.ico|浏览器标签页
logo-128.png|
proxmox_logo.png|页面左上角

# 文件说明
文件名|格式
-|-
bootsplash.jpg|640x480 像素
favicon.ico|64x64 像素
logo-128.png|128x128 像素，带有透明背景
proxmox_logo.png|172x30 像素，带有透明背景