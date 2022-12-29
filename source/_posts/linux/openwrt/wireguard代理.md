---
title: wireguard代理
date: 2022-12-29 21:16:55
tags: [linux,openwrt,wireguard,proxy,nat,route]
categories: [linux,openwrt]
---
# 前言
本教程只是实验目的，禁止用于非法用途。一切后果与本人无关。

# 测试环境
![WireGuard代理](/images/WireGuard代理.png)

# 操作步骤
在原有WireGuard隧道的基础上进行小修改即可。 
## 路由器
### 防火墙
在WireGuard的防火墙空间上 `允许转发到wan所在的空间` 既可。  
入站、出站、转发建议全部接受。开启`IP动态伪装(这个是IPv4的NAT，v6NAT需要改防火墙文件且只有fw4版本才开始支持)`。  
添加防火墙规则(负责转发IPv6，一般来说V4 wan口默认全局NAT)。其他Linux命令差不多，要操作的表不一样。
{% tabs firewall %}
<!-- tab iptables(22.03之前) -->
```bash
OpenWrt21.02.5测试没有问题
# 命令     nat表     wan出站对应的链       匹配： 限制源地址 跳到MASQUERADE(自动重写地址为出站端口)
ip6tables -t nat -A zone_wan_postrouting -s 10.3.3.0/24 -j MASQUERADE
```
<!-- endtab -->
<!-- tab nftables(22.03之后) -->
```bash
# OpenWrt22.03.2测试没有问题
# 添加规则 在inet集fw4表的srcnat_wan链中，匹配是IPv6且iif(源网卡)是wg网卡的数据做masquerade(自动重写地址为出站端口)
nft add rule inet fw4 srcnat_wan meta nfproto ipv6 iif "wg" masquerade
```
<!-- endtab -->
{% endtabs %}
### 路由表
如果到这里设置外围设备允许的IP地址为 `0.0.0.0/0,::/0` 或 `::/1, 8000::/1, 0.0.0.0/1, 128.0.0.0/1` IPv6能上网就不需要继续看了，教程结束。  
如果IPv6不能联网大概率是路由表的锅。  
IPv4的路由表是自动配置的。IPv6的路由表自动配置的可能会有问题。
使用以下方法解决
```bash
cat <<EOF > /etc/hotput.d/iface/99-wireguard
#!/bin/sh
[ $ACTION = ifup ]||exit0
# 要检测上线的接口(一般是wan)
iface='pppor-wan'
[ $INTERFACE = "$iface" ]||exit 0
logger -t WireGuard "接口$iface重新上线，正在重新分配路由"
route=$(ip -6 route show dev $iface|head -n1|awk '{print $1,$4,$5,$6,$7}')
status=$(ip -6 add route $route)
logger -t WireGuard "IPv6路由状态：$route $status"
EOF
chmod +x /etc/hotplug.d/iface/99-wireguard
route=$(ip -6 route show dev $iface|head -n1|awk '{print $1,$4,$5,$6,$7}')
ip -6 add route $route
```
现在IPv6应该可以正常上网了。