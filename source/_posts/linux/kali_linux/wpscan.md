---
title: "WPscan"
date: 2022-1-20
updated: 2022-1-20
tag: [linux,kali_tools]
categories: [linux,kali]
---
# 介绍

Wordpress专用扫描器

# 参数

|参数|用途|
|--|--|
|--api-token|添加apikey扫描更详细，官网注册|
|--url|后面加要扫描的站点|
|-e/--enumerate|枚举|
|u|用户名|
|p|枚举插件|
|ap|枚举所有插件|
|vp|枚举有漏洞的插件|
|t|枚举主题|
|at|枚举所有主题|
|vt|枚举有漏洞的主题|
|-w/--wordlist|后面加字典|
|-U/--username|指定用户|
|--update|更新漏洞数据库|
|-o|导出结果到文件|

# 实例

## 扫描站点

```sh
wpscan --url http://localhost/wordpress
```

## 对主题进行扫描

```sh
wpscan -u http://localhost/wordpress/ -e t
```

## 扫描主题存在的漏洞

```sh
wpscan -u http://localhost/wordperss -e vt
```

## 扫描已安装的插件

```sh
wpscan -u http://localhost/wordperss -e p
```

## 枚举用户

```sh
wpscan -u http://xxx -e u
```

## 暴力破解

```sh
wpscan -u http://xxx -w 密码字典 -U 用户名或用户字典
```
