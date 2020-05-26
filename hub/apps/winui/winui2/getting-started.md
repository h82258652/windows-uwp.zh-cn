---
title: Windows UI 库入门
description: 如何安装并使用 Windows UI 库。
ms.topic: reference
ms.date: 05/08/2020
keywords: windows 10, uwp, 工具包 sdk
ms.openlocfilehash: 585475df4138c6a5d4d8b885582137c972a64287
ms.sourcegitcommit: 3a7f9f05f0127bc8e38139b219e30a8df584cad3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2020
ms.locfileid: "83775868"
---
# <a name="getting-started-with-the-windows-ui-library"></a>Windows UI 库入门

[WinUI 2.4](release-notes/winui-2.4.md) 是 WinUI 的最新稳定版本，应该用于生产环境中的应用。

该库以 NuGet 包的形式提供，可以添加到任何新的或现有的 Visual Studio 项目中。

> [!NOTE]
> 要详细了解如何试用 WinUI 3.0 早期预览版，请参阅 [WinUI 3.0 预览版 1](../winui3/index.md)。

## <a name="download-and-install-the-windows-ui-library"></a>下载并安装 Windows UI 库

1. 下载 [Visual Studio 2019](https://developer.microsoft.com/windows/downloads)，确保在 Visual Studio 安装程序中选择“通用 Windows 平台开发”工作负载。

2. 打开现有项目，或使用“Visual C#”>“Windows”>“通用”下的“空白应用”模板或适用于语言投影的模板创建一个新项目。  

    > [!IMPORTANT]
    > 若要使用 WinUI 2.4，必须在项目属性中设置 TargetPlatformVersion > = 10.0.18362.0 和 TargetPlatformMinVersion > = 10.0.15063.0。

3. 在“解决方案资源管理器”面板中，右键单击项目名称，然后选择“管理 NuGet 包”。 选择“浏览”选项卡，然后搜索 **Microsoft.UI.Xaml** 或 **WinUI**。 然后选择要使用哪个 [Windows UI 库 NuGet 包](nuget-packages.md)。
**Microsoft.UI.Xaml** 包包含适用于所有应用的 Fluent 控件和功能。  
可以选择勾选“包括预发行版”，了解包含实验性新功能的最新预发行版。

    ![NuGet 程序包](images/ManageNugetPackages.png "管理 NuGet 包映像")

    ![NuGet 包](images/NugetPackages.png)

4. 将 Windows UI (WinUI) 主题资源添加到 App.xaml 资源。 可以通过两种方式来这样做，具体取决于你是否有其他应用程序资源。

    a. 如果没有其他应用程序资源，请将 `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` 添加到 Application.Resources：

    ``` XAML
    <Application>
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </Application>
    ```

    b. 否则，如果有多个应用程序资源集，请将 `<XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>` 添加到 Application.Resources.MergedDictionaries：

    ``` XAML
    <Application>
        <Application.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
        </Application.Resources>
    </Application>
    ```

    > [!IMPORTANT]
    > 添加到 ResourceDictionary 的资源的顺序会影响它们的应用顺序。 `XamlControlsResources` 字典会重写许多默认的资源键，因此应先将其添加到 `Application.Resources`，使之不会在应用中重写任何其他自定义样式或资源。 有关资源加载的详细信息，请参阅 [ResourceDictionary 和 XAML 资源引用](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)。

5. 将对工具包的引用添加到 XAML 页和代码隐藏页。

    * 在 XAML 页的页面顶部添加引用

        ```xaml
        xmlns:controls="using:Microsoft.UI.Xaml.Controls"
        ```

    * 在代码中（如果想要使用类型名称而不对其进行限定），可以添加 using 指令。

        ```csharp
        using MUXC = Microsoft.UI.Xaml.Controls;
        ```

## <a name="additional-steps-for-a-cwinrt-project"></a>C++/WinRT 项目的其他步骤

将 NuGet 包添加到 C++/WinRT 项目时，此工具会在项目的 `\Generated Files\winrt` 文件夹中生成一组投影头文件。 若要将这些头文件引入项目中，以便解析对这些新类型的引用，可以进入预编译的头文件（通常为 `pch.h`）中，将它们包括进去。 下面是一个示例，其中包含为 **Microsoft.UI.Xaml** 包生成的头文件。

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

如果需要通过完整的分步演练来了解如何为 C++/WinRT 项目添加对 Windows UI 库的简单支持，请参阅[一个简单的 C++/WinRT Windows UI 库示例](/windows/uwp/cpp-and-winrt-apis/simple-winui-example)。

## <a name="contributing-to-the-windows-ui-library"></a>为 Windows UI 库贡献内容

WinUI 是托管在 GitHub 上的开源项目。

我们欢迎你在 [Windows UI 库存储库](https://aka.ms/winui)中提供 Bug 报告、提交功能请求和贡献社区代码。

## <a name="other-resources"></a>其他资源

如果不熟悉 UWP，建议在开发人员门户中访问 [UWP 开发入门](https://developer.microsoft.com/windows/getstarted)页。
