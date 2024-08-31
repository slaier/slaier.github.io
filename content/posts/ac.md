---
title: "Android Cheatsheet"
date: 2024-07-14T12:05:58+08:00
keywords: ["android", "aosp"]
tags: ["android", "aosp"]
---

刷机备忘录

<!--more-->

## APP List

| name              | version                                 | Note         |
| ----------------- | --------------------------------------- | ------------ |
| Binary Eye        | Latest                                  | Open Source  |
| DAVx5             | Latest                                  | Open Source  |
| Fcitx5            | Latest                                  | Open Source  |
| Password Store    | Latest                                  | Open Source  |
| PiliPala          | 1.0.23.0505                             | Open Source  |
| SchildiChat       | Latest                                  | Open Source  |
| Termux            | Latest                                  | Open Source  |
| Tieba Lite        | [4.0.0-beta1](https://t.me/tblite/97)   | Open Source  |
| c001apk           | [559bea7(40)](https://t.me/c001apk/183) | Open Source  |
| ntfy              | Latest                                  | Open Source  |
| org.fossify.notes | Latest                                  | Open Source  |
| otp helper        | Latest                                  | Open Source  |
| Bmap              | 7.240415                                | Close Source |
| Mi Buds M8        | Latest                                  | Close Source |
| Via               | Latest                                  | Close Source |
| Leaf Calendar     | Latest                                  | Close Source |
| 1DM+              | Latest                                  | Mod          |
| AccuBattery       | Latest                                  | Mod          |
| WPS Office        | Latest                                  | Mod          |
| Alipay            | Latest                                  |              |
| Ct client         | 10.5.0                                  |              |
| Ticktok           | Latest                                  |              |
| beisen italent    | Latest                                  |              |
| boc               | Latest                                  |              |
| cmb               | 11.5.0                                  |              |
| jingdong          | Latest                                  |              |
| meituan           | Latest                                  |              |
| qq                | Latest                                  |              |
| taobao            | Latest                                  |              |
| wechat            | Latest                                  |              |
| xianyu            | Latest                                  |              |

## Backup Check List

### APP Data

- [x] DAVx5
- [x] Password Store
- [x] org.fossify.notes
- [x] via
- [x] wechat

### Directory

- [x] DCIM
- [x] Pictures

## Network setting

### Connectivity check

```shell
adb shell settings put global captive_portal_https_url https://connectivitycheck.platform.hicloud.com/generate_204
adb shell settings put global captive_portal_http_url http://connectivitycheck.platform.hicloud.com/generate_204
```

### NTP

```shell
adb shell settings put global ntp_server ntp.sjtu.edu.cn
adb shell cmd network_time_update_service force_refresh
adb shell cmd network_time_update_service dump
```
