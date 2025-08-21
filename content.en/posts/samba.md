---
title: "Using Samba on Debian"
date: 2021-11-01T20:46:40+08:00
keywords: ["linux", "samba"]
tags: ["linux", "samba"]
bookFlatSection: true
---

## Server

**Install Samba**

```bash
apt install samba -y
```

**Enable write permission**

Locate the definition of the homes share
```
   read only = no
```

**Add Samba user**

```bash
smbpasswd -a $USER
```

**Restart Samba service**

```bash
service smbd restart
```


## Client

**Install cifs-utils**

```bash
apt install cifs-utils -y
```

**Mount Samba**

```bash
mount -t cifs -o username=xxx,uid=xxx,gid=xxx //server_ip/user /path/to/mount
```