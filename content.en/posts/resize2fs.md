---
title: "Resize Linux Disk Partition"
date: 2021-11-01T20:59:21+08:00
keywords: ["linux", "resize2fs"]
tags: ["linux", "resize2fs"]
---

## Expand Partition Size

```bash
growpart /dev/sda 1
resize2fs /dev/sda1
```

## Shrink Partition Size

**Shrink Filesystem**

```bash
resize2fs /dev/sdb1 24G
```

Multiply the output 4k value by 4 for the following operations

**Resize Partition**
```bash
fdisk /dev/sdb
d
n
```