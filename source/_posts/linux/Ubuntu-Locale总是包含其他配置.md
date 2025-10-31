---
title: Ubuntu Locale总是包含其他配置
tags: [ubuntu,linux,locale]
categories: [linux, locale]
description: ''
date: 2025-10-31 18:40:44
updated: 2025-10-31 18:40:44
---
# 问题描述
locale-gen 生成的locale总是包含其他配置，例如：
```bash
zh_CN.UTF-8
en_US.UTF-8
en_GB.UTF-8
```
但是locale.conf中只配置了其中一个:`zh_CN.UTF-8`

# 原因
Ubuntu默认会安装 `language-pack-en`, 这些语言包会让locale-gen生成对应的locale  
对应 `/var/lib/locales/supported.d/en` 文件，这里面就是没配置但会生成的locale

# 解决方法
卸载 `language-pack-en`，然后重新生成locale  
或者直接编辑 `/var/lib/locales/supported.d/en` 文件，删除不需要的locale