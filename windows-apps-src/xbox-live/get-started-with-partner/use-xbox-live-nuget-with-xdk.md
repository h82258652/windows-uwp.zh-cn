---
title: 结合使用 Xbox Live NuGet 程序包与 XDK
author: KevinAsgari
description: 了解如何使用 Xbox Live API NuGet 程序包开发 XDK 主题作品。
ms.assetid: 2c5ae514-393d-48bb-afd8-a897d35f7938
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, NuGet
ms.localizationpriority: medium
ms.openlocfilehash: b8b12201c0511339c4dd38824e17f7586e03708e
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5479829"
---
# <a name="use-the-xbox-live-api-nuget-package-to-develop-xdk-titles"></a>使用 Xbox Live API NuGet 程序包开发 XDK 主题作品

### <a name="1--ensure-you-have-the-latest-nuget-package-manager-installed"></a>1.  确保你安装了最新的 NuGet 程序包管理器
1.  查看当前版本：
    - 在菜单栏上，依次选择“工具”->“扩展和更新”。
    - 在“已安装”选项卡下，查找 `NuGet Package Manager`
![](../images/nuget/nuget_uwp_install_1.png)
2.  更新当前版本：
    - 在菜单栏上，依次选择“工具”->“扩展和更新”。
    - 在“更新”->“Visual Studio 库”选项卡下，选择 `Update`
![](../images/nuget/nuget_uwp_install_2.png)

### <a name="2--add-reference-to-the-project"></a>2.  添加对项目的引用
1.  添加对项目的引用
    1.  右键单击项目解决方案，然后选择“管理 NuGet 程序包”
<br/>
![](../images/nuget/nuget_xbox_install_4.png)
1.  搜索 `Xbox Live` 并选择相应的程序包，然后单击 `Install`。
  - Xbox 服务 API 同时支持 UWP 和 XDK 以及 C++ 和 WinRT。  
  - 在 `Microsoft.Xbox.Live.SDK.*.UWP` 和 `Microsoft.Xbox.Live.SDK.*.XboxOneXDK` 之间选择。  `XboxOneXDK`  适用于 ID@Xbox 和使用 Xbox One XDK 的托管开发人员。  `UWP`  适用于可在电脑、Xbox One 或 Windows Phone 上运行的 UWP 游戏。  你可以阅读更多有关 Xbox One 上运行 UWP[https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)
  - 在 `Microsoft.Xbox.Live.SDK.Cpp.*` 和 `Microsoft.Xbox.Live.SDK.WinRT.*` 之间选择。 `Cpp`  适用于使用 Xbox Live API 的 C++ 游戏引擎。  `WinRT`  适用于使用 Xbox Live API 通过 C++、C# 或 Javascript 语言编写的游戏引擎。  在将 WinRT 与 C++ 引擎结合使用时，将使用有乘幂号 (^) 的 C++/CX。  `Cpp`  是建议用于 C++ 游戏引擎的 API。    
![](../images/nuget/nuget_xbox_install_5.png)
![](../images/nuget/nuget_uwp_install_7.png)
1. 接受 License TOS 后，等到成功添加程序包。  你应该在“程序包管理器”输出窗口中看到此日志：

```
========== Finished ==========
```

### <a name="3--optionally-include-header"></a>3.  （可选）包括标头
* 对于基于 `Microsoft.Xbox.Live.SDK.Cpp.*` 的项目，`#include <xsapi\services.h>` 位于项目的源中。
* 对于基于 `Microsoft.Xbox.Live.SDK.WinRT.*` 的项目，无需包括任何标头。   
