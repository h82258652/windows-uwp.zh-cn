---
author: normesta
Description: "本文包含桌面到 UWP 桥的已知问题。"
Search.Product: eADQiWindows 10XVcnh
title: "桌面到 UWP 桥：已知问题"
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.openlocfilehash: 1923d7eb8d2d4956d68a10d3727251eee0398b06
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-known-issues"></a>桌面到 UWP 桥：已知问题

本文包含桌面到 UWP 桥的已知问题。

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>带有错误代码 0x139 (KERNEL_SECURITY_CHECK_FAILURE) 的蓝屏

在从 Windows 应用商店安装或启动某些应用后，计算机可能由于以下错误而意外重新启动：**0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)**。

已知受影响的应用包括 Kodi、JT2Go、Ear Trumpet、Teslagrad 和其他应用。

[Windows 更新（版本 14393.351 - KB3197954）](https://support.microsoft.com/kb/3197954)于 2016 年 10 月 27 日发布，该更新包含解决此问题的重要修补程序。 如果你遇到此问题，请更新计算机。 如果由于计算机在你可以登录前便重新启动，从而无法更新电脑，应使用系统还原将系统恢复到早于你安装受影响应用之一时的时间点。 有关如何使用系统还原的信息，请参阅 [Windows10 中的恢复选项](https://support.microsoft.com/help/12415/windows-10-recovery-options)。

如果更新未解决问题或者你不确定如何恢复电脑，请联系 [Microsoft 支持](https://support.microsoft.com/contactus/)。

如果你是开发人员，你可能希望阻止在不包含此更新的 Windows 版本上安装桌面桥应用。 请注意，通过执行此操作，应用将对尚未安装该更新的用户不可用。 若要限制应用对已安装此更新的用户的可用性，请修改 AppxManifest.xml 文件，如下所示：

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

可在以下位置找到关于 Windows 更新的详细信息：
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history
