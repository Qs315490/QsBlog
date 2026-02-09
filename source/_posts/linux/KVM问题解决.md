---
title: KVM问题解决
tags: [KVM, Linux, AMD GPU]
categories: [linux, KVM]
description: 'KVM 常见问题解决，以及一些小技巧'
date: 2026-02-09 16:30:29
updated: 2026-02-09 16:30:29
---

# AMD GPU Reset BUG
内核参数添加 `amdgpu.reset_method=0`
