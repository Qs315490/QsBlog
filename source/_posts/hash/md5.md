---
title: MD5算法
date: 2022-04-24 13:45:26
tags: [hash,md5]
categories: [hash]
---

MD5是数据摘要算法

## 特征
JS库名称为`MD5`,常见文件名为`md5.js`
```js
var hashCode = md5("原文");
```

128位（16字节）的hash值，固定长度（32或64位）
一个数据的MD5值是唯一的
一般为 `0-9` 和 `a-f(不区分大小写)` 组成

## 破解方法
### 暴力破解
使用`John`或`HashCat`
一般无需破解，大部分场景直接使用密文即可。