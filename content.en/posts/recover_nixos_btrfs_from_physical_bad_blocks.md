---
title: "Recovering NixOS from Physical SSD Bad Blocks: A Btrfs & NVMe Repair Guide"
date: 2026-06-01T10:00:00+08:00
description: "A deep dive into fixing a corrupted NixOS system caused by NVMe physical bad sectors, involving btrfs csum errors and manual /nix/store recovery."
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

This skill guides the user or executes recovery procedures when a NixOS system on a Btrfs filesystem suffers from physical SSD bad sectors (NVMe I/O errors) that corrupt specific paths in the read-only `/nix/store`.

It bridges the gap between hardware-level block remapping and software-level Nix store state alignment.

<!--more-->

---

## Trigger Conditions
*   `sudo nix-store --verify --check-contents --repair` hangs or crashes with kernel I/O errors.
*   `dmesg` reports `critical medium error` or `failed command: READ FPDMA QUEUED` on an NVMe device.
*   `dmesg` reports `BTRFS warning: csum failed` on a specific inode.

---

## Execution Protocol

### Phase 1: Mitigate Physical Bad Blocks (Hardware Level)
*When a physical sector is unreadable, standard OS tools will hang. We must force the SSD controller to retire and remap the bad block by writing zeros to it.*

1.  Identify the failing LBA (Logical Block Address) from `dmesg` (e.g., `sector 31547800`).
2.  Use the low-level NVMe utility to force-write zeros to the corrupted block range:
    ```bash
    sudo nvme write-zeroes /dev/<nvme_device> -s <start_lba> -c <block_count>
    # Example: sudo nvme write-zeroes /dev/nvme0n1 -s 31547800 -c 200
    ```
    *Note: This bypasses Btrfs and forces the SSD controller to remap the bad NAND flash memory.*

---

### Phase 2: Resolve Btrfs Checksum (CSUM) Mismatches (Filesystem Level)
*After writing zeros, the hardware error is gone, but the filesystem now detects corrupted (zeroed-out) data because the Btrfs checksum does not match.*

1.  Trigger a read to catch the checksum error and locate the corrupted **Inode**:
    ```bash
    sudo btrfs scrub start -B /
    # Check dmesg for: "BTRFS warning: csum failed ... ino <inode_number>"
    ```
2.  Map the corrupted Inode to its actual file path on `/nix/store`:
    ```bash
    sudo btrfs inspect-internal inode-resolve <inode_number> /
    # Alternative if the above fails:
    sudo find / -inum <inode_number> 2>/dev/null
    ```

---

### Phase 3: Purge Corrupted Nix Store Paths (Package Manager Level)
*Since `/nix/store` is immutable (read-only), we must temporarily remount it as read-write to delete the corrupted package.*

1.  Remount the Nix Store with write permissions:
    ```bash
    sudo mount -o remount,rw /nix/store
    ```
2.  Change permissions and delete the corrupted package directory (use the path found in Phase 2):
    ```bash
    sudo chmod -R +w /nix/store/<hash>-<package-name>
    sudo rm -rf /nix/store/<hash>-<package-name>
    ```

---

### Phase 4: Nix Database Reconstruction & Validation (System Level)
*Force Nix to realize the missing path, download a healthy copy from the binary cache, and re-verify system integrity.*

1.  Force Nix to download/rebuild the deleted store path:
    ```bash
    sudo nix-store --realise /nix/store/<hash>-<package-name>
    ```
2.  *(Optional but recommended)* Run a system-wide NixOS repair to fix any cascading dependencies:
    ```bash
    sudo nixos-rebuild switch --repair
    ```
3.  Perform a final, deep verification check:
    ```bash
    sudo nix-store --verify --check-contents
    ```

---

## Expected Outcome
*   **Hardware:** SSD bad blocks are successfully remapped.
*   **Filesystem:** Btrfs `csum` errors are cleared.
*   **OS:** `/nix/store` database integrity is restored, and all corrupted packages are re-downloaded to pristine, verified states.
