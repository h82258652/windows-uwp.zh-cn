---
title: Windows 电脑上的 Xbox Live 设置疑难解答
description: 了解如何在 Windows 电脑上解决 Xbox Live 开发环境问题。
ms.assetid: 9cfebdcd-0c1c-4fc2-9457-e7e434b6374c
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 疑难解答
ms.localizationpriority: medium
ms.openlocfilehash: c1f055a49fe34be35335e50dc8b1efbfb7b9b922
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2018
ms.locfileid: "8730438"
---
# <a name="troubleshooting-xbox-live-setup-on-windows-pc"></a>Windows 电脑上的 Xbox Live 设置疑难解答

Windows 10 电脑上，你可以确保你的计算机设置正确使用以下步骤：

1. 更改你的计算机为指向示例设计运行的 XDKS.1 沙盒。  通过运行以下脚本执行此操作：

        {*SDK source root*}\Tools\SwitchSandbox.cmd XDKS.1

1. 提取 SDK 中的压缩文件“SourcesAndSamples.zip”的内容。
1. 打开一个示例解决方案：
    1. 对于 C++ API：{*SDK source root*}\Samples\Social\UWP\Cpp\Social.Cpp.140.sln
    1. 对于带 C# 的 WinRT API：{*SDK source root*}\Samples\Social\UWP\CSharp\Social.CSharp.140.sln
    1. 对于带 C++/CX 的 WinRT API：{*SDK source root*}\Samples\TitleStorage\UWP\CppCx\TitleStorageUniversal.sln
1. 将构建目标平台更改为“Win32”或“x64”。
1. 右键单击解决方案并重建所有内容。
1. 在调试程序中启动应用。
1. 登录上[Xbox 开发人员门户](https://xdp.xboxlive.com)中，创建的开发帐户或使用零售开发人员帐户有权在[合作伙伴中心](https://partner.microsoft.com/dashboard)。
1. 授予应用访问 Xbox Live 信息的权限。
1. 验证该应用是否可以检索你的信息以及你是否能够看到你的玩家代号。