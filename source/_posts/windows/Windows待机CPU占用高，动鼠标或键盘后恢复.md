---
title: Windows待机CPU占用高，动鼠标或键盘后恢复
tags: [Windows, WinSAT, 待机占用, 系统评估工具]
categories: [Windows, 小技巧]
description: ''
date: 2026-04-28 12:34:23
updated: 2026-04-28 12:34:23
---
# Windows待机CPU占用高，动鼠标或键盘后恢复
`WinSAT` 是 Windows 自带的系统评估工具，空闲时自动运行可能导致 CPU 占用高，可通过任务计划程序禁用以降低资源占用。

如果禁用 `Windows系统评估工具` 无效，请往病毒方向寻找问题。

# WinSAT 的作用
WinSAT（Windows System Assessment Tool）用于评估系统硬件性能，包括 CPU、内存、硬盘、图形 GPU 和网卡等，并生成性能评分，为系统优化提供参考。它通常在`系统空闲时自动运行`，以更新性能分数，但在低配电脑或频繁触发时可能导致 `CPU 占用 100%`。

# CPU 占用高的原因
自动触发：WinSAT 会在系统空闲或更新驱动时自动运行，执行全面的硬件性能测试。  
后台任务计划：WinSAT 通过任务计划程序在 Microsoft\Windows\Maintenance 下运行，如果频繁触发，会占用大量 CPU 资源。  
低配硬件：在性能较低的电脑上，评估过程本身会增加系统负载，甚至出现“WinSAT 已停止工作”的提示。

# 解决方法
## 方法一：通过任务计划程序禁用 WinSAT
右键点击 此电脑 → 管理。
打开 任务计划程序 → 任务计划程序库 → Microsoft → Windows → Maintenance。
在右侧找到 WinSAT，右键选择 禁用 或 删除。

## 方法二：修改权限或删除相关文件（高级用户）
修改 WaaSMedicSvc.dll 权限或删除后重启系统，可阻止 WinSAT 自动运行，但需谨慎操作，避免影响系统更新。

## 方法三：使用命令行或 PowerShell
可以通过 winsat 命令手动运行评估，避免自动触发。若不需要评估，可不执行命令，从而减少 CPU 占用。 

# 注意事项
禁用 WinSAT 不会影响系统核心功能，但会停止自动生成性能评分。
对于低配电脑，禁用 WinSAT 可以显著降低空闲时的 CPU 占用，提高系统响应速度。  
通过以上方法，可以有效解决 Windows 系统评估工具占用 CPU 高的问题，同时保持系统稳定运行。