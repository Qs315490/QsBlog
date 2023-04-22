---
date: "2020/1/1"
title: "Android开机自启动ADB服务和开启ADB WiFi"
description: ""
tag: [linux,android,adb]
categories: [android]
---

编辑 `/system/build.prop` 往文件写入以下代码即可
```ini
# 开启adb wifi
service.adb.tcp.port=5555
# 开启adb
persist.service.adb.enable=1
```