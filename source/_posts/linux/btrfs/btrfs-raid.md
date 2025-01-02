---
title: btrfs raid1转read10记录
date: 2023-05-31 09:45:49
updated: 2023-05-31 09:45:49
tags: [linux,btrfs,filesystem,raid]
categories: [linux,btrfs]
---
# 测试环境
模拟我的4盘位硬盘盒，现有两块做 raid1 并且已经有数据。  
在新加入两块盘后组成 raid10 并<font color="red">不损失数据</font>。  
在 VMware Workstation 创建虚拟机配置如下
![配置](/images/btrfs/vmware配置.png)

# 创建Raid1阵列
模拟我现在的磁盘状态。
```bash
# 已有数据需要 -f 参数强制覆盖
mkfs.btrfs -m raid1 -d raid1 /dev/sd{b,c}
```
![命令返回结果](/images/btrfs/创建文件系统.png)
写入数据，用于辨别操作后是否损坏数据。
```bash
mount /dev/sdb /mnt
echo "abcde">/mnt/test.txt
dd if=/dev/random of=/mnt/testfile bs=1M count=256
```
查看当前使用情况
![加入设备前使用情况](/images/btrfs/加入设备前使用情况.png)
# 将新加入磁盘加入阵列
## 将磁盘加入阵列
单纯的将磁盘加入阵列只是以reid1模式加入。
```bash
# btrfs device add -f device[ device ...] path -f参数强制覆盖会损失盘上所有数据
btrfs d a /dev/sd{d,e} /mnt
```
![加入设备后使用情况](/images/btrfs/加入设备后使用情况.png)
## 均衡磁盘
btrfs将设备加入阵列后数据是不会变动的。需要重新分配。  
重新分配时加入转换参数将 reid1 转为 reid10.
经确认数据完整。不过实际设备往往数据量大需要时间长，在转换过程中突发断电数据难保。
```bash
btrfs balance start -dconvert=raid10 -mconvert=raid10 -sconvert=raid10 -f /mnt
```
![开始平衡数据](/images/btrfs/开始平衡数据.png)
![平衡数据后](/images/btrfs/平衡数据后.png)
![平衡数据后1](/images/btrfs/平衡数据后1.png)