---
title: PVE Intel网卡SR-IOV
tags: [PVE, KVM, SR-IOV]
categories: [linux, PVE]
description: ''
date: 2026-07-05 11:38:23
updated: 2026-07-05 11:38:23
---
# 确定网卡名称
```console
root@pve:~# dmesg | grep eth

[    4.602387] ixgbe 0000:5f:00.0 ens5f0: renamed from eth0
[    4.686386] ixgbe 0000:5f:00.1 ens5f1: renamed from eth1
```
其中 `ens5f0` 是物理网卡，`ixgbe` 是驱动名称
# 检查SR-IOV支持
网卡驱动就看有没有 `vf` 设置
```console
root@pve:~# modinfo ixgbe
filename:       /lib/modules/7.0.2-proxmox/updates/dkms/ixgbe.ko
license:        GPL
description:    Intel(R) 10 Gigabit Network Card Driver
author:         Intel Corporation, <e1000-devel@lists.sourceforge.net>
...
options:
    max_vfs:       Maximum number of virtual functions (0-128) (32)
...
```
一般名称为 `max_vfs`，此选项默认值表示最多支持32个虚拟网卡
## 查看网卡支持的虚拟网卡数量
```console
root@pve:~# cat /sys/class/net/ens5f0/device/sriov_totalvfs
128
```
## 查看网卡当前可用的虚拟网卡数量
```console
root@pve:~# cat /sys/class/net/ens5f0/device/sriov_numvfs
0
```
# 设置虚拟网卡
设置最大虚拟网卡数量为4，这样能节省一些内存资源
```bash
cat <<EOF > /etc/modprobe.d/sriov_nic.conf
options ixgbe max_vfs=4
EOF
upgrade-initramfs -u
reboot
```
重启后，网卡可用的虚拟网卡数量为4，使用以下命令设置虚拟网卡数量为4
```bash
echo 4 > /sys/class/net/ens5f0/device/sriov_numvfs
```
如果未报错，则设置成功
## 查看虚拟网卡
```console
root@pve:~# root@pve:~# ip link show ens5f0
5: ens5f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq state UP mode DEFAULT group default qlen 1000
 link/ether XX:XX:XX:XX:XX:XX brd ff:ff:ff:ff:ff:ff
 vf 0     link/ether 00:00:00:00:00:00 brd ff:ff:ff:ff:ff:ff, spoof checking on, link-state auto, trust off, query_rss off
 vf 1     link/ether 00:00:00:00:00:00 brd ff:ff:ff:ff:ff:ff, spoof checking on, link-state auto, trust off, query_rss off
 altname enp95s0f1
```
## 查看虚拟网卡MAC地址
```console
root@pve:~# cat /sys/class/net/ens5f0v0/address
52:54:00:00:00:02
```
## 设置虚拟网卡MAC地址
```bash
ip link set dev ens5f0 vf 0 mac 02:00:00:00:00:00
```
# 持久化
`/etc/default/nic_sriov`
```bash
#!/bin/bash

nic_sriov_set(){
    # $1:nic name
    # $2:number of vnic
    if [ -z "$1" ];then
        echo "Please input the nic name"
        return 1
    fi
    if [ -z "$2" ];then
        echo "Please input the number of vnic"
        return 1
    fi
    if [ ! -d "/sys/class/net/$1/device" ];then
        echo "$1 is not a nic"
        return 1
    fi
    echo $2 > /sys/class/net/$1/device/sriov_numvfs
    if [ $? -ne 0 ];then
        echo "Set $1 sriov failed"
        return 1
    fi
    for i in $(seq 0 $(( $2 - 1 )));do
        ip link set dev $1 vf $i mac 02:00:00:00:00:$(( $i + 1 ))
    done
}
# nic_sriov_set 网卡名称 虚拟网卡数量
nic_sriov_set nic2 4
```
添加到开机启动
`/etc/systemd/system/sriov_nic.service`
```systemd
[Unit]
Description=Set NIC SR-IOV
After=network.target

[Service]
ExecStart=/bin/bash /etc/default/nic_sriov
Type=oneshot

[Install]
WantedBy=multi-user.target
```
启用并启动服务
```bash
systemctl enable sriov_nic.service
systemctl start sriov_nic.service
```
