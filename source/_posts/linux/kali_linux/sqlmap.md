---
date: "2022/1/20"
title: "sqlmap"
tag: [linux,kali_tools]
categories: [linux,kali]
---
# 介绍

sqlmap是一个sql漏洞扫描和注入工具

# 参数

|参数|作用|
|--|--|
|-v/--version|显示当前sqlmap版本|
|-u URL/--url=URL|sql注入检测|
|-d 参数|直接连接数据库|
|-m 文件|从多行文本文件里读取多个目标|
|--data=“DATA”|要传递的POST参数，列：“id=1”|
|--cookie=“COOKIE”|要传递的cookie，列：“PHPSESSID=a8d127e.....”|
|-f/--fingerprint|返回数据库指纹|
|-b/--banner|返回数据库banner|

# 实列

## sqlmap直接连接数据库

```sh
# 服务型
sqlmap -d "mysql://USER:PASSWORD@DBMS_IP:DBMS_PORT/DATABASE_NAME" 其他参数
# 文件型
sqlmap -d "DBMS://DATABASE_FILEPATH" 其他参数
```