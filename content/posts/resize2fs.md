---
title: "Linux调整硬盘分区大小"
date: 2021-11-01T20:59:21+08:00
keywords: ["linux", "resize2fs"]
tags: ["linux", "resize2fs"]
---

## 扩充分区大小

```bash
growpart /dev/sda 1
resize2fs /dev/sda1
```

## 缩小分区大小

**缩小文件系统**

```bash
resize2fs /dev/sdb1 24G
```

将输出的4k值*4用于下面的操作

**调整分区**
```bash
fdisk /dev/sdb
d
n
```
