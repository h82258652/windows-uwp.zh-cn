---
title: 适用于 UWP 带有 .NET 脚本的 Unity
author: KevinAsgari
description: 针对 ID@Xbox 和托管的合作伙伴，为适用于 UWP 带有 .NET 脚本后端的 Unity 添加 Xbox Live 支持
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, Unity
ms.localizationpriority: medium
ms.openlocfilehash: ba2460e64d63892aa1ff02d2178f2632109b3655
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6276084"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-net-scripting-backend-for-idxbox-and-managed-partners"></a>针对 ID@Xbox 和托管的合作伙伴，为适用于 UWP 带有 .NET 脚本后端的 Unity 添加 Xbox Live 支持

**1) 安装 Unity**

安装 Unity 5.3 或更高版本，在安装过程中勾选“Windows 应用商店 .NET 脚本后端”组件。

![](../images/unity/unity1-install.png)

**2) 打开新的或现有的 Unity 项目**

它可以是 3D 或 2D 项目。 两种类型都适用于 Xbox Live SDK。

**3) 导入最新版本的 Xbox Live WinRT Unity 资源包，位置在 https://github.com/Microsoft/xbox-live-api/releases**

**4) 将新的 C\# 脚本添加并附加到 Unity 对象。**

例如，单击“主相机”等 Unity 对象，然后依次单击“添加组件”\|“新建脚本”\| C\# Script \|，然后将其命名为“XboxLiveScript”。 可对任何游戏对象执行此操作。

**5) 在 Unity 中生成项目。**

1.  转到“文件”\|“生成设置”，单击 Windows 应用商店，并确保你单击了“切换平台”

2.  单击“添加打开的场景”将当前场景添加到版本中

3.  在 SDK 组合框中，选择“通用 10”

4.  在 UWP 构建类型组合框中，选择“D3D”。如果你愿意，也可以选择“XAML”。

5.  单击“Unity C\# 项目”复选框，以生成 Assembly-Csharp.dll 项目

6.  单击 Unity 的“生成”，生成 UWP Visual Studio 项目，该项目将你的 Unity 游戏包装在 UWP 应用程序中。 当提示你选择位置时，新建一个文件夹，以避免混淆，因为将创建大量新文件。 建议将该文件夹命名为“Build”，然后选择该文件夹

![](../images/unity/unity3-buildsettings.png)


**6) 在 Visual Studio 中打开生成的 UWP 项目**

Unity 将打开资源管理器中的输出项目文件夹。  忽略此处的 .sln 文件。  而是导航到“Build”文件夹，然后在 Visual Studio 中打开生成的 .sln。  

你将在此解决方案中看到 3 个项目。

1.  Assembly-CSharp。 这就是 Xbox Live 脚本的所在位置

2.  Assembly-Csharp-firstpass。 对我们来说，可忽略此项目。

3.  基于项目名称的 UWP 应用。 这是托管 Unity 引擎的传统 UWP 应用。 你将在此处设置一些类似于传统 UWP 应用的 Xbox Live 配置。


**7) 将 Xbox Live 配置添加到 UWP 应用中**

按照名为[将 Xbox Live 添加到新的或现有的 UWP 项目](get-started-with-visual-studio-and-uwp.md)的文档中的指示操作

**8) 将 Xbox Live 代码添加到脚本中**

将此 Xbox Live 代码示例复制/粘贴到已附加到游戏对象的脚本中。 此脚本将显示在“Assembly-CSharp”项目中。 你可以根据需要更改代码。

```csharp
#if NETFX_CORE

using UnityEngine;
using System;
using Microsoft.Xbox.Services.System;

public class XboxLiveScript : MonoBehaviour
{
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();
    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
    string debugText = "";

    void Start()
    {
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;

        UIDispatcher = cw.Dispatcher;
        SignIn();
    }

    void Update()
    {
    }

    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }

    async void SignIn()
    {
        SignInResult result = await m_user.SignInAsync(UIDispatcher);

        if (result.Status == SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }
    }
}

#endif
```

**9) 在 Visual Studio 中编译并运行 UWP 应用**

这将启动类似正常的 UWP 应用的应用，并允许进行 Xbox Live 调用，因为它们需要 UWP 应用容器才能正常工作。

**10) 如果对 Unity 中的任何内容进行更改，请重新生成**
  
如果你在 Unity 中更改任何内容，则必须重新生成 UWP 项目。

请注意，当重新编译时，Unity 将替换你的 pfx 文件，这将导致 Xbox Live 登录失败，因此，你必须在 Unity 项目中更新它以避免此问题。

若要执行此操作，请转到“文件”\|“生成设置”，单击 Windows 应用商店播放器上的“生成设置”，然后单击 PFX 按钮以将 PFX 文件替换为上述文件。 你也可以在每次从 Unity 中重新生成项目时删除 PFX 文件。

## <a name="troubleshooting-common-issues"></a>常见问题疑难解答

**1)** 如果 Unity 出现无法加载关联脚本的情况，请确保你已执行步骤 3 将 WinMD 拖到了 Unity 项目资源面板中

**2)** 如果应用在启动时或尝试运行以下代码行时立即崩溃：

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

确保你已将 xboxservices.config 文件添加到项目及其属性中，并且将“生成操作”设置成了“内容”，将“复制到输出目录”设置成了“始终复制”。

> [!NOTE]
> Xboxservices.config 中的所有值都区分大小写。

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

d) Unity 提供的 stock .pfx 文件没有正确的标识，可从磁盘中删除它，并在 .csproj 中删除引用它的行，或者在 Visual Studio 中右键单击项目，然后依次选择“应用商店”\|“将应用程序与应用商店关联”，该操作将放置正确的 .pfx 文件。  然后，请务必返回到 Unity，单击 Windows 应用商店播放器上的“生成设置”，然后单击 PFX 按钮，以将 .pfx 文件替换为通过 Visual Studio 的“将应用程序与应用商店关联”操作获得的文件。
