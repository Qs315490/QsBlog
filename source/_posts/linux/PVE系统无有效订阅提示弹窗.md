---
title: PVE系统无有效订阅提示弹窗
tags: [KVM, PVE]
categories: [linux, PVE]
description: 'Proxmox VE (PVE) 是一款优秀的开源虚拟化管理平台，基于 Debian 并集成了 KVM 和 LXC 虚拟化技术。然而，在使用社区版时，每次登录 Web 管理界面都会弹出订阅提示窗口，虽然不影响功能使用，但频繁出现确实会影响使用体验。'
date: 2026-02-09 16:36:31
updated: 2026-02-09 16:36:31
---
# 8.x
```bash
cp /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js.bak

sed -i "s/if (res === null || res === undefined || \!res || res/if(/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
sed -i "s/.data.status.toLowerCase() !== 'active'/false/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js

systemctl restart pveproxy
```

# 9.x
PVE 9.x的版本，除了每次登录WEB后台会显示无有效订阅提示弹窗之外，在点击「软件包版本」按钮时，首先也会弹出「无有效订阅」的提示弹窗，然后再跳转到软件包版本页面。  
以前版本没有这种逻辑。推测应该是这个原因导致旧方法不能完美适配PVE 9.x版本。

新方法的核心原理是替换关系表达式符号，其本质仅仅是替换一个字符而已，即将!==替换为===，不同于旧方法原理的修改替换关系表达式条件。  
新方法完美适配PVE 9.x版本，不会导致PVE的WEB后台「软件包版本」按钮无法响应。  
新方法的命令操作步骤：
```bash
cp /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js.bak

line_num=$(grep -n "res.data.status.toLowerCase() !== 'active'" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js | head -1 | cut -d: -f1)
sed -i "${line_num}s/!==/===/" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js

systemctl restart pveproxy
```
这个新方法同样适用于屏蔽PBS备份系统的无有效订阅提示弹窗，即Proxmox Backup Server，修改文件操作完成之后重启PBS代理服务即可。
```bash
systemctl restart proxmox-backup-proxy
```