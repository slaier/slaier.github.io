---
title: "debian使用samba"
date: 2021-11-01T20:46:40+08:00
keywords: ["linux", "samba"]
tags: ["linux", "samba"]
---

## 服务端

**安装samba**

```bash
apt install samba -y
```

**开启写权限**

定位到 homes 共享的定义部分
```
   read only = no
```

**添加samba用户**

```bash
smbpasswd -a $USER
```

**重启samb服务**

```bash
service smbd restart
```


## 客户端

**安装cifs-utils**

```bash
apt install cifs-utils -y
```

**挂载samba**

```bash
mount -t cifs -o username=xxx,uid=xxx,gid=xxx //server_ip/user /path/to/mount
```
