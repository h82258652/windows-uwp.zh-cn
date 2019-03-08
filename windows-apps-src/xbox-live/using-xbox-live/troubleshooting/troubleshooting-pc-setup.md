---
title: Windows 电脑上的 Xbox Live 设置疑难解答
description: 了解如何在 Windows 电脑上解决 Xbox Live 开发环境问题。
ms.assetid: 9cfebdcd-0c1c-4fc2-9457-e7e434b6374c
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 疑难解答
ms.localizationpriority: medium
ms.openlocfilehash: c1f055a49fe34be35335e50dc8b1efbfb7b9b922
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57647732"
---
# <a name="troubleshooting-xbox-live-setup-on-windows-pc"></a>Windows 电脑上的 Xbox Live 设置疑难解答

在 Windows 10 PC 上，你可以确保你的计算机是正确使用这些步骤进行设置：

1. 更改您的计算机以指向示例的目的在于运行 XDKS.1 沙盒。  通过运行以下脚本执行此操作：

        {*SDK source root*}\Tools\SwitchSandbox.cmd XDKS.1

1. 提取 SDK 中的压缩文件“SourcesAndSamples.zip”的内容。
1. 打开一个示例解决方案：
    1. 对于 C++ API：{*SDK source root*}\Samples\Social\UWP\Cpp\Social.Cpp.140.sln
    1. 对于带 C# 的 WinRT API：{*SDK source root*}\Samples\Social\UWP\CSharp\Social.CSharp.140.sln
    1. 对于带 C++/CX 的 WinRT API：{*SDK source root*}\Samples\TitleStorage\UWP\CppCx\TitleStorageUniversal.sln
1. 将构建目标平台更改为“Win32”或“x64”。
1. 右键单击解决方案并重建所有内容。
1. 在调试程序中启动应用。
1. 在使用创建开发帐户登录[Xbox 开发人员门户](https://xdp.xboxlive.com)，或使用在中获得授权的零售开发人员帐户[合作伙伴中心](https://partner.microsoft.com/dashboard)。
1. 授予应用访问 Xbox Live 信息的权限。
1. 验证该应用是否可以检索你的信息以及你是否能够看到你的玩家代号。