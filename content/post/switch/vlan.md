---
title: "路由交换技术 - VLAN"
date: 2021-08-08T11:17:42+08:00
keywords: ["vlan", "switch", "l2"]
tags: ["vlan", "switch", "l2"]
---

把一个LAN划分成多个逻辑的VLAN，每个VLAN是一个广播域，VLAN内的主机间通信就和在一个LAN内一样，而VLAN间则不能直接互通，这样，广播报文就被限制在一个VLAN内。

<!--more-->

## VLAN的帧格式

IEEE 802.1Q协议规定，在以太网数据帧的目的MAC地址和源MAC地址字段之后、协议类型字段之前加入4个字节的VLAN标签（又称VLAN Tag，简称Tag），用以标识VLAN信息。

![Vlan Tag](/images/switch/vlan_tag.jpeg)

802.1q Tag各字段含义:

  * TPID：长度为2字节，表示帧类型。取值为0x8100时表示802.1q Tag帧。
  * PRI：Priority，长度为3比特，表示帧的优先级，取值范围为0～7，默认值越大优先级越高。一般情况下，当交换机部署QoS时，优先发送优先级高的数据帧。
  * CFI：Canonical Format Indicator，长度为1比特，表示MAC地址是否是经典格式。CFI为0说明是经典格式，CFI为1表示为非经典格式。用于区分以太网帧、FDDI（Fiber Distributed Digital Interface）帧和令牌环网帧。在以太网中，CFI的值为0。
  * VID：VLAN ID，长度为12比特，表示该帧所属的VLAN。由于0和4095为协议保留取值，所以VLAN ID的有效取值范围是1～4094。


在一个VLAN交换网络中，以太网帧主要有以下两种形式：

  - 有标记帧（Tagged帧）：加入了4字节VLAN标签的帧。
  - 无标记帧（Untagged帧）：原始的、未加入4字节VLAN标签的帧。

## VLAN 划分

为了提高处理效率，交换机内部处理的数据帧一律都是Tagged帧。

当端口收到untagged帧时，则需要为其分配vlan tag。

给数据帧加上标签可以通过以下方法：

- 基于流 (高优先级)
- vlan翻译
- 基于mac地址
- 基于子网
- 基于协议
  - FrameType
    EthernetII, IEEE 802.3 SNAP, IEEE 802.3 LLC
  - EtherType
    IPV4, IPX-RAW, IPX-LLC等
- 基于端口，网络管理员给交换机的每个端口配置缺省VLAN(PVID)

基于端口划分VLAN的优先级最低，但却是最常用的VLAN划分方式。

## VLAN的基本概念

根据端口在转发报文时对Tag标签的不同处理方式，可将端口的链路类型分为三种：

- 接入链路(Access Link)

  端口发出去的报文不带tag标签。一般用于和不能识别VLAN tag的终端设备相连，或者不需要区分不同VLAN成员时使用。

- 干道链路(Trunk Link)

  端口发出去的报文，端口缺省VLAN内的报文不带tag，其它VLAN内的报文都必须带tag。通常用于网络传输设备之间的互连。

- 混合链路(Hybrid Link)

  端口发出去的报文可根据需要设置某些VLAN内的报文带tag，某些VLAN内的报文不带tag。

  Hybrid类型端口既可以用于网络传输设备之间的互连，又可以直接连接终端设备。

不同链路类型端口收发报文的差异：

- 接入端口(Access Port)

  只属于缺省VLAN。

  接收untag帧时，添加缺省VLAN的Tag。

  接收tag帧时，vlan与缺省vlan相同时，接收该报文；vlan与缺省vlan不相同时，丢弃该报文。

  发送帧时，剥掉Tag。

- 干道端口(Trunk Port)

  可以加入多个VLAN。

  接收untag帧时，添加缺省VLAN的Tag。

  接收tag帧时，vlan在允许vlan列表内，接收该报文，否则该丢弃报文。

  发送帧时，vlan为缺省vlan时，剥掉tag，否则保留tag。

- 混合端口(Hybrid Port)

  可以加入多个VlAN。

  接收untag帧时，添加缺省VLAN的Tag。

  接收tag帧时，vlan在允许vlan列表内，接收该报文，否则该丢弃报文。

  发送帧时，如果该vlan配置的是tag则保留tag，否则剥掉tag。

## QINQ

因为IEEE802.1Q中定义的VLAN Tag域只有12个比特，仅能表示4096个VLAN，无法满足以太网中标识大量用户的需求，于是QinQ技术应运而生。

QinQ（802.1Q-in-802.1Q）通过在802.1Q标签报文的基础上再增加一层802.1Q的Tag来达到扩展VLAN空间的功能。

内层 VLAN Tag：为用户的私网 VLAN Tag，Customer VLAN Tag (简称 CVLAN)。设备依靠该 Tag 在私网中传送报文。

外层 VLAN Tag：为运营商分配给用户的公网 VLAN Tag， Service VLAN Tag(简称 SVLAN)。设备依靠该 Tag 在公网中传送 QinQ 报文。

交换机可对内外层Tag进行添加、修改、删除操作。
