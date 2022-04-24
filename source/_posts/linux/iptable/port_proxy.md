---
title: iptable 实现正向代理/端口转发/端口中继
date: 2022-03-22 13:59:56
tags: [linux,iptable,firewall]
categories: [linux,iptable]
---

# 命令
```sh
iptables -t nat -A PREROUTING -p tcp -m tcp --dport 10000 -j DNAT --to-destination 1.1.1.1:30000
iptables -t nat -A PREROUTING -p udp -m udp --dport 10000 -j DNAT --to-destination 1.1.1.1:30000
iptables -t nat -A POSTROUTING -d 1.1.1.1 -p tcp -m tcp --dport 30000 -j SNAT --to-source 192.168.0.1
iptables -t nat -A POSTROUTING -d 1.1.1.1 -p udp -m udp --dport 30000 -j SNAT --to-source 192.168.0.1
```
不同端口 端口转发
将本地服务器(中转服务器 192.168.0.1 )的 10000 端口转发至目标IP(被中转服务器)为 1.1.1.1 的 30000 端口