---
title: "Nmap"
date: 2022-1-20
updated: 2022-1-20
tag: [linux,kali_tools]
categories: [linux,kali]
---
# 参数

```sh
nmap [参数] [主机名/IP/CIDR]
```

root用户无参数默认-sS扫描，普通用户默认-sT扫描

-sS TCP SYN扫描

-sU UDP 扫描

-p 1-65535 指定端口扫描,起始和结束将被忽略

-v 显示扫描过程

-F 快速扫描

-Pn 禁止ping的服务器扫描，跳过主机发现，默认主机存在

-A 全面扫描，显示 -sV 和 -O 信息

-sV 扫描服务/版本信息

-O 操作系统扫描

-iL 文件 指定存有IP的文件

|端口状态||
|--|--|
|open|打开|
|close|关闭|
|filterde|被限制，比如防火墙拦截|
