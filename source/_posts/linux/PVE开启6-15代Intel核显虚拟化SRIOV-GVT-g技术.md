---
title: PVE开启6-15代Intel核显虚拟化SR-IOV/GVT-g技术
tags: [PVE, SR-IOV, GVT-g]
categories: [linux, PVE]
description: ''
date: 2026-02-09 16:51:51
updated: 2026-02-09 16:51:51
---
# 设备支持情况
英特尔持续推动英特尔®图形处理器上的图形虚拟化技术。这些技术中的最新技术是`单根 IO 虚拟化 （SR-IOV）`，它取代了之前的 `英特尔® Graphics Virtualization Technology –g （GVT-g）`  
要了解每个[英特尔®显卡家族](https://www.intel.cn/content/www/cn/zh/support/articles/000093216/graphics/processor-graphics.html)支持哪种显卡虚拟化技术，请参阅下表：
产品家族|支持的显卡虚拟化技术
-|-
英特尔® Arc™ Pro B 系列独立显卡家族（过去称为 `Battlemage`）|单根 IO 虚拟化 （SR-IOV）
英特尔® Arc™ B 系列独立显卡家族（过去称为 `Battlemage`）|`不支持`
英特尔® Core™ Ultra 处理器（系列 2） 处理器家族（以前称为 `Arrow Lake`）|单根 IO 虚拟化 （SR-IOV）
英特尔® Core™ Ultra 处理器（系列 2） 处理器家族（以前称为 `Lunar Lake`）|`不支持`
英特尔® Core™ Ultra 处理器（系列 1） 处理器家族（以前称为 `Meteor Lake`）|`不支持`
第 14 代英特尔® 酷睿™处理器家族（过去称为 `Raptor Lake Refresh`）|单根 IO 虚拟化 （SR-IOV）
第 13 代英特尔® 酷睿™处理器家族（过去称为 `Raptor Lake`）|单根 IO 虚拟化 （SR-IOV）
英特尔® Data Center GPU Flex Series 独立显卡家族（过去称为 `Arctic Sound`）|单根 IO 虚拟化 （SR-IOV）
英特尔® Arc™ Pro A 系列独立显卡家族（过去称为 `Alchemist`）|`不支持`
英特尔® Arc™ A 系列独立显卡家族（过去称为 `Alchemist`）|`不支持`
第 12 代智能英特尔® 酷睿™处理器家族（过去称为 `Alder Lake`）|单根 IO 虚拟化 （SR-IOV）
第十一代智能英特尔® 酷睿™处理器家族（前身为 `Tiger Lake`）|`不支持`
第十一代智能英特尔® 酷睿™处理器家族（过去称为 `Rocket Lake`）|英特尔® Graphics Virtualization Technology –g （GVT-g）
第十代智能英特尔® 酷睿™处理器家族（过去称为 `Ice Lake`）|英特尔® Graphics Virtualization Technology –g （GVT-g）
第十代智能英特尔® 酷睿™处理器家族（过去称为 `Comet Lake`|英特尔® Graphics Virtualization Technology –g （GVT-g）
第九代智能英特尔® 酷睿™处理器家族（以前称为 `Coffee Lake Refresh`）|英特尔® Graphics Virtualization Technology –g （GVT-g）
第八代智能英特尔® 酷睿™处理器家族（过去称为 `Amber Lake`）|英特尔® Graphics Virtualization Technology –g （GVT-g）
第八代智能英特尔® 酷睿™处理器家族（过去称为 `Coffee Lake`）|英特尔® Graphics Virtualization Technology –g （GVT-g）
第七代智能英特尔® 酷睿™处理器家族（前身为 `Apollo Lake`）|英特尔® Graphics Virtualization Technology –g （GVT-g）
第七代智能英特尔® 酷睿™处理器家族（过去称为 `Kaby Lake`）|英特尔® Graphics Virtualization Technology –g （GVT-g）
第六代智能英特尔® 酷睿™处理器家族（前身为 `Skylake`）|英特尔® Graphics Virtualization Technology –g （GVT-g）

注意，代号为 `Battlemage` | `Lunar Lake` | `Meteor Lake` | `Alchemist` | `Tiger Lake` 这5款图形处理器并不支持`SR-IOV`，也不支持`GVT-g`

# PVE基本设置
安装PVE过程略过，提前将pve安装好，如要正常使用`11-15代`核显`SR-IOV`，需要将`内核更新到6.8及以上`。并在BIOS提前开启`VT-d`（必须）,`SR-IOV`(如有可以开), `Above 4GB`(如有可以开),和关闭安全模式(`Secure Boot`)

本篇文章将大量使用nano文本编辑命令，至于怎么使用自行百度，这里不重复造轮子了。 知道如何保存就行Ctrl +X 输入“Y”回车保存

# 开启GPU虚拟化
{% tabs GPU-Virt %}
<!-- tab SR-IOV -->
## intel 11-15代开启SRIOV核显虚拟化
### 安装headers头文件
```bash
apt install pve-headers-$(uname -r)
```
### 安装构建工具
```bash
apt install build-essential dkms
```
### 下载i915-sriov驱动
国内访问github很慢，建议给PVE挂上代理再下载，或者手动到github下载下来再上传到PVE
[i915-sriov版本发布页](https://github.com/strongtz/i915-sriov-dkms/releases)
适用于`6.8 ~ 6.16`内核的i915-sriov驱动，下载到临时目录/tmp/
```bash
wget -O /tmp/i915-sriov-dkms_2025.07.22_amd64.deb "http://dl.yangwenqing.com/webos/link/8f592d04d4be4194a8fe5a2fff6446f6/i915-sriov-dkms_2025.07.22_amd64.deb"
```
适用于`6.12 ~ 6.19`内核的i915-sriov驱动，下载到临时目录/tmp/
```bash
wget -O /tmp/i915-sriov-dkms_2026.02.04_amd64.deb "http://dl.yangwenqing.com/webos/link/4a03e8873eea4aaeaa1532927134b830/i915-sriov-dkms_2026.02.04_amd64.deb"
```
### 安装SR-IOV驱动
如果你的PVE内核是`Linux6.8 ~ 6.16`的版本则安装`i915-sriov-dkms_2025.07.22_amd64.deb`  
如果你的PVE内核是`Linux6.12 ~ 6.19`的版本则安装`i915-sriov-dkms_2026.02.04_amd64.deb`
```bash
dpkg -i /tmp/i915-sriov-dkms*.deb
```
经过漫长等待完成后，检查安装是否成功，运行
```bash
modinfo i915|grep vf
```
反馈如下表示成功：
```
parm:           max_vfs:Limit number of virtual functions to allocate. (0 = no VFs [default]; N = allow up to N VFs) (uint)
```
#### 如何安装6.8内核？
```bash
# 安装6.8.12-10-pve内核
apt install pve-kernel-6.8.12-10-pve
# 查看当前安装的内核
proxmox-boot-tool kernel list
# 如 6.8.12-10-pve
# 将装好的6.8内核设置成第一启动项，重启PVE即可进到新内核
proxmox-boot-tool kernel pin 6.8.12-10-pve
# 别忘了更新内核后给内核安装头文件再打驱动
apt install pve-headers-$(uname -r)
```
### 设置内核参数
需要提前在主板BIOS开启虚拟化功能，才能开启硬件直通。在BIOS开启`vt-d`, `SR-IOV`(如有可以开), `Above 4GB`(如有可以开), 和关闭安全模式(`Secure Boot`)。
使用nano命令编辑`/etc/default/grub`文件
```
nano /etc/default/grub
```
开启`iommu`分组、`i915 guc`和`SR-IOV`，在里面找到：GRUB_CMDLINE_LINUX_DEFAULT="quiet"项将其修改为
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt i915.enable_guc=3 i915.max_vfs=7 module_blacklist=xe"
```
修改完成更新grub
```bash
update-grub
```
然后重启PVE
### 设置SR-IOV显卡数量
按需设置最高7个（`i915.max_vfs=7`），设置1个时性能最强。
#### 安装sysfsutils
```bash
apt install -y sysfsutils
```
#### 添加VFs核显数量
按需设置最高7个，设置1个时性能最强。
开启3个VFs核显数量:
```bash
echo 3 > /sys/devices/pci0000:00/0000:00:02.0/sriov_numvfs
```
或者持久化到`/etc/sysfs.conf`文件中，重启后依然生效
```bash
echo "devices/pci0000:00/0000:00:02.0/sriov_numvfs = 3" > /etc/sysfs.conf
# 应用配置文件, 重启也行
sysctl -p
```
### 验证是否开启核显SRIOV
使用lspci命令查看核显信息，有如下内容则成功开启核显
注意：物理核显02.0不能直通，否则所有虚拟核显消失
```
00:02.0 VGA compatible controller: Intel Corporation Alder Lake-S GT1 [UHD Graphics 770]（物理核显）
00:02.1 VGA compatible controller: Intel Corporation Alder Lake-S GT1 [UHD Graphics 770] （虚拟核显1）
00:02.2 VGA compatible controller: Intel Corporation Alder Lake-S GT1 [UHD Graphics 770] （虚拟核显2）
00:02.3 VGA compatible controller: Intel Corporation Alder Lake-S GT1 [UHD Graphics 770] （虚拟核显3）
```
### 温馨提示
如果你的处理器是最新的`Arrow Lake`架构，安装了`i915-sriov-dkms`，没法看到7个VF。可以另外加上`i915.force_probe=7d67`参数到grub。
<!-- endtab -->
<!-- tab GVT-g -->
## 开启硬件直通和GVT-g
### 设置内核参数
需要提前在主板BIOS开启虚拟化功能，才能开启硬件直通。在BIOS开启`vt-d`, `Above 4GB`(如有可以开)和关闭安全模式(`Secure Boot`)。
使用nano命令编辑/etc/default/grub
```bash
nano /etc/default/grub 
```
开启iommu分组和GVT-g，在里面找到：GRUB_CMDLINE_LINUX_DEFAULT="quiet"项将其修改为
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt i915.enable_gvt=1 pcie_acs_override=downstream,multifunction"
```
更新grub
```bash
update-grub
```
重启PVE

### 验证是否开启GVT-g
```bash
ls /sys/bus/pci/devices/0000:00:02.0/mdev_supported_types
```
显示有2个或者更多即成功，UHD630核显最多可以开2个
```bash
i915-GVTg_V5_4  i915-GVTg_V5_8
```
<!-- endtab -->
{% endtabs %}

# 验证是否开启 iommu
```bash
dmesg | grep iommu
# 或者
dmesg | grep -e DMAR -e IOMMU -e AMD-Vi
```
出现如下例子。则代表成功
```
[ 1.341100] pci 0000:00:00.0: Adding to iommu group 0
[ 1.341116] pci 0000:00:01.0: Adding to iommu group 1
[ 1.341126] pci 0000:00:02.0: Adding to iommu group 2
[ 1.341137] pci 0000:00:14.0: Adding to iommu group 3
[ 1.341146] pci 0000:00:17.0: Adding to iommu group 4
```
此时输入命令
```bash
find /sys/kernel/iommu_groups/ -type l
```
出现很多直通组，就代表成功了。如果没有任何东西，就是没有开启

# 虚拟机设置
推荐OVMF启动方式，其他部分和普通PCIe直通一样，设置好虚拟机即可。
{% tabs GPU-Virt %}
<!-- tab SR-IOV -->
{% p red::注意: 不要把物理核显02.0直通，否则所有虚拟核显消失 %}
<!-- endtab -->
<!-- tab GVT-g -->
添加PCI设备时勾选`ROM-Bar`和`PCIE`在`Mdev类型`中选择vgpu设备`i915-GVTg_V5_8`或其他配置项
<!-- endtab -->
{% endtabs %}