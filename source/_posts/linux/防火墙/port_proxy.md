---
title: 实现正向代理/端口转发/端口中继
date: 2022-03-22 13:59:56
updated: 2022-03-22 13:59:56
tags: [linux, iptables, nftables, firewall, port proxy, 防火墙, 端口转发, NAT]
categories: [linux, 防火墙]
---

# 端口转发
将本地服务器(中转服务器 192.168.0.1 )的 10000 端口转发至目标IP(被中转服务器)为 192.168.0.2 的 30000 端口
这样，访问本地服务器的 10000 端口，就相当于访问目标IP的 30000 端口
## 外部数据包进入时，进行目标地址转换
{% tabs firewall %}
<!-- tab iptables -->
```sh
iptables -t nat -A PREROUTING -p tcp -m tcp --dport 10000 -j DNAT --to-destination 192.168.0.2:30000
iptables -t nat -A PREROUTING -p udp -m udp --dport 10000 -j DNAT --to-destination 192.168.0.2:30000
```
<!-- endtab -->
<!-- tab nftables -->
```sh
nft add table nat
nft -- add chain nat prerouting { type nat hook prerouting priority -100 \; }
nft add rule nat prerouting tcp dport 10000 dnat to 192.168.0.2:30000
nft add rule nat prerouting udp dport 10000 dnat to 192.168.0.2:30000
```
<!-- endtab -->
{% endtabs %}
## 内部数据包出去时，进行源地址转换
{% tabs firewall %}
<!-- tab iptables -->
```sh
iptables -t nat -A POSTROUTING -s 192.168.0.2 -p tcp -m tcp --sport 30000 -j SNAT --to-source 192.168.0.1
iptables -t nat -A POSTROUTING -s 192.168.0.2 -p udp -m udp --sport 30000 -j SNAT --to-source 192.168.0.1
```
<!-- endtab -->
<!-- tab nftables -->
```sh
nft add table nat
nft add chain nat postrouting { type nat hook postrouting priority 100 \; }
nft add rule nat postrouting tcp saddr 192.168.0.2 sport 30000 snat to 192.168.0.1
nft add rule nat postrouting udp saddr 192.168.0.2 sport 30000 snat to 192.168.0.1
```
<!-- endtab -->
{% endtabs %}
或者使用`MASQUERADE`(自动重写源地址为出站接口 IP)
{% tabs firewall %}
<!-- tab iptables -->
```sh
iptables -t nat -A POSTROUTING -s 192.168.0.2 -p tcp -m tcp --sport 30000 -j MASQUERADE
iptables -t nat -A POSTROUTING -s 192.168.0.2 -p udp -m udp --sport 30000 -j MASQUERADE
```
<!-- endtab -->
<!-- tab nftables -->
```sh
nft add table nat
nft add chain nat postrouting { type nat hook postrouting priority 100 \; }
nft add rule nat postrouting tcp saddr 192.168.0.2 sport 30000 masquerade
nft add rule nat postrouting udp saddr 192.168.0.2 sport 30000 masquerade
```
<!-- endtab -->
{% endtabs %}
