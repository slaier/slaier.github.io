---
weight: 3
title: "VLAN"
---

A LAN is divided into multiple logical VLANs. Each VLAN is a broadcast domain. Communication between hosts in a VLAN is the same as in a LAN, while VLANs cannot communicate with each other directly. In this way, broadcast packets are restricted to a VLAN.

<!--more-->

## VLAN Frame Format

The IEEE 802.1Q protocol stipulates that a 4-byte VLAN tag (also known as VLAN Tag, or Tag for short) is added after the destination MAC address and source MAC address fields of an Ethernet data frame and before the protocol type field to identify VLAN information.

![Vlan Tag](/images/switch/vlan_tag.jpeg)

The meaning of each field of the 802.1q Tag:

  * TPID: 2 bytes in length, indicating the frame type. A value of 0x8100 indicates an 802.1q Tag frame.
  * PRI: Priority, 3 bits in length, indicating the priority of the frame. The value ranges from 0 to 7. The larger the default value, the higher the priority. In general, when a switch deploys QoS, it preferentially sends data frames with high priority.
  * CFI: Canonical Format Indicator, 1 bit in length, indicating whether the MAC address is in canonical format. A CFI of 0 indicates that it is in canonical format, and a CFI of 1 indicates that it is in non-canonical format. It is used to distinguish between Ethernet frames, FDDI (Fiber Distributed Digital Interface) frames, and Token Ring frames. In Ethernet, the value of CFI is 0.
  * VID: VLAN ID, 12 bits in length, indicating the VLAN to which the frame belongs. Since 0 and 4095 are reserved values for the protocol, the valid value range of the VLAN ID is 1 to 4094.


In a VLAN switching network, Ethernet frames mainly have the following two forms:

  - Tagged frame: A frame with a 4-byte VLAN tag added.
  - Untagged frame: The original frame without a 4-byte VLAN tag added.

## VLAN Division

In order to improve processing efficiency, all data frames processed inside the switch are Tagged frames.

When the port receives an untagged frame, it needs to assign a vlan tag to it.

You can add a tag to a data frame in the following ways:

- Based on flow (high priority)
- vlan translation
- Based on mac address
- Based on subnet
- Based on protocol
  - FrameType
    EthernetII, IEEE 802.3 SNAP, IEEE 802.3 LLC
  - EtherType
    IPV4, IPX-RAW, IPX-LLC, etc.
- Based on port, the network administrator configures a default VLAN (PVID) for each port of the switch

VLAN division based on ports has the lowest priority, but it is the most commonly used VLAN division method.

## Basic Concepts of VLAN

According to the different processing methods of Tag tags when the port forwards packets, the link types of the port can be divided into three types:

- Access Link

  The packets sent out by the port do not have a tag. It is generally used to connect to terminal devices that cannot recognize VLAN tags, or when it is not necessary to distinguish between different VLAN members.

- Trunk Link

  For the packets sent out by the port, the packets in the default VLAN of the port do not have a tag, and the packets in other VLANs must have a tag. It is usually used for interconnection between network transmission devices.

- Hybrid Link

  For the packets sent out by the port, you can set the packets in some VLANs to have a tag and the packets in some VLANs to not have a tag as needed.

  Hybrid type ports can be used for interconnection between network transmission devices and can also be directly connected to terminal devices.

Differences in sending and receiving packets for different link type ports:

- Access Port

  Only belongs to the default VLAN.

  When receiving an untagged frame, add the Tag of the default VLAN.

  When receiving a tagged frame, if the vlan is the same as the default vlan, receive the packet; if the vlan is different from the default vlan, discard the packet.

  When sending a frame, strip the Tag.

- Trunk Port

  Can join multiple VLANs.

  When receiving an untagged frame, add the Tag of the default VLAN.

  When receiving a tagged frame, if the vlan is in the allowed vlan list, receive the packet, otherwise discard the packet.

  When sending a frame, if the vlan is the default vlan, strip the tag, otherwise keep the tag.

- Hybrid Port

  Can join multiple VLANs.

  When receiving an untagged frame, add the Tag of the default VLAN.

  When receiving a tagged frame, if the vlan is in the allowed vlan list, receive the packet, otherwise discard the packet.

  When sending a frame, if the vlan is configured with a tag, keep the tag, otherwise strip the tag.

## QINQ

Because the VLAN Tag field defined in IEEE802.1Q has only 12 bits and can only represent 4096 VLANs, it cannot meet the needs of identifying a large number of users in the Ethernet, so QinQ technology came into being.

QinQ (802.1Q-in-802.1Q) expands the VLAN space by adding another layer of 802.1Q Tag on the basis of the 802.1Q tagged packet.

Inner VLAN Tag: is the user's private network VLAN Tag, Customer VLAN Tag (CVLAN for short). The device relies on this Tag to transmit packets in the private network.

Outer VLAN Tag: is the public network VLAN Tag assigned to the user by the operator, Service VLAN Tag (SVLAN for short). The device relies on this Tag to transmit QinQ packets in the public network.

The switch can add, modify, and delete the inner and outer layer Tags.