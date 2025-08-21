---
title: "Android Cheat Sheet"
date: 2024-07-14T12:05:58+08:00
keywords: ["android", "aosp"]
tags: ["android", "aosp"]
---

Flashing Memo

<!--more-->

## APP List

| name              | version         | tags |
| ----------------- | --------------- | ---- |
| Binary Eye        | Latest          | OSS  |
| DAVx5             | Latest          | OSS  |
| *DataBackup*      | Latest          | OSS  |
| Fcitx5            | Latest          | OSS  |
| Password Store    | Latest          | OSS  |
| PiliPala          | 1.0.23.0505     | OSS  |
| SchildiChat       | Latest          | OSS  |
| Termux            | Latest          | OSS  |
| Tieba Lite        | 4.0.0-beta1[^1] | OSS  |
| c001apk           | 559bea7(40)[^2] | OSS  |
| ntfy              | Latest          | OSS  |
| org.fossify.notes | Latest          | OSS  |
| otp helper        | Latest          | OSS  |
| AGC               | Latest          | CSS  |
| Bmap              | 7.240415        | CSS  |
| Leaf Calendar     | Latest          | CSS  |
| Mi Buds M8        | Latest          | CSS  |
| Notify for Xiaomi | Latest          | CSS  |
| Sleep             | Latest          | CSS  |
| Via               | Latest          | CSS  |
| 1DM+              | Latest          | Mod  |
| *AccuBattery*     | Latest          | Mod  |
| WPS Office        | Latest          | Mod  |
| Alipay            | Latest          | CNS  |
| Ct client         | 10.5.0          | CNS  |
| Ticktok           | Latest          | CNS  |
| beisen italent    | Latest          | CNS  |
| boc               | Latest          | CNS  |
| cmb               | 11.5.0          | CNS  |
| jingdong          | Latest          | CNS  |
| meituan           | Latest          | CNS  |
| qq                | Latest          | CNS  |
| taobao            | Latest          | CNS  |
| wechat            | Latest          | CNS  |
| wheelview         | Latest          | CNS  |
| xianyu            | Latest          | CNS  |

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

[^1]: https://t.me/tblite/97
[^2]: https://t.me/c001apk/183