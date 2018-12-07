---
title: 适用于 UWP 带有 IL2CPP 后端的 Unity
description: 针对 ID@Xbox 和托管的合作伙伴，为适用于 UWP 带有 IL2CPP 脚本后端的 Unity 添加 Xbox Live 支持
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, Unity
ms.localizationpriority: medium
ms.openlocfilehash: ace950dec6a57a9550ea7b3fbe6c52b53855e2e0
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2018
ms.locfileid: "8746997"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>针对 ID@Xbox 和托管的合作伙伴，为适用于 UWP 带有 IL2CPP 脚本后端的 Unity 添加 Xbox Live 支持

## <a name="overview"></a>概述

Windows 运行时对 Unity 中 IL2CPP 的支持

在 Unity 5.6f3 发布后，该引擎已加入了一项新功能，开发人员通过在游戏项目中直接包含 Windows 运行时 (WinRT) 组件即可直接在脚本中进行使用。 在 Unity 5.6 之前的版本中，开发人员需要使用插件或 dll 来支持 UWP 游戏脚本中的任何平台功能（包括 Xbox Live SDK）。 这个新的投影层不仅消除了对插件的要求，还引入了一种新的简化工作流（只有选择了 IL2CPP 脚本后端的游戏才可使用此工作流）。

有关如何开始使用的详细信息，请参阅 Unity 文档：https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>步骤

**1) 安装 Unity**

安装 Unity 5.6 或更高版本，并确保你在安装过程中选择了 **Windows Store Il2CPP 脚本后端**

**2) 安装 Visual Studio Tools for Unity 3.1 或更高版本，以在使用 WinMD 时获得 IntelliSense 支持** 对于 Visual Studio 2015，可以到 https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity 获取该组件。  对于 Visual Studio 2017，可以在 Visual Studio 2017 安装程序内部添加该组件。

**3) 打开新的或现有的 Unity 项目**

**4) 在“Unity 生成设置”菜单中将平台切换到通用 Windows 平台**

**5) 在 Unity 播放器设置中启用 IL2CPP 脚本后端，并将 API 兼容性设置为 .NET 4.6**

![](../images/unity/unity-il2cpp-1.png)

**6) 导入最新版本的 Xbox Live WinRT Unity 资源包**，位置在 https://github.com/Microsoft/xbox-live-api/releases

**7) 将新的 C\# 脚本添加并附加到 Unity 对象。**

例如，单击“主相机”等 Unity 对象，然后依次单击“添加组件”\|“新建脚本”\| C\# Script \|，然后将其命名为“XboxLiveScript”。 可对任何游戏对象执行此操作。

**8) 在 Visual Studio（已安装 VSTU 3.1+）中打开脚本**

你将看到两个项目，在 VSTU 生成的“Player”项目中打开你的游戏脚本 XboxLiveTest.cs

![](../images/unity/unity-il2cpp-2.png)

这是为 UWP 生成的特殊项目，其中还包含对你放置在资源中的 winmd 文件的引用。
它还会为你定义“#if ENABLE_WINMD_SUPPORT”，以使 IntelliSense 和语法突出显示功能正常工作。

**9) 将以下 Xbox Live 代码添加到 XboxLiveTest.cs 源文件中**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new   Microsoft.Xbox.Services.System.XboxLiveUser();

    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
#endif
    string debugText = "";
    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;
        UIDispatcher = cw.Dispatcher;
        SignIn();
#endif
    }
    // Update is called once per frame
    void Update()
    {
    }
    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }
#if ENABLE_WINMD_SUPPORT
    async void SignIn()
    {
        Microsoft.Xbox.Services.System.SignInResult result = await m_user.SignInAsync(UIDispatcher);
        if (result.Status == Microsoft.Xbox.Services.System.SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }

    }
#endif
}

```

**10) 确保你在播放器设置的发布设置中选择了“InternetClient”功能**

![](../images/unity/unity-il2cpp-3.png)

**11) 在 Unity 中生成项目。**

1.  转到“文件”\|“生成设置”，单击**通用 Windows 平台**，并确保你单击了**切换平台**

2.  单击“添加打开的场景”将当前场景添加到版本中

3.  在 SDK 组合框中，选择“通用 10”

4.  在 UWP 构建类型组合框中，选择“D3D”。如果你愿意，也可以选择“XAML”。

5.  单击 Unity 的“生成”，生成 UWP Visual Studio 项目，该项目将你的 Unity 游戏包装在 UWP 应用程序中。 当提示你选择位置时，新建一个文件夹，以避免混淆，因为将创建大量新文件。 建议将该文件夹命名为“Build”，然后选择该文件夹

**12) 将 Xbox Live 配置添加到项目中**

添加 xboxservices.config 文件：

![](../images/unity/unity-il2cpp-4.png)

按照名为[将 Xbox Live 添加到新的或现有的 UWP 项目](get-started-with-visual-studio-and-uwp.md)的文档中的指示操作

> [!NOTE]
> Xboxservices.config 中的所有值都区分大小写。

**13) 在 Visual Studio 中编译并运行 UWP 应用**

这将启动类似正常的 UWP 应用的应用，并允许进行 Xbox Live 调用，因为它们需要 UWP 应用容器才能正常工作。

**14) 如果对 Unity 中的任何内容进行更改，请重新生成**
  
如果你在 Unity 中更改任何内容，则必须重新生成 UWP 项目。

请注意，当重新编译时，Unity 将替换你的 pfx 文件，这将导致 Xbox Live 登录失败，因此，你必须在 Unity 项目中更新它以避免此问题。

若要执行此操作，请转到“文件”\|“生成设置”，单击**通用 Windows 平台**播放器上的“生成设置”，然后单击 PFX 按钮以将 PFX 文件替换为上述文件。 你也可以在每次从 Unity 中重新生成项目时删除 PFX 文件。

## <a name="troubleshooting-common-issues"></a>常见问题疑难解答

**1)** 如果 Unity 出现无法加载关联脚本的情况，请确保你已执行步骤 3 将 WinMD 拖到了 Unity 项目资源面板中

**2)** 如果应用在启动时或尝试运行以下代码行时立即崩溃：

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

确保你已将 xboxservices.config 文件添加到项目及其属性中，并且将“生成操作”设置成了“内容”，将“复制到输出目录”设置成了“始终复制”。
还要确保它包含正确格式的 JSON（其带有十进制格式的 TitleId），例如：

```json
{
    "TitleId" : 928643728,
    "PrimaryServiceConfigId" : "3ebd0100-ace5-4aa4-ab9c-5b733759fa90"
}
```

**3)** 如果应用能够启动，但无法登录，请检查以下项：

a) 计算机已设置为开发人员沙盒。  使用 Xbox Live SDK 中 \Tools 文件夹下的 SwitchSandbox.cmd 脚本来执行此操作。

b) 你当前用于登录的 Xbox Live 帐户登录有权限访问开发人员沙盒。  常规的零售版 Xbox Live 帐户没有访问权限。  你可以使用 XDP 或合作伙伴中心创建测试帐户。

c) UWP 应用中的 package.appxmanfiest 已设为正确的标识。  你可以手动编辑它，但最简单的方法是在 Visual Studio 中右键单击项目，然后选择“应用商店”\|“将应用程序与应用商店关联”。

d) Unity 提供的 stock .pfx 文件没有正确的标识，可从磁盘中删除它，并在 .csproj 中删除引用它的行，或者在 Visual Studio 中右键单击项目，然后依次选择“应用商店”\|“将应用程序与应用商店关联”，该操作将放置正确的 .pfx 文件。  然后，请务必返回到 Unity，单击**通用 Windows 平台**播放器上的“生成设置”，然后单击 PFX 按钮，以将 .pfx 文件替换为通过 Visual Studio 的“Associate App with the Store”操作获得的文件。
