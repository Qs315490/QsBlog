---
title: Base64编码
date: 2022-04-24 15:50:46
updated: 2023-11-9 19:45
tags: [加密算法,编码,Base64]
categories: [加密算法]
---

Base64不是加密算法, 是一种编码

# 特征
```js
var encode = Base64.encode("原文"); //加密
var decode = Base64.decode(encode); //解密
```

使用 `A-Z`，`a-z`，`0-9`，`+`，`/` 64个字符表示数据
末尾使用`=`补位，补位不超过两个