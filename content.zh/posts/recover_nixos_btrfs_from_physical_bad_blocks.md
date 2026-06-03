---
title: "从物理 SSD 坏块中恢复 NixOS：Btrfs 与 NVMe 修复指南"
date: 2026-06-01T10:00:00+08:00
description: "深入解析由 NVMe 物理坏扇区引起的损坏 NixOS 系统的修复方法，涉及 Btrfs csum 错误及手动 /nix/store 恢复。"
keywords:
  - NixOS
  - Btrfs
  - NVMe
  - nix-store repair
  - SSD Bad Blocks
  - write-zeroes
  - Checksum Error
  - Linux System Recovery
tags:
  - NixOS
  - Linux
  - Storage
  - Troubleshooting
  - Hardware
categories:
  - Technical Tutorial
  - System Administration
---

此技能指导用户执行恢复操作，当运行在 Btrfs 文件系统中的 NixOS 系统遇到物理 SSD 坏扇区（NVMe I/O 错误）时，这些错误会破坏只读 `/nix/store` 中的特定路径。该技能解决了硬件级块重映射与软件级 Nix 存储状态对齐之间的差距。

<!--more-->

---

## 触发条件
*   `sudo nix-store --verify --check-contents --repair` 因内核 I/O 错误而卡住或崩溃。
*   `dmesg` 报告 NVMe 设备出现`critical medium error`或`failed command: READ FPDMA QUEUED`。
*   `dmesg` 报告特定 inode 出现`BTRFS warning: csum failed`。

---

## 执行协议

### 阶段 1：缓解物理坏块问题（硬件级）
当某个物理扇区无法读取时，常规操作系统工具会卡住。我们必须强制 SSD 控制器清除并重新映射该坏块，通过向其中写入零值来实现。

1. 从 `dmesg` 中识别出故障的 LBA 地址（例如：`sector 31547800`）。
2. 使用低级 NVMe 工具向损坏的块区域写入零值：
    ```bash
    sudo nvme write-zeroes /dev/<nvme_device> -s <start_lba> -c <block_count>
    # 示例：sudo nvme write-zeroes /dev/nvme0n1 -s 31547800 -c 200
    ```
    *注意：这绕过了 Btrfs，强制 SSD 控制器重新映射损坏的 NAND 闪存。*

---

### 阶段 2：解决 Btrfs 校验和 (CSUM) 不匹配问题（文件系统级）
写入零值后，硬件错误已消除，但文件系统检测到损坏（被零值覆盖）的数据，因为 Btrfs 校验和不一致。

1. 触发读取操作以捕捉校验和错误并定位损坏的 **Inode**：
    ```bash
    sudo btrfs scrub start -B /
    # 查看 dmesg 中的： "BTRFS warning: csum failed ... ino <inode_number>"
    ```
2. 将损坏的 Inode 映射到 `/nix/store` 中的实际文件路径：
    ```bash
    sudo btrfs inspect-internal inode-resolve <inode_number> /
    # 如果上述方法失败，可使用：
    sudo find / -inum <inode_number> 2>/dev/null
    ```

---

### 阶段 3：清除损坏的 Nix 存储路径（包管理器级）
由于 `/nix/store` 是不可变的（只读），我们必须临时将其重新挂载为可写，以删除损坏的包。

1. 以可写权限重新挂载 Nix Store：
    ```bash
    sudo mount -o remount,rw /nix/store
    ```
2. 修改权限并删除损坏的包目录（使用阶段 2 中找到的路径）：
    ```bash
    sudo chmod -R +w /nix/store/<hash>-<package-name>
    sudo rm -rf /nix/store/<hash>-<package-name>
    ```

---

### 阶段 4：Nix 数据库重建与验证（系统级）
强制 Nix 识别缺失的路径，从二进制缓存中下载健康的副本，并重新验证系统完整性。

1. 强制 Nix 下载/重建被删除的存储路径：
    ```bash
    sudo nix-store --realise /nix/store/<hash>-<package-name>
    ```
2. （可选但推荐）运行全系统范围的 NixOS 修复以解决任何级联依赖问题：
    ```bash
    sudo nixos-rebuild switch --repair
    ```
3. 进行最终深度验证检查：
    ```bash
    sudo nix-store --verify --check-contents
    ```

---

## 预期结果
*   **硬件：** SSD 坏块成功重新映射。
*   **文件系统：** Btrfs `csum` 错误被清除。
*   **操作系统：** `/nix/store` 数据库完整性恢复，所有损坏的包重新下载到完好且经过验证的状态。
