---
weight: 4
title: "Layer 2 Switching"
---

Ethernet frames are forwarded in the Layer 2 switching network according to the destination MAC address until they are sent to the peer device.

<!--more-->

## Layer 2 Forwarding Principle

### Learning based on source MAC address

After receiving a data frame, the switch adds the mapping of the source MAC and the interface to the MAC table.

### Forwarding based on destination MAC address

After receiving a data frame, the switch matches the destination MAC address with the MAC address table. The interface of the matched MAC address table entry is the outgoing interface of the data frame.


## MAC Table

The Key value of the MAC table is usually MAC address + VLAN. The actions include sending from the port, discarding, etc.

MAC entries are divided into static MAC and dynamic MAC.

Static MAC will not be aged and is manually configured by the administrator.

Dynamic MAC will be aged and deleted after not being active for a period of time. It is a dynamically learned MAC address.

## MAC Learning

MAC learning is divided into hardware learning and software learning.

Hardware learning is automatically added to the mac table and then reported to the cpu.

Software learning reports the new mac to the cpu, and the cpu adds the mac entry by itself. The same mac entry will be filtered and will not be reported repeatedly.

## MAC Aging

There are two MAC aging mechanisms: Poll and Timeout.

### Poll mode

In Poll mode, the mac entry has a hit flag.

When a packet hits the MAC table, the hit will be updated to 1.

In each aging cycle, the entry with hit 1 will be updated to 0, and the entry with hit 0 will be deleted.

In summary, the aging time of a MAC entry is 1 to 2 aging cycles.

Same as MAC learning, hardware MAC aging is aged first and then notifies the cpu. Software aging first notifies the cpu, and the cpu deletes the mac entry by itself.

### Timeout mode

The MAC entry has a ttl value, which will be reset when the packet hits.

When the ttl becomes 0, a notification is sent to the cpu to delete the entry.

## MAC Flapping

MAC flapping means that the outgoing interface bound to the MAC address changes from port A to port B, which usually occurs when there is a loop in the network.

MAC flapping can be restricted by setting the flapping priority of the port. It can flap from low priority to high priority, but not from high priority to low priority.

## MAC Learning Number Limit

The maximum number of MAC learning entries for a single port, VLAN, link aggregation, and tunnel can be limited to prevent a single port from occupying the entire MAC table, which will cause other ports to fail to learn MAC normally.

The action after exceeding the limit can be permit, drop, or copy-to-cpu, that is, the action taken when the smac of the packet does not hit the mac table after the MAC table reaches the upper limit.

## MAC Table Action

In addition to specifying the outgoing interface, the action of the mac entry can also be source discard, destination discard, copy-to-cpu, go to the layer 3 process, etc.