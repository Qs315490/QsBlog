---
title: KVM/PVE问题解决
tags: [KVM, Linux, AMD GPU, PVE, PROXMAX, spice, 鼠标偏移]
categories: [linux, PVE]
description: 'KVM/PVE 常见问题解决，以及一些小技巧'
date: 2026-02-09 16:30:29
updated: 2026-02-09 16:30:29
---

# AMD GPU Reset BUG
内核参数添加 `amdgpu.reset_method=0`

# PVE 隐藏虚拟化标志
此命令创建一个配置文件，该文件设置了一个自定义CPU模型，该模型具有隐藏的虚拟化标志，并且传递主机CPU模型。给需要隐藏虚拟化标志的虚拟机使用此CPU模型即可。
```bash
cat << EOF | tee /etc/pve/virtual-guest/cpu-models.conf
cpu-model: hiddenvm
    flags -hypervisor;+hv-evmcs
    phys-bits host
    hidden 1
    hv-vendor-id proxmox
    reported-model host

EOF
```

# PVE鼠标偏移解决
spice鼠标偏移，在虚拟机中鼠标会飘，在 `/etc/pve/qemu-server/xxx.conf` 中加入：
```yml
tablet: true
```
或shell中执行：
```shell
qm set xxx -tablet true
```
然后重新启动即可。 `xxx` 是虚拟机 id
