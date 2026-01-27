---
title: Windows 小技巧
date: 2023-12-30 23:12:14
updated: 2023-12-30 23:12:14
tags: [Windows, 小技巧, 'Windows Server', 密码安全设置, DNS, IP, 主机名, 端口, 桌面壁纸, 文件元数据, Chrome, Edge]
categories: [Windows, 小技巧]
---

# 知道IP查询内网设备的主机名
```cmd
ping -a <IP地址>
```

# 查询指定端口占用情况
```cmd
netstat -ano | findstr <端口号>
tasklist | findstr <返回的PID>
```

# 当前用户桌面壁纸位置
```
%UserProfile%\AppData\Roaming\Microsoft\Windows\Themes\CachedFiles\
```

# 修改文件元数据
```powershell
# 创建时间
(ls file.txt).CreationTime = "2038-12-31 23:59:59"
# 修改时间
(ls file.txt).LastWriteTime = "2038-12-31 23:59:59"
# 访问时间
(ls file.txt).LastAccessTime = "2038-12-31 23:59:59"
```

# 关闭 Chrome 系统升级提示
所有用户应用. 如果只需要当前用户生效，把 HKEY_LOCAL_MACHINE 替换为 HKEY_CURRENT_USER
```regedit
Windows Registry Editor Version 5.00 

[HKEY_LOCAL_MACHINE\Software\Policies\Google\Chrome] 
"SuppressUnsupportedOSWarning"=dword:1

[HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Edge] 
"SuppressUnsupportedOSWarning"=dword:1
```

# 关闭 Microsoft Edge 使用推荐设置提示
所有用户应用. 如果只需要当前用户生效，把 HKEY_LOCAL_MACHINE 替换为 HKEY_CURRENT_USER
```regedit
Windows Registry Editor Version 5.00 

[HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Edge] 
"ShowRecommendationsEnabled"=dword:0
```

# MacOS 安装 Windows 卡准备就绪
```regedit
Windows Registry Editor Version 5.00

[HKLOCAL MACHINE\SYSTEM\SETUP\SATUS\ChildCompletion]
"setup.exe" = dword:3
```

# 关闭 DNS 多解析
组策略
中文系统路径:
```
计算机配置 -> 管理模板 -> 网络 -> DNS 客户端 -> 禁用智能多宿主名称解析
```
英文系统路径:
```
Computer Configuration -> Administrative Templates -> Network -> DNS Client -> Turn Off Multicast Name Resolution
```

# windows Server取消密码复杂度
## 1. 打开本地安全策略
服务管理工具 > 工具 > 本地安全策略

## 2. 密码策略
账户策略 > 密码策略 > 密码必须符合复杂性要求 > 禁用

其中，密码最长使用期限、密码最短使用期限、密码最短使用期限（分钟）可以设置为0，表示不限制

# 解决Win11 24H2下网页卡屏/卡顿/显示残留的问题
如题，最近电脑老有问题，神烦  
比如浏览网页，好端端的，随机的就会出现网页卡屏  
我明明切换到另一个标签了，显示的还是上一个标签的图像  
起初我以为是(显卡(因为vscode也有问题)，浏览器)的问题，后来发现其他网友在使用其他软件时也会遇到  
那就是微软的锅了
## 解决方案
### 1. （推荐）禁用 [MPO](https://learn.microsoft.com/zh-cn/windows-hardware/drivers/display/multiplane-overlay-support)
打开`注册表编辑器`，路径`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Dwm`
新建一个`DWORD(32位)值`，命名为`OverlayTestMode`，值设为`5`
```
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Dwm" /v OverlayTestMode /t REG_DWORD /d 5  
```
### 2. 关闭硬件加速
关了就没办法使用GPU硬件加速，导致看视频卡顿，CPU占用高，但是网页浏览正常
### 3. 退回23H2
### 4. 关闭浏览器设置
Edge浏览器的`GPU rasterization`和`Accelerated 2D canvas`都禁用了就好了，可以试试。`edge://flags/`进入设置。

# Windows 睡眠后，莫名唤醒
使用以下命令查看最后一次唤醒的原因
```cmd
powercfg /LASTWAKE
```

## 电源按钮唤醒
如果是有线连接，可能是局域网内其他设备的广播唤醒电脑。  
在设备管理器里找到对应网卡，右键属性，取消勾选 `允许此设备唤醒计算机`。  
如果需要 `网络唤醒(WoL)`功能，可以勾选 `允许此设备唤醒计算机`，然后勾选 `只允许幻术封包唤醒计算机`。

如果是无线网卡也可以试试。虽然无线网卡理论上没有这个功能，但是无线网卡也有唤醒能力。  

# Microsoft Edge 浏览器历史记录、收藏夹、网页登录状态丢失bug
该bug是由`edge游戏助手`引起，只需删掉游戏助手即可解决。

删除`edge游戏助手`的步骤：{%kbd Win%}+{%kbd G%}打开`xbox game bar`，点击最上面的`小组件菜单`→`小组件商店`→`已安装`，删除`edge游戏助手即可`。

由于`edge游戏助手`在系统更新时`会自动重新安装`，所以在更新系统后，需要再次`重复删除步骤`。

如果不幸触发了该bug，不用担心，你的所有记录都没有真正的丢失。在`删除edge游戏助手`，确保所有edge的进程关闭后，彻底地重启edge，`即可恢复所有的记录、收藏夹和网站的登录状态`。

# win7蓝屏A5解决方法 10代以上主板蓝屏A5
复制 [win7蓝屏A5问题解决修改文件acpi.zip](/win7蓝屏A5问题解决修改文件acpi.zip) 到`C:\Windows\System32\drivers\`目录下，替换掉 `acpi.sys` 文件
```
C:\Windows\System32\drivers\acpi.sys
```
