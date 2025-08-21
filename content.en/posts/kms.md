---
title: "Activate Windows and Office with KMS"
date: 2021-08-07T20:12:30+08:00
keywords: ["kms", "windows", "office"]
tags: ["kms", "windows", "office"]
pin: true
---

From 神代綺凛 ["KMS Activation"](https://moe.best/kms.html)

<!--more-->

KMS (Key Management Services) is a key management service. This is an activation mechanism for enterprises (multi-user, large customers) that has appeared since the Vista era. It adopts the "client/server corresponding management method". That is, a computer with KMS service is configured within the enterprise, and other computers within the enterprise can be connected to the computer with KMS service for batch activation.

## KMS
Supports activation of **VL version** of Windows and Office. If it is not VL, please see the corresponding instructions.
The validity period of each successful activation is `180 days`, and it will be automatically renewed every `180` days thereafter, as long as you have an internet connection and the KMS server of this site is not down.

## Windows
1. If you have used other keys or are not VL, please use [Microsoft's official key][1] to convert to VL first.
2. **Open** Command Prompt or PowerShell **as an administrator** and run the following commands
    ```powershell
    cd /d "%SystemRoot%\system32"
    slmgr /skms kms.lolico.moe
    slmgr /ato
    slmgr /xpr
    ```
3. Enjoy~

## Office
1. Download [Office Tool Plus](https://otp.landian.la/)
2. Open the software and switch to the "Activation" tab
3. If you are not a VL version of Office, please select the volume license version of your corresponding Office suite in "Retail to Volume License Conversion" above and then install the license
4. Enter `kms.lolico.moe` in "KMS Server Address" and click "Set Server Address"
5. Go back and click the "Activate" button in the middle of the bottom
6. Enjoy~

[1]: https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612867(v=ws.11)