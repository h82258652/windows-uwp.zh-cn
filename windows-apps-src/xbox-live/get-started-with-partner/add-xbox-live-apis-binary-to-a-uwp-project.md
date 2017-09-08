---
title: "向 UWP 项目添加 Xbox Live API 二进制文件"
author: KevinAsgari
description: "了解如何使用 NuGet 向 UWP 项目添加 Xbox Live API 二进制文件包。"
ms.assetid: 1e77ce9f-8a0e-402c-9f46-e37f9cda90ed
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, nuget"
ms.openlocfilehash: f861b4b53dd4f1e89eb32c1cc1d9428e6576dcd1
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="add-xbox-live-apis-binary-package-to-your-uwp-project"></a>向 UWP 项目添加 Xbox Live API 二进制文件包

你可以通过遵循以下步骤，使用 NuGet 将最新的 Xbox Live API 导入游戏：

### <a name="1-ensure-you-have-the-windows-10-rtm-and-visual-studio-2015-or-later-installed"></a>1. 确保你安装了 Windows 10 RTM 和 Visual Studio 2015 或更高版本。

- Visual Studio 2015。  请参阅 [https://www.visualstudio.com/en-us/downloads/visual-studio-2015-downloads-vs.aspx](https://www.visualstudio.com/en-us/downloads/visual-studio-2015-downloads-vs.aspx)
- Windows 10 SDK v10.0.14393.0 或更高版本 [https://developer.microsoft.com/zh-CN/windows/downloads/windows-10-sdk](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)

### <a name="2--ensure-you-have-the-latest-nuget-package-manager-installed"></a>2. 确保你安装了最新的 NuGet 程序包管理器

1.  查看当前版本：
    - 在菜单栏上，依次选择“工具”->“扩展和更新”。
    - 在“已安装”选项卡下，查找 `NuGet Package Manager`
![](../images/nuget/nuget_uwp_install_1.png)
2.  更新当前版本：
    - 在菜单栏上，依次选择“工具”->“扩展和更新”。
    - 在“更新”->“Visual Studio 库”选项卡下，选择 `Update`
![](../images/nuget/nuget_uwp_install_2.png)

### <a name="3--add-reference-to-the-project"></a>3. 为项目添加引用
1.  右键单击项目解决方案，并选择 `Manage NuGet Packages`
![](../images/nuget/nuget_uwp_install_6.png)
1.  搜索 `Xbox Live` 并选择相应的程序包，然后单击 `Install`。
  - Xbox 服务 API 同时支持 UWP 和 XDK 以及 C++ 和 WinRT。  
  - 在 `Microsoft.Xbox.Live.SDK.*.UWP` 和 `Microsoft.Xbox.Live.SDK.*.XboxOneXDK` 之间选择。  `XboxOneXDK`  适用于 ID@Xbox 和使用 Xbox One XDK 的托管开发人员。  `UWP`  适用于可在电脑、Xbox One 或 Windows Phone 上运行的 UWP 游戏。  有关在 Xbox One 上运行 UWP 的更多详细信息，请访问 [https://docs.microsoft.com/zh-cn/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)
  - 在 `Microsoft.Xbox.Live.SDK.Cpp.*` 和 `Microsoft.Xbox.Live.SDK.WinRT.*` 之间选择。 `Cpp`  适用于使用 Xbox Live API 的 C++ 游戏引擎。  `WinRT`  是使用 Xbox Live API 通过 C++、C# 或 Javascript 语言编写的游戏。  在将 WinRT 与 C++ 引擎结合使用时，将使用有乘幂号 (^) 的 C++/CX。  `Cpp`  是建议用于 C++ 游戏引擎的 API。    
  -  如果你是 Xbox Live 创意者计划的一员，可以使用以下任一选项：1) 适用于 C++ UWP 游戏引擎的 Microsoft.Xbox.Live.SDK.Cpp.UWP，2) 适用于 C# 或 JavaScript UWP 的 Microsoft.Xbox.Live.SDK.WinRT.UWP。 当结合使用 WinRT 与 C++ 引擎时，你可以使用运用乘幂号 (^) 的 C++/CX。  Microsoft.Xbox.Live.SDK.Cpp.UWP 是用于 C++ 游戏引擎的推荐 API。 3) Unity。  请参阅[使用 Unity 开发创意者主题作品](../get-started-with-creators/develop-creators-title-with-unity.md)文章获取详细信息。
![](../images/nuget/nuget_uwp_install_7.png)
1. 接受 License TOS 后，等到成功添加程序包。  你应该在“程序包管理器”输出窗口中看到此日志：

```
========== Finished ==========
```

### <a name="4--optionally-include-header"></a>4.（可选）包括标头
* 对于基于 `Microsoft.Xbox.Live.SDK.Cpp.*` 的项目，`#include <xsapi\services.h>` 位于项目的源中。
* 对于基于 `Microsoft.Xbox.Live.SDK.WinRT.*` 的项目，无需包括任何标头。   
