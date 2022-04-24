---
title: RSA
date: 2022-04-24 15:14:19
tags: [加密算法,RSA]
categories: [加密算法]
---

RSA非对称加密
公钥加密, 私钥解密

## 特征
JS库名称为`JSEncrypt`,常见文件名为`jsencrypt.js`
```js
//加密
var encrypt = new JSEncrypt();
encrypt.setPublicKey("公钥");
var encrypted = encrypt.encrype('原文');//加密
//解密
var decrypt = new JSEncrypt();
decrypt.setPrivateKey("私钥");
var uncrypted = decrypt.decrypt(encrypted);//解密
```
密文末尾为`==`
加密后密文与Base64特征类似