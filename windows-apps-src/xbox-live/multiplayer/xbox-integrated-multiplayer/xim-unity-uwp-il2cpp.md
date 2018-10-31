---
title: 使用 XIM（带有 IL2CPP 的 Unity）
author: KevinAsgari
description: 通过带有 IL2CPP 脚本后端的适用于 UWP 的 Unity 使用 Xbox 集成多人游戏
ms.author: kevinasg
ms.date: 04/03/2018
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, Unity, Xbox 集成多人游戏
ms.localizationpriority: medium
ms.openlocfilehash: 4171fa830059eb557106ad3a7c485a6e96deeec6
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5816995"
---
# <a name="use-xim-unity-with-il2cpp"></a>使用 XIM（带有 IL2CPP 的 Unity）

## <a name="overview"></a>概述

Windows 运行时对 Unity 中 IL2CPP 的支持

在 Unity 5.6f3 发布后，该引擎已加入了一项新功能，开发人员通过在游戏项目中直接包含 Windows 运行时 (WinRT) 组件即可直接在脚本中进行使用。 在 Unity 5.6 之前的版本中，开发人员需要使用插件或 dll 来支持 UWP 游戏脚本中的任何平台功能（包括 Xbox 集成多人游戏）。 这个新的投影层不仅消除了对插件的要求，还引入了一种新的简化工作流（只有选择了 IL2CPP 脚本后端的游戏才可使用此工作流）。

- 有关如何使用 WinRT 和 Unity 的详细信息，请参阅 [Unity 文档](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)。
- 有关如何向使用 IL2CPP 的 Unity 添加 Xbox Live 支持的详细信息，请参阅有关该主题的 [Xbox Live 相关文档](https://docs.microsoft.com/windows/uwp/xbox-live/get-started-with-partner/partner-add-xbox-live-to-unity-uwp)。

## <a name="using-the-xim-unity-asset-package"></a>使用 XIM Unity 资产包

### <a name="1-install-unity"></a>1. 安装 Unity

安装 Unity 5.6 或更高版本，并确保你在安装过程中选择了 **Windows Store Il2CPP 脚本后端**

### <a name="2-install-visual-studio-tools-for-unity-version-31-and-above-for-intellisense-support-when-using-winmds"></a>2. 安装 Visual Studio Tools for Unity 3.1 或更高版本，以在使用 WinMD 时获得 IntelliSense 支持

对于 Visual Studio 2015，可以在 [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity) 找到该组件。 对于 Visual Studio 2017，可以在 Visual Studio 2017 安装程序内部添加该组件。

### <a name="3-open-a-new-or-existing-unity-project"></a>3. 打开新的或现有的 Unity 项目

### <a name="4-switch-the-platform-to-universal-windows-platform-in-the-unity-build-settings-menu"></a>4. 在“Unity 生成设置”菜单中将平台切换到通用 Windows 平台

![Unity 生成设置菜单中选中了通用 Windows 平台生成设置](../../images/xboxintegratedmultiplayer/xim-unity-build.png)

### <a name="5-enable-il2cpp-scripting-backend-in-the-unity-player-settings-and-set-api-compatibility-to-net-46"></a>5. 在 Unity 播放器设置中启用 IL2CPP 脚本后端，并将 API 兼容性设置为 .NET 4.6

![在 Unity 播放器设置菜单中的配置部分，“API 兼容性”设置为 ".NET 4.6"](../../images/unity/unity-il2cpp-1.png)

### <a name="6-import-the-latest-version-of-the-xbox-integrated-multiplayer-winrt-unity-asset-package"></a>6. 导入最新版本的 Xbox 集成多人游戏 WinRT Unity 资产包

资产包位置在 https://github.com/Microsoft/xbox-integrated-multiplayer-unity-plugin/releases

### <a name="7-you-can-now-use-xim-in-your-scripts"></a>7. 现在可以在脚本中使用 XIM

有关如何通过 C# 使用 XIM 的更多指南，请参阅[使用 XIM (C#)](using-xim-cs.md)。

下面的代码段显示了如何将 XIM 集成到你的代码：

```cs

using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

#if ENABLE_WINMD_SUPPORT
using Microsoft.Xbox.Services.XboxIntegratedMultiplayer;
#endif

public class XimScript
{
    public void Start()
    {
#if ENABLE_WINMD_SUPPORT
        XboxIntegratedMultiplayer.TitleId = XboxLiveAppConfiguration.SingletonInstance.TitleId;
        XboxIntegratedMultiplayer.ServiceConfigurationId = XboxLiveAppConfiguration.SingletonInstance.ServiceConfigurationId;
#endif
    }

    public void Update()
    {
#if ENABLE_WINMD_SUPPORT
        using (var stateChanges = XboxIntegratedMultiplayer.GetStateChanges())
        {
            foreach (var stateChange in stateChanges)
            {
                switch (stateChange.Type)
                {
                    case XimStateChangeType.PlayerJoined:
                        HandlePlayerJoined((XimPlayerJoinedStateChange)stateChange);
                        break;

                    case XimStateChangeType.PlayerLeft:
                        HandlePlayerLeft((XimPlayerLeftStateChange)stateChange);
                        break;
                    [...]
                }
            }
        }
#endif
    }
}
```

有关 `ENABLE_WINMD_SUPPORT` #define 指令的详细信息，请参阅 [Windows 运行时支持](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)上的 Unity 文档。

### <a name="8-required-capability-content"></a>8. 所需功能内容

使用 XIM 的应用程序本身需要同时通过 Internet 和本地网络连接并接受来自网络资源的连接。 此外，还需要获得对麦克风设备的访问权限，以支持语音聊天。 因此，应用应在播放器设置的发布设置中声明 "InternetClientServer"、"PrivateNetworkClientServer" 功能以及 "Microphone" 设备功能。

![Unity 功能菜单中选中了 "InternetClientServer"、"PrivateNetworkClientServer" 和 "Microphone" 功能](../../images/xboxintegratedmultiplayer/xim-unity-capability.png)

### <a name="9-build-the-project-in-unity"></a>9. 在 Unity 中生成项目。

1. 转到“文件”|“生成设置”，单击**通用 Windows 平台**，并确保你单击了**切换平台**

2. 单击“添加打开的场景”将当前场景添加到版本中

3. 在 SDK 组合框中，选择“通用 10”

4. 在 UWP 构建类型组合框中，选择“D3D”。如果你愿意，也可以选择“XAML”。

5. 单击 Unity 的“生成”，生成 UWP Visual Studio 项目，该项目将你的 Unity 游戏包装在 UWP 应用程序中。

    当提示你选择位置时，新建一个文件夹，以避免混淆，因为将创建大量新文件。 建议将该文件夹命名为 "Build"，然后选择该文件夹。

6. 将 XIM 的网络清单添加到项目中

    添加 networkmanifest.xml 文件：

    ![Visual Studio 的 networkmanifest.xml 属性](../../images/xboxintegratedmultiplayer/xim-unity-networkmanifest.png)

    有关网络清单及其内容的详细信息，请参阅 [XIM 项目配置](xim-manifest.md)。

7. 在 Visual Studio 中编译并运行 UWP 应用

这将启动类似正常的 UWP 应用的应用，并允许进行 Xbox Live 调用，因为它们需要 UWP 应用容器才能正常工作。

### <a name="10-rebuild-if-you-make-changes-to-anything-in-unity"></a>10. 如果对 Unity 中的任何内容进行更改，请重新生成

如果你在 Unity 中更改任何内容，则必须重新生成 UWP 项目
