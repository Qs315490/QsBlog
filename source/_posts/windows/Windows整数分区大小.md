---
title: Windows整数分区大小
tags: [Windows, 分区]
categories: [Windows, 分区]
description: ''
date: 2026-03-21 15:54:08
updated: 2026-03-21 15:54:08
---

# Windows整数分区大小

## 1. 前言

Windows 系统下，创建NTFS分区时，分区大小总会少一点，比如创建100GiB分区，实际会创建99GiB分区，这是因为Windows系统在创建分区时，会占用一部分空间，这部分空间的大小是固定的，不会随着分区剩余大小变化。


## 2. 分区大小计算/补偿分区开销

TiB（单位：MiB）+ 4.096 MiB(等于4GiB) = 总 MiB
例如：2,097,152 MiB + 4,096 MiB = 2,101,248 MiB

GiB（以 MiB 为单位）+ 1,024 MiB(等于1GiB) = 总 MiB
例如：512,000 MiB + 1,024 MiB = 513,024 MiB

# 查找表
分区大小|MiB
-|-
1 TB|1052672
2 TB|2101248
3 TB|3149824
4 TB|4198400
5 TB|5246976
6 TB|6295552
7 TB|7344128
8 TB|8392704
50 GB|52224
100 GB|103424
150 GB|154624
200 GB|205824
250 GB|257024
300 GB|308224
350 GB|359424
400 GB|410624
450 GB|461824
500 GB|513024
550 GB|564224
600 GB|615424
650 GB|666624
700 GB|717824
750 GB|769024
800 GB|820224
850 GB|871424
900 GB|922624
950 GB|973824

# 参考
[Create Perfect Round Number Hard Drive Partitions](https://community.spiceworks.com/t/create-perfect-round-number-hard-drive-partitions/1008237)