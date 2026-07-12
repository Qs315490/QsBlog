---
title: 帮 Android 13+ 找回应用语言设置
tags: [android,Language Selector, 按应用设定语言偏好, Per-app language preferences]
categories: [android]
description: ''
date: 2026-07-12 10:16:53
updated: 2026-07-12 10:16:53
---
# 前言
`Android 13 更新里加入了 应用语言` 设置，允许用户单独为应用设置显示语言。但是一些 `OEM 深度定制的系统中，这个功能入口却莫名其妙的消失了`。对于一些期待着它的用户，手上的设备终于`吃上了 Android 13 却找不到应用语言` 这项设置的时候，颇有种煮好了饺子却发现家里没醋了的感觉。

# 解决方案
[Language Selector](https://github.com/VegaBobo/Language-Selector) 的作者就遇上了这个问题：`基于 Android 13 的 MIUI 目前没有这个功能的入口`，但是他发现通过 ADB 命令
```console
adb shell cmd locale set-app-locales <PACKAGE_NAME> [--user <USER_ID>] [--locales <LOCALE_INFO>]
```
仍然可以单独设置应用的语言，也就是说功能还在，只是入口被隐藏了。于是他为这项功能编写了一个「前端」，也就是 `Language Selector`。 由于 `Language Selector 只是 adb shell 命令的前端`，因此首先需要系统底层版本为 Android 13 及以上，此外 `Language Selector 需要通过 Shizuku 获得合适的权限`来调用 LocaleManager API 进行应用语言的更改。

# Language Selector介绍
Language Selector 提供了两种方式来管理应用语言，  
第一种方式就是进入 Language Selector，选择要修改语言的应用*点击修改；  
第二种方式是首先在 Language Selector 内通过长按语言列表中语言项将其置项，然后在系统的快速磁贴中添加 Language Selector 的磁贴，打开需要修改语言的应用点击磁贴将应用修改为前面被置项的语言。

虽然 Language Selector 在尽力还原 AOSP 上的应用语言设置体验，但是由于实现方式上的限制，两者还是有一些差别，`AOSP 的应用语言设置中默认只显示适配了该功能的应用`，而 `Language Selector 默认显示所有可用应用`，类似于在 AOSP 上运行
```console
adb shell settings put global settings_app_locale_opt_in_enabled false
```
命令的效果。同样受限于实现方式，Language Selector 中的应用列表滑动经常掉帧，作者在 README 的 Language availability 部分做了说明。

# 警告
需要注意更改不受支持的应用或者系统应用的语言可能会导致应用运行出现问题，请酌情使用