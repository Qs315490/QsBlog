---
title: RSA
date: 2022-04-24 15:14:19
updated: 2024-8-25 16:02
tags: [加密算法,RSA]
categories: [加密算法]
---

RSA非对称加密
公钥加密, 私钥解密

# 特征
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
加密后密文与Base64特征类似，密文末尾为因为补位常为`==`。通常密钥非常的长。