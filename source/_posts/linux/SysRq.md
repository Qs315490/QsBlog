---
title: SysRq
tags: [sysrq,linux]
categories: [linux,sqsrq]
description: ''
date: 2025-10-30 17:48:46
updated: 2025-10-30 17:48:46
---

# /proc/sysrq-trigger详解
这是一组“魔术组合键”，只要内核没有被完全锁住，不管内核在做什么事情，使用这些组合键能即时打印出内核的信息。

使用SysRq组合键是了解系统目前运行情况的最佳方式。如果系统出现挂起的情况或在诊断一些和内核相关，比较怪异，比较难重现的问题的时候，使用SysRq键是个比较好的方式。

# 怎么打开和关闭SysRq组合键?

为了安全起见，在红帽企业版Linux里面，默认SysRq组合键是关闭的。 打开这个功能，运行：
```bash
echo 1 > /proc/sys/kernel/sysrq
```
关闭这个功能：
```bash
echo 0 > /proc/sys/kernel/sysrq
```
如果想让此功能一直生效，在/etc/sysctl.conf里面设置kernel.sysrq的值为1。重新启动以后，此功能将会自动打开。
```bash
echo 'kernel.sysrq = 1' > /etc/sysctl.conf
```
因为打开sysrq键的功能以后，有终端访问权限的用户将会拥有一些特别的功能。因此，除非是要调试，解决问题，一般情况下，不要打开此功能。如果一定要打开，请确保你的终端访问的安全性。

# 怎么触发一个sysrq事件?
有几种方式能触发sysrq事件。在带有AT键盘的一般系统上，在终端上输入一下组合键:
{%kbd Alt%}+{%kbd PrintScreen%}+[CommandKey]

例如，要让内核导出内存信息(CommandKey “m”)，你应该同时按下{%kbd Alt%} 和 {%kbd Print Screen%} 键，然后按下 {%kbd M%} 键。

提示：此组合键在Xwindows上是无法使用的。所以，你先要转换到文本虚拟终端下。如果你当前是在图像界面，

能按{%kbd Ctrl%}+{%kbd Alt%}+{%kbd F1%}转换到虚拟终端。

在串口终端上,要想获得同样的效果，需要先在终端上发送Break信号，然后在5秒内输入sysrq组合键。

如果你在机器上有root权限，你能把commandkey字符写入到/proc/sysrq-trigger文件。这能帮助你通过脚本或你不在系统终端上的时候触发sysrq事件。
```bash
echo m > /proc/sysrq-trigger
```
当我触发一个sysrq事件的时候，结果保存在什么地方?

当一个sysrq命令被触发，内核将会打印信息到内核的环形缓冲并输出到系统控制台。此信息一般也会通过syslog输出到/var/log/messages。

有时候，可能系统已无法响应，syslogd可能无法记录此信息。在这种情况下，建议你设置一个串口终端来收集这个信息。

那些类型的sysrq事件能被触发?

sysrq功能被打开后，有几种sysrq事件能被触发。不同的内核版本可能会有些不同。但有一些是共用的：
CommandKey|功能
---|---
b|立即重新启动计算机
o|立即关闭计算机(如果机器设置并支持此项功能)
m|导出关于内存分配的信息
t|导出线程状态信息
p|导出当前CPU寄存器信息和标志位的信息
c|故意让系统崩溃(在使用netdump或diskdump的时候有用)
s|即时同步所有挂载的文件系统
u|立即重新挂载所有的文件系统为只读
