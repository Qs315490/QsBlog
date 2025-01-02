---
title: 交换机VLAN Tag
date: "2022/2/19"
updated: 2022/2/19
tag: [思科,交换机,VLAN Tagging,VLAN Trunk,虚拟局域网中继技术,IEEE 802.1q]
categories: [思科,交换机]
---
# 介绍
VLAN Trunk  (虚拟局域网中继技术)
作用是让连接在不同交换机上的相同VLAN中的主机互通。

一般来说，交换机的一个端口只能承载单个VLAN的流量，如果希望一个端口承载多个VLAN 的流量，管理员就需要在交换机上使用干道（[Trunk](#1)）技术。

# 作用
如果交换机 1 的VLAN1 中的机器要访问交换机 2 的VLAN1中的，我们可以把两台交换机的级联端口设置为Trunk端口，这样，当交换机把数据包从级联口发出去的时候，会在数据包中做一个标记（TAG），以使其它交换机识别该数据包属于哪一个VLAN，这样，其它交换机收到这样一个数据包后，只会将该数据包转发到标记中指定的VLAN，从而完成了跨越交换机的VLAN内部数据传输。

# Trunk运用的2种协议

{% tabs Trunk %}

<!-- tab 802.1Q -->

基本标准的IEEE协议，属业界标准。  
802.1Q是在不破坏原数据帧的情况下在中间插入了区分Vlan的信息。
{% link 802.1q_百度百科::https://baike.baidu.com/item/802.1q %}

<!-- endtab -->

<!-- tab ISL（Inter-Switch Link 交换机间链路） -->

Cisco专有的Trunk封装方式。  
ISL相当于在外面再打了一层包.在原数据帧的头尾都加了东西。
{% link ISL_百度百科::https://baike.baidu.com/item/ISL %}

<!-- endtab -->

{% endtabs %}

## 不同部分
因为封装的形式不同导致ISL与没有做ISL封装的普通数据帧无法识别，无法通信，
而802.1Q没有破坏原数据帧结构，所以802.1Q可以与没有做Trunk封装的标准数据帧兼容，正常通信。
 
# 结论
802.1Q比ISL好用。所以尽可能的使用802.1Q封装。而且ISL的私有性也决定了它使用的会比标准少。

# 范列
交换机间使用VLAN划分3个虚拟局域网： 1（[交换机默认VLAN](#2)），2，3。要求交换机将网口设置为Trunk模式后不影响Trunk端口的无VLAN Tag通信。

列如：  
交换机1配置：
| |eth0|eth1|eth2|eth3|
|-|-|-|-|-|
|vlan1|No tag|No Tag|OFF|OFF|
|vlan2|Tag|OFF|No Tag|OFF|
|vlan3|Tag|OFF|OFF|No Tag|

交换机2配置：
| |eth0|eth1|eth2|eth3|
|-|-|-|-|-|
|vlan1|No tag|OFF|OFF|No TAG|
|vlan2|Tag|No Tag|OFF|OFF|
|vlan3|Tag|OFF|No Tag|OFF|

VLAN中的设备：
{% tabs vlan %}

<!-- tab VLAN1 -->

交换机1的eth0，eth1
交换机2的eth0，eth3

<!-- endtab -->

<!-- tab VLAN2 -->

交换机1的eth2
交换机2的eth1

<!-- endtab -->

<!-- tab VLAN3 -->

交换机1的eth3
交换机2的eth2

<!-- endtab -->

{% endtabs %}

其中eth0即做VLAN Trunk端口又做普通交换机端口

<br id=1>
<br id=2><br>
Trunk: 在单条物理链路上承载多个VLAN的流量。一般用在交换机与交换机之间。

默认VLAN：交换机默认所有端口都是VLAN0（无TAG）