---
title: AES/DES
date: 2022-04-24 14:00:26
updated: 2024-08-25
tags: [加密算法,AES,DES]
categories: [加密算法]
---

AES/DES是对称加密算法

# 特征
JS库名称为`CryptoJS`，常见文件名`crypyo-js.js`
```js
var Key = '12345678' //密钥
var Msg = 'WoW' //原文
var encrypt = CryptoJs.DES.encrypt(Msg, CryptoJs.enc.Utf8.parse(Key),{
	mode: CryptoJs.mode.ECB
	padding: CryptoJS.pad.Pkcs7
}).toString(); //加密
var decrypt = CryptoJS.DES.decrypt(encrypt,CryptoJs.enc.Utf8.parse(Key),{
	mode: CryptoJs.mode.ECB
	padding: CryptoJS.pad.Pkcs7
}).toString(CryptoJS.enc.Utf8); //解密
```

{% tabs qubie %}
<!-- tab AES -->
加密后密文长度是16的整数倍
AES类型|密钥或IV位数
-|-
AES-128|16 bytes
AES-192|24 bytes
AES-256|32 bytes
<!-- endtab -->

<!-- tab DES -->
加密后密文长度是8的整数倍
<!-- endtab -->
{% endtabs %}
加密后为二进制，常使用base64编码
padding填充模式常见值为`CryptoJS.pad.Pkcs7`

在二进制逆向中，可以通过AES加密矩阵判断程序是否使用AES加密算法。

# 破解方法
## 暴力破解
使用`John`或`HashCat`

DES的猜测的密钥数量为 2<sup>密钥位数</sup> 位
