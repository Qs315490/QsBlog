---
title: "man添加中文语言包"
date: 2021-1-20
updated: 2021-1-20
description: "man是Linux下命令帮助文档集锦，默认语言是英语。需要安装中文包以显示中文。"
tag: [linux,language,man]
categories: [linux]
---
`man`是Linux下命令帮助文档集锦，默认语言是英语。需要安装中文包以显示中文。

# 安装

{% tabs install %}

<!-- tab Ubuntu&Debian -->

```sh
sudo apt-get install manpages-zh
```

<!-- endtab -->

<!-- tab Arch Linux&Manjaro -->

```sh
sudo pacman -S man-pages-zh_CN
```

<!-- endtab -->

<!-- tab Centos -->

```sh
sudo yum install man-pages-zh-CN.noarch
```

<!-- endtab -->

{% endtabs %}
