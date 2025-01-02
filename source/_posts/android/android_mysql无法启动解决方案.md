---
title: "Android下MySql无法启动解决方案"
date: "2020/1/1"
updated: "2020/1/1"
description: "MySql是Linux下的数据库服务器，在部分安卓手机上运行可能会报错。"
tag: [linux,android,server,mysql]
categories: [android]
---

# 前言
由于安卓系统对Linux内核的修改，在使用手机搭建服务器的时候可能会遇到Mysql无法启动的情况 (Mariadb也是)。

这可能是因为安卓内核安全机制, 将用户加入`aid_inet`用户组才可以联网.

数据库因为没有网络权限而报错, 进而导致无法启动

# 解决方案

在终端输入
```sh
usermod -a -G aid_inet 数据库的用户名
```
或者重新编译内核, 关闭安全相关选项.(这个偏麻烦一些,而且手机厂商一般不公布内核源码)

同理PHP无网络权限也可以试试这个方案. 