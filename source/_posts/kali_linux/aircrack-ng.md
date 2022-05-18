---
date: "2022/1/20"
title: "aircrack-ng"
tag: [linux,kali_tools]
categories: [linux,kali]
---
# 实列
## 设置无线网卡为监听模式

```sh
airmon-ng start wlan0 # 启动监听模式
airmon-ng stop wlan0mon # 停止监听模式
```

`wlan0`为对应无线网卡，成功启动后网卡名称会在原名称后加`mon`

## 监听网络

### 获取周围网络信息

```sh
airodump-ng wlan0mon
```

**BSSID(Basic Service SetIdentifier)**: AP 的 MAC 地址。  
**PWR(Power)**: 信号强度。  
**Beacons**: AP 发出的通告编号,每个接入点(AP)在最低速率(1M)时差不多每秒会发送 10 个左右的 beacon,所以它们能在很远的地方就被发现。  
**#Data**:当前数据传输量。  
**#/s**:过去 10 秒钟内每秒捕获数据分组的数量。  
**CH(Channel)**: AP 所在的频道。  
**MB**: AP 的最大传输速度。MB=11 => 802.11b,MB=22 => 802.11b+, MB>22 => 802.11g。后面带.的表
示短封包标头,处理速度快,更利于破解。  
**ENC(Encryption)**: 使用的加密算法体系。  
**CIPHER**: 检测到的加密算法。#这个和 ENC 的区别我确实不明白,有没有知道的朋友可以告诉我。  
**AUTH(Authority)**: 认证方式。  
**ESSID(The Extended Service Set Identifier)**: AP 的名称。

### 开始定点监听

```sh
airodump-ng --bssid BSSID地址 -c 信道 -w 文件保存位置 wlan0mon
```

## 断开一个客户端连接

```sh
aireplay-ng -0 次数 -a BSSID地址 -c 客户端地址 wlan0mon
```

使用如上命令辅助握手包抓取
