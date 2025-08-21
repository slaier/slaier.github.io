---
weight: 2
title: "Ethernet Protocol"
---

The protocols defined in the data link layer include Ethernet protocol, High-Level Data Link Control (HDLC), Point-to-Point Protocol (PPP), Frame Relay (FR) protocol, etc. The most common is the Ethernet protocol.

<!--more-->

## MAC Address

A MAC address has 48 bits (6 bytes) and is represented in hexadecimal. The first 24 bits are allocated by the IEEE, and the last 24 bits are specified by the manufacturer that actually produces the network device.

MAC addresses can be divided into unicast MAC addresses, broadcast MAC addresses, and multicast MAC addresses. A unicast MAC address uniquely identifies a terminal on the Ethernet; a broadcast MAC address is used to represent all terminal devices on the network, all of which are 1, represented as `ff:ff:ff:ff:ff:ff`; a multicast MAC address is used to represent a group of terminals on the network, with the 8th bit being 1, for example, the mac of protocol packets such as lacp and flow control is `01:80:C2:xx:xx:xx`.

## Ethernet Frame

Ethernet II, IEEE 802.Q.

|   DA    |   SA    |  Type   |    PayLoad    |   FCS   |
| :-----: | :-----: | :-----: | :-----------: | :-----: |
| 6 Bytes | 6 Bytes | 2 Bytes | 46~1500 Bytes | 4 Bytes |

+ `DA`(Desstination Address): `MAC` address of the destination node
+ `SA`(Source Address): `MAC` address of the source node
+ `Type`: <=1500 is `length`, >=1536 is `Ethertype`
+ `PayLoad`: Payload of the upper layer protocol
+ `FCS`: 4-byte checksum

In Ethernet, the minimum frame length is 64 bytes, which is determined by the maximum transmission distance and the collision detection mechanism.

Common Ethernet types:

`0x8100`: VLAN

`0x88A8`: QINQ

`0x0800`: IP protocol

`0x0806`: ARP protocol

`0x0835`: RARP protocol

When a packet has multiple layers of headers, such as QINQ and IP-IN-IP, the one closer to the Ethernet header is called the outer layer, and the one closer to the PayLoad is called the inner layer.

## Preamble
Each Ethernet frame starts with an 8-byte preamble when it is sent.

`Preamble` is 7 bytes of data with alternating 1s and 0s (1 0 1 0 1 0 ...). The function of this part is to notify the receiver that a data frame is coming, so that it can be synchronized with the input clock. The 56-bit mode allows the station to discard some bits at the beginning of the frame.

`SFD` is a byte 10101011. Finally, 11 is used to notify the receiver that the next field is the address of the destination host. In fact, the preamble is added at the physical layer and is not part of the frame.

## Interframe Gap

When Ethernet transmits data, there is an Inter Frame Gap (IFG) between every two frames. The function of the interframe gap is to make the signal in the medium in a stable state, and at the same time allow the frame receiver to perform necessary processing on the received frame (such as adjusting the cache pointer, updating the count, and sending an interrupt to let the host process the packet), which is usually 12 bytes.

## Switching Mode

The switching modes of a switch are Cut-Through and Store-and-Forward.

In Cut-Through mode, the switch starts forwarding after receiving the destination address. The switch does not detect errors and directly forwards the data frame with low latency.

In Store-and-Forward mode, the switch starts the forwarding process only after receiving the complete data frame. The switch detects errors. Once an error is found, the packet will be discarded, and the latency is relatively large.