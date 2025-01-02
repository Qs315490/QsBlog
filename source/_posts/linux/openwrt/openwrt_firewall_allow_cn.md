---
title: openwrt防火墙只允许国内访问
date: 2022-07-29 11:47:09
updated: 2022-07-29 11:47:09
tags: [openwrt,firewall,iptables,nftables]
categories: [linux,openwrt]
---
# 前言
由于我家是电信公网，在后台日志中常常看到来自世界各地的访问。  
这些IP尝试爆破我SSH服务的密码、微软远程桌面的用户名和密码，显然是一个安全隐患。
为了使我的防火墙更加安全，我决定将其设置为只允许国内访问。

{% note info::openwrt 22.03 版开始使用 nftables，不再使用 iptables。<br>
openwrt 22.03.3 版本 Luci 界面支持配置 IP集 %}

# 安装所需的依赖包
{% note info::安装 `ipset`, openwrt 22.03 或更新版本需要安装 `iptables-nft` 来兼容iptables命令。<br>
PS: `nftables` 自带列表功能，不需要 `ipsec`。%}

```sh
opkg update
opkg install ipset
```

# 配置 IP 列表
{% tabs ipset %}
<!-- tab ipset -->
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
{% note  warning::注意：最后有一条命令会覆盖这个文件的内容，或者手动插入 `ipset -R < /etc/ipset.conf` 到 `exit 0` 命令前一行 %}
<!-- endtab -->
<!-- tab nftables -->
```sh
# 创建一个名为cnip的规则
nft add set inet fw4 CNIP{type ipv4_addr\; flags interval\;}
# 下载国家IP段，这里以中国为例
wget -P /tmp/ http://www.ipdeny.com/ipblocks/data/countries/cn.zone
# 将IP段添加到cnip规则中  注：有效率问题
for i in $(cat /tmp/cn.zone ); do nft add element inet fw4 CNIP {$i}; done
# 保存规则到配置文件中
nft list set inet fw4 CNIP > /etc/CNIP.nft
# 重启自动恢复规则, firewall.user 文件可能不存在，反正能在防火墙运行时添加规则就行。
echo "nft -f /etc/CNIP.nft" >> /etc/firewall.user
```
新安装的系统需要手动配置 `firewall.user`。参考以下链接或代码块。
{% link::firewall配置文件说明::https://openwrt.org/docs/guide-user/firewall/firewall_configuration#includes_for_2203_and_later_with_fw4%}
```conf
config include
    option path '/etc/firewall.user'
    option fw4_compatible 1
```
<!-- endtab -->
<!-- tab Luci -->
自行准备一个`cn.zone`文件，内容为国家IP段。
1. 打开`网络`->`防火墙`->`IP集合`
2. 点击`添加`
3. `数据包字段匹配`选择`IP地址`
4. 通过`包括文件`上传`cn.zone`文件，点击`保存`
5. 点击`保存&应用`
<!-- endtab -->
{% endtabs %}
# 配置防火墙
{% tabs firewall %}
<!-- tab iptables -->
```sh
# 将规则写入防火墙的自定义规则文件中
cat << EOF >> /etc/firewall.user
# 阻断端口22 3389
iptables -I zone_wan_input -p tcp -m multiport  --dport 22,3389 -j DROP
# 允许国内访问端口
iptables -I zone_wan_input -p tcp -m multiport  --dport 22,3389 -m set --match-set cnip src -j ACCEPT
EOF
# 重启防火墙, 报错就用service firewall restart
fw3 restart
```
{% note info::openwrt的防火墙`Filter`表中`INPUT`链处理所有接入的包, 其中`zone_wan_input`链是`wan`空间的`INPUT`链。<br>
`zone_wan_input`链中可能有规则会允许所有的包通过，为保证规则的效应`iptables`使用`插入(-I)`而不是`添加(-A)`。%}
<!-- endtab -->
<!-- tab nftables -->
```sh
# firewall.user文件可能不存在，反正能在防火墙运行时添加规则就行
cat <<EOF >> /etc/firewall.user
# 创建一个由端口组成的列表
nft add set inet fw4 PORT{type inet_service\; flags interval\;}
# 向列表添加端口
nft add element inet fw4 PORT{ssh,3389}
# 阻止列表外的IP访问端口
nft insert rule inet fw4 input_wan ip saddr != @CNIP tcp dport @PORT drop
EOF
# 重启防火墙, 报错就用service firewall restart
fw4 restart
```
<!-- endtab -->
<!-- tab Luci -->
1. 打开`网络`->`防火墙`->`通信规则`
2. 点击`添加`，源区域`wan`，目的区域`输入`，把要限制的端口添加到`目的端口`中，操作`丢弃`，点击`保存`
3. 点击`添加`，源区域`wan`，目的区域`输入`，限制的端口添加到`目的端口`中，操作`接受`，高级设置里`使用IP集`选择之前创建的IP集，点击`保存`
<!-- endtab -->
{% endtabs %}
