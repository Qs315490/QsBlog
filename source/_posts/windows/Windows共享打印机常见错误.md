---
title: Windows共享打印机常见错误
date: 2023-12-30 23:12:14
tags: [Windows, 打印机, Windows共享打印机]
categories: [Windows, 打印机]
---

# 0xa
未知，重试几次后问题解决。

# 0x180b
共享服务器版本过低  
控制面板开启 `SMB1.0/CIFS` 重启解决

# 0x11b 与 0x40
新版本Windows出现此问题，原因KB5005565这个更新导致  
使用注册表或卸载更新解决  
{% tabs e-0x11b %}
<!-- tab 注册表文件 -->
{% link 点击下载::/files/0x11b.reg %}
```
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print]
"RpcAuthnLevelPrivacyEnabled"=dword:0
```
<!-- endtab -->
<!-- tab cmd -->
```cmd
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print" /v "RpcAuthnLevelPrivacyEnabled" /t dword /d 0 /f
```
<!-- endtab -->
<!-- tab PowerShell -->
```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Print" -Name "RpcAuthnLevelPrivacyEnabled" -Value 0
```
<!-- endtab -->
{% endtabs %}
