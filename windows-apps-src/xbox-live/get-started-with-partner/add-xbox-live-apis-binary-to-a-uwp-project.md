---
title: 向 UWP 项目添加 Xbox Live API 二进制文件
author: KevinAsgari
description: 了解如何使用 NuGet 向 UWP 项目添加 Xbox Live API 二进制文件包。
ms.assetid: 1e77ce9f-8a0e-402c-9f46-e37f9cda90ed
ms.author: kevinasg
ms.date: 11/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, nuget
ms.localizationpriority: medium
ms.openlocfilehash: 84d3ce8b56e5d1bf921eef48499d54b1d3fc4c22
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4356576"
---
# <a name="add-xbox-live-apis-binary-package-to-your-uwp-project"></a>向 UWP 项目添加 Xbox Live API 二进制文件包

## <a name="requirements"></a>要求

2. **[Windows 10](https://microsoft.com/windows)**。
3. **[Visual Studio](https://www.visualstudio.com/)**。 使用 Visual Studio 2015 更新 3 或更高版本，可以生成 UWP 应用。 我们建议你使用最新版本的 Visual Studio 开发人员和安全更新。
4. **[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** 或更高版本。

## <a name="add-the-binary-package-via-nuget"></a>通过 NuGet 添加二进制文件包

要从项目使用 Xbox Live API，可以通过使用 NuGet 程序包添加对二进制文件的引用或添加 API 源。 添加 NuGet 程序包可加快编译速度，而添加源可简化调试。 本文将演示如何使用 NuGet 程序包。 如果要使用源，请参阅[在 UWP 项目中编译 Xbox Live API 源](add-xbox-live-apis-source-to-a-uwp-project.md)。

Xbox 服务 API 同时支持 UWP 和 XDK 以及 C++ 和 WinRT，它们的命名空间结构为 **Microsoft.Xbox.Live.SDK.*.UWP** 和 **Microsoft.Xbox.Live.SDK.*.XboxOneXDK**。

1. **UWP** 适用于生成可在电脑、Xbox One 或 Windows Phone 上运行的 UWP 游戏的开发人员。
2. **XboxOneXDK** 适用于 ID@Xbox 和使用 Xbox One XDK 的托管开发人员。
3. C++ SDK 可用于 C++ 游戏引擎，而 WinRT SDK 用于以 C++、C# 或 JavaScript 编写的游戏引擎。
4. 在将 WinRT 与 C++ 引擎结合使用时，应使用有乘幂号 (^) 的 C++/CX。 C++ 是建议用于 C++ 游戏引擎的 API。  

> [!TIP]
> 阅读 [Xbox One 上的 UWP 应用开发入门](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started)可了解有关在 Xbox One 上运行 UWP 的更多信息。

可通过以下方式添加 Xbox Live SDK NuGet 程序包：

1. 在 Visual Studio 中，转到**工具** > **NuGet 包管理器** > **管理解决方案的 NuGet 程序包...**。
2. 在 NuGet 包管理器中，单击**浏览**并在搜索框中输入 **Xbox.Live.SDK**。
3. 从左侧列表中选择要使用的 Xbox Live SDK 版本。
3. 在窗口右侧，选中项目旁的复选框并单击**安装**。

> [!NOTE]
> Xbox Live 创意者计划开发人员必须使用 Xbox Live SDK UWP 版本之一，因为 XDK 不受支持。

![通过 NuGet 添加 XBL](../images/getting_started/vs-add-nuget-xbl.gif)

> [!IMPORTANT]
> 对于基于 `Microsoft.Xbox.Live.SDK.Cpp.*` 的项目，请确保在项目的源中包含标头 `#include <xsapi\services.h>`。
