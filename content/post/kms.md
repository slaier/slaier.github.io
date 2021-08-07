---
title: "KMS激活Windows和Office"
date: 2021-08-07T20:12:30+08:00
keywords: ["kms", "windows", "office"]
tags: ["kms", "windows", "office"]
---

转自神代綺凛[《KMS激活》](https://moe.best/kms.html)

<!--more-->

KMS（Key Management Services）密钥管理服务。这是从Vista时代开始出现的一种针对企业（多用户、大客户）的激活机制，采用了“客户/服务器对应管理的方式”。即企业内部配置一台有KMS服务的电脑，企业内部其他电脑联接到KMS服务的电脑即可进行批量激活。

## KMS
支持激活 **VL版本** 的 Windows 和 Office，如果不是 VL 请看对应说明。
每次激活成功的有效期是`180天`，之后每过`180`天会自动续期，只要你有网而且本站的 KMS 服务器没宕。

## Windows
1. 如果你已经使用过其他密匙或者非 VL，请先使用[微软官方的密匙][1]转换成 VL
2. **以管理员身份打开** 命令提示符 或者 PowerShell，运行以下命令
    ```powershell
    cd /d "%SystemRoot%\system32"
    slmgr /skms kms.lolico.moe
    slmgr /ato
    slmgr /xpr
    ```
3. Enjoy~

## Office
1. 下载 [Office Tool Plus](https://otp.landian.la/)
2. 打开软件，切换到“激活”标签
3. 如果你不是 VL 版 Office，请先在上方“零售批量版本互转”中选择你对应的 Office 套件的批量版然后安装许可证
4. 在“KMS 服务器地址”处输入`kms.lolico.moe`，然后点击“设定服务器地址”
5. 返回，点击最下面中间的那个“激活”
6. Enjoy~

[1]: https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612867(v=ws.11)
