---
title: Android 手动触发 dex2oat
tags: [Android, 优化, dex2oat]
categories: [Android, 优化]
description: ''
date: 2025-10-15 22:27:29
updated: 2025-10-15 22:27:29
---
# 介绍 dex2oat
运用 dex2oat 提前把dex编译成oat文件，提高应用运行效率。

## AOT & JIT
Android 从 L(5.0) 开始，引入了 ART 虚拟机，拥有了预先 (AOT) 编译的新特性，即在安装时ART就通过 dex2oat 工具来编译应用，提高应用运行时的效率。但是，安装过程执行dex2oat会增加安装应用的时间。

于是Android N(7.0)引入了新的机制，运行时 (ART) 包含一个具备代码分析功能的即时 (JIT) 编译器，有助于提高运行时性能，节省存储空间，以及加快应用及系统更新速度。JIT在软件安装时不进行编译而是根据用户的使用习惯来记录值得编译的内容 (包括软件运行时编译并保存，在闲时编译记录的需要编译而没来得及干的事情，而这些事情就是记录为speed-profile)。

## 编译模式
从Android O(8.0)开始，官方支持4个编译模式。
- verify：只运行 DEX 代码验证。
- quicken：`Android 11 或更低版本` 运行 DEX 代码验证，并优化一些 DEX 指令，以获得更好的解译器性能。
- speed：运行 DEX 代码验证，并对所有方法进行 AOT 编译。不优化任何类的类加载。
- speed-profile：运行 DEX 代码验证，对配置文件中列出的方法进行 AOT 编译，并为配置文件中的类优化类加载。

`Android 14 及更高版本` 默认使用 speed-profile 编译过滤器进行编译；如果未提供配置文件，则使用 speed 编译过滤器进行编译。
`Android 13 及更低版本` 默认使用 speed 编译过滤器进行编译。

以下是 Android 文档中并未明确提及的编译模式：
- space-profile: 依据配置文件优化空间使用
- space: 优化空间使用
- everything: 编译所有方法，也是占用手机容量空间最大的模式

## 操作限制
而随着Android系统版本的更迭，发现原本可以在应用进程上触发dex2oat编译的方式，却在`targetSdkVersion>=29且Android 10+`的系统上，不再允许使用。其原因是系统在`targetSdkVersion=29`的时候，对此做了限制，不允许应用进程上触发dex2oat编译（Android 运行时 (ART) 不再从应用进程调用 dex2oat。这项变更意味着 ART 将仅接受系统生成的 OAT 文件）（OAT为dex2oat后的产物）。

那当前是否会受到这个限制的影响呢？

在2020年的时候Android 11系统正式发布，各大应用市场就开始限制App的`targetSdkVersion>=29`，也就意味着，现如今App的`targetSdkVersion>=29`是不可避免的。而且随着新Android设备的不断迭代，越来越多的用户，使用上了携带新系统的新机器，使得Android 10+系统的占有量逐步增加，目前为止Android 10+系统的占有量约占整体的30%~40%左右，也就是说这部分机器将会受到这个限制的影响。

`cmd package compile`是应用进程触发系统行为，然后由系统触发dex2oat的方式。不受`targetSdkVersion>=29`的限制，但是需要root权限，且需要系统版本为Android 10+。

# 手动触发 dex2oat
强制编译可通过如下命令
```bash
cmd package compile -m <compile-mode> -f <package-name>
```
其中，`<compile-mode>` 可以是 `verify`、`quicken`、`speed`、`speed-profile` 之一，`<package-name>` 是要编译的包名。

编译所有应用
```bash
cmd package compile -m <compile-mode> -f -a
```

## 示例
speed-profile 某个应用
```bash
cmd package compile -m speed-profile -f com.example.app
```
speed-profile 所有应用
```bash
cmd package compile -m speed-profile -f -a
```
日常在手机上手动执行dex2oat只需要
```bash
cmd package compile -m speed -a
```

# 清除配置文件数据并移除编译后的代码
对特定软件包
```bash
cmd package compile --reset <package-name>
```
对所有软件包
```bash
cmd package compile --reset -a
```

# 参考
- [Android官方文档](https://source.android.com/docs/core/runtime/configure?hl=zh-cn)