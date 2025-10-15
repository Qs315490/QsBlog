---
title: Windows11 升级24H2之后无法关闭Hyper-V怎么办？
tags: [Windows, 虚拟化, 虚拟机平台, HyperV]
categories: [Windows, 虚拟化]
description: ''
date: 2025-10-15 21:48:57
updated: 2025-10-15 21:48:57
---
# 前言
在升级Windows 11至24H2版本后，因Microsoft为所有全新安装和升级的 Windows 11 默认开启了 Device/Credential Guard功能，且该功能会调用Hyper-V功能（即基于虚拟化的安全性功能），导致部分用户会遇到无法关闭Hyper-V的情况，导致使用虚拟机过程中感觉运行卡顿、虚拟机无法启动等情况，可参考以下步骤操作关闭Hyper-V功能。

# 操作步骤
## 1. 下载工具
从 Microsoft 下载中心下载[Device Guard and Credential Guard hardware readiness tool](https://www.microsoft.com/en-us/download/details.aspx?id=53337)
![下载DGCG](/images/windows_hyperv/下载DGCG.webp)

## 2. 解压工具
解压下载的文件，并进入解压后的文件夹。点击文件夹上方的地址栏，复制地址内容。
![文件路径](/images/windows_hyperv/文件路径.webp)

## 3. 终端目录切换
鼠标右键点击Windows菜单  
Windows 11选择终端管理员，Windows 10选择 Windows PowerShell(管理员)  
并确保终端运行的Shell为PowerShell
![Windows终端](/images/windows_hyperv/Windows终端.webp)

在终端输入`cd <目录路径>`即可切换目录，例如：
```powershell
cd C:\Temp\dgreadiness_tool_v3.6
```
在终端或 PowerShell中输入cd，按下空格后 粘贴（右键或{%kbd Ctrl%}+{%kbd V%}）剪切板的路径，并按下回车，切换到对应文件夹目录路径。

## 4. 运行脚本
先设置执行策略为RemoteSigned，否则会报错。  
复制并粘贴以下命令到终端，然后按下{%kbd Enter%}键执行。
```powershell
set-ExecutionPolicy RemoteSigned
```
进入目录后在终端输入`.\DG`，然后按下{%kbd Tab%}键，会自动补全脚本名称，然后按下{%kbd Enter%}键运行脚本。
```powershell
.\DG_Readiness_Tool_v3.6.ps1 -Disable
```
![运行脚本](/images/windows_hyperv/运行脚本.webp)

## 5. 重启电脑
完成执行后关闭终端或 PowerShell，并保存所有打开的文件，重启电脑。  
重启后Windows会进入Credential Guard Opt-out Tool，此时按下 {%kbd Win%} 或 {%kbd F3%} 后再按下回车即可继续。
![Credential Guard Opt-out Tool](/images/windows_hyperv/Credential Guard Opt-out Tool.webp)

## 6. 查看 `基于虚拟化安全性` 状态
完成上述操作后，电脑会再次重启，进入Windows后即可看到 Device/Credential Guard 已关闭。  
可通过Windows搜索栏输入“系统信息”进行查看。
![Windows搜索栏-系统信息](/images/windows_hyperv/Windows搜索栏-系统信息.webp)
![系统信息](/images/windows_hyperv/系统信息.webp)
