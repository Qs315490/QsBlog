---
title: Windows 小技巧
date: 2023-12-30 23:12:14
tags: [Windows, 小技巧]
categories: [Windows, 小技巧]
---

# 知道IP查询内网设备的主机名
```cmd
ping -a <IP地址>
```

# 查询指定端口占用情况
```cmd
netstat -ano | findstr <端口号>
tasklist | findstr <返回的PID>
```