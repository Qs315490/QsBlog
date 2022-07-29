---
title: openwrt防火墙只允许国内访问
date: 2022-07-29 11:47:09
tags: [openwrt,firewall,iptables]
categories: [linux,openwrt]
---
# 前言
由于我家是电信公网，在后台日志中常常看到来自世界各地的访问。  
这些IP尝试爆破我SSH服务的密码、微软远程桌面的用户名和密码，显然是一个安全隐患。
为了使我的防火墙更加安全，我决定将其设置为只允许国内访问。

{% span red::注意：openwrt 22.03版开始使用nftables，不再使用iptables。 %}

# 安装所需的依赖包
安装`ipset`, 新版本openwrt需要安装`iptables`来兼容此教程。

```sh
opkg update
opkg install ipset
```

# 配置 ipset

```sh
# 创建一个名为cnip的规则
ipset create cnip hash:net
# 下载国家IP段，这里以中国为例
wget -P /tmp/ http://www.ipdeny.com/ipblocks/data/countries/cn.zone
# 将IP段添加到cnip规则中
for i in $(cat /tmp/cn.zone ); do ipset add cnip $i; done
# 保存规则到配置文件中
ipset save > /etc/ipset.conf
# 重启自动恢复ipset规则
echo "ipset -R < /etc/ipset.conf" > /etc/rc.local
```

{% span red::注意：最后有一条命令会覆盖这个文件的内容，或者手动插入`ipset -R < /etc/ipset.conf`到`exit 0`命令前一行 %}

# 配置防火墙

```sh
# 将规则写入防火墙的自定义规则文件中
cat << EOF >> /etc/firewall.user
# 阻断端口22 3389
iptables -I zone_wan_input -p tcp -m multiport  --dport 22,3389 -j DROP
# 允许国内访问端口
iptables -I zone_wan_input -p tcp -m multiport  --dport 22,3389 -m set --match-set cnip src -j ACCEPT
EOF
# 重启防火墙
service firewall restart
```

openwrt的防火墙`Filter`表中`INPUT`链处理所有接入的包, 其中`zone_wan_input`链是`wan`空间的`INPUT`链。  
`zone_wan_input`链中有规则会允许所有的包通过，为保证规则的效应`iptables`使用`插入(-I)`而不是`添加(-A)`。
