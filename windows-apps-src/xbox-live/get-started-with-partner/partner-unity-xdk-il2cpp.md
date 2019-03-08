---
title: 适用于带有 IL2CPP 后端的 XDK 的 Unity
description: 针对 ID@Xbox 和托管的合作伙伴，为适用于 XDK 带有 IL2CPP 脚本后端的 Unity 添加 Xbox Live 支持
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, Unity
ms.localizationpriority: medium
ms.openlocfilehash: cfd722ca0d0b080f6395680cd62000cea9b402fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608462"
---
# <a name="add-xbox-live-support-to-unity-for-xdk-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>针对 ID@Xbox 和托管的合作伙伴，为适用于 XDK 带有 IL2CPP 脚本后端的 Unity 添加 Xbox Live 支持

## <a name="overview"></a>概述

Windows 运行时对 Unity 中 IL2CPP 的支持

在 Unity 5.6f3 发布后，该引擎已加入了一项新功能，开发人员通过在游戏项目中直接包含 Windows 运行时 (WinRT) 组件即可直接在脚本中进行使用。 在 Unity 5.6 之前的版本中，开发人员需要使用插件或 dll 来支持游戏脚本中的任何平台功能（包括 Xbox Live SDK）。 这个新的投影层不仅消除了对插件的要求，还引入了一种新的简化工作流（只有选择了 IL2CPP 脚本后端的游戏才可使用此工作流）。

有关如何开始使用的详细信息，请参阅 Unity 文档： https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>步骤

**1） 安装 Unity**

安装 Unity 5.6 或更高版本，并确保你已安装 Xbox One 编辑器扩展。

**2) 安装 Visual Studio Tools for Unity 3.1 或更高版本，以在使用 WinMD 时获得 IntelliSense 支持** 对于 Visual Studio 2015，可以到 https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity 获取该组件。  对于 Visual Studio 2017，该组件可以添加到 Visual Studio 2017 安装程序中。

**3） 打开新的或现有的 Unity 项目**

**4） 切换到 Xbox One Unity 生成设置菜单中的平台**

**5） 启用 IL2CPP 在 Unity 播放器设置中，脚本后的端和 API 兼容性设置为.NET 4.6**

![](../images/unity/unity-il2cpp-1.png)

**6） 切换到了 Roslyn 脚本编译器**

**7） 的 Xbox One 的相应的系统库将所有自动添加到你的项目，并包含平台二进制文件所需的任何额外步骤。**

**8） 添加和附加新的 C\#对 Unity 对象的脚本。**

例如，单击一个 Unity 对象，如"Main Camera"，然后单击"添加组件" \| "新脚本" \| C\#脚本\|并将其命名为"XboxLiveScript"。 可对任何游戏对象执行此操作。

**9） 打开该脚本在 Visual Studio 中 （使用 VSTU 3.1 + 安装)**

你将看到两个项目，在 VSTU 生成的“Player”项目中打开你的游戏脚本 XboxLiveTest.cs

![](../images/unity/unity-il2cpp-2.png)

这是为 XDK 生成的特殊项目，其中还包含对你放置在资源中的 winmd 文件的引用。
它还会为你定义“#if ENABLE_WINMD_SUPPORT”，以使 IntelliSense 和语法突出显示功能正常工作。

**10） 将 Xbox Live 的以下代码添加到 XboxLiveTest.cs 源文件**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Windows.Xbox.System.User mCurrentUser = null;
    XboxLiveContext mContext = null;
#endif

    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        mCurrentUser = Windows.Xbox.ApplicationModel.Core.CoreApplicationContext.CurrentUser;
        mContext = new XboxLiveContext(mCurrentUser);
#endif
    }

    // Update is called once per frame
    void Update()
    {
    }

    void OnGUI()
    {
        if(mContext != null) Gui.TextArea(new Rect(10,10,50,200), mContext.XboxUserId);
    }
}

```

**11） 请确保你已在播放机设置中找到的发布设置中选择 InternetClient 功能**

![](../images/unity/unity-il2cpp-3.png)

**12） 生成在 Unity 项目。**
