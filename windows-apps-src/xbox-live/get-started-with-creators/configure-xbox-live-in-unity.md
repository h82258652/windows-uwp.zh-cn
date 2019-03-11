---
title: 在 Unity 中配置 Xbox Live
description: 了解如何使用 Xbox Live Unity 插件在 Unity 游戏中配置 Xbox Live。
ms.assetid: 55147c41-cc49-47f3-829b-fa7e1a46b2dd
ms.date: 01/25/2018
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, Unity, 配置
localizationpriority: medium
ms.openlocfilehash: d464fc54d322db9da91870bd3ca7cbc29957b379
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596722"
---
# <a name="configure-xbox-live-in-unity"></a>在 Unity 中配置 Xbox Live

> [!NOTE]
> Xbox Live Unity 插件仅建议用于[Xbox Live Creators 计划](../developer-program-overview.md)成员，因为当前没有成就或之多人游戏的支持。

与[Xbox Live Unity 插件](https://github.com/Microsoft/xbox-live-unity-plugin)、 添加到 Unity 游戏的 Xbox Live 支持非常简单，这使您更多时间来专注于以在最适合你的标题的方式使用 Xbox Live。

本主题将介绍在 Unity 中设置 Xbox Live 的过程。

## <a name="prerequisites"></a>必备条件

在 Unity 中使用 Xbox Live 之前，您将需要以下：

1.  **[Xbox Live 帐户](https://support.xbox.com/browse/my-account/manage-account/Create%20account)**。
1. 中的注册**[合作伙伴中心开发人员计划](https://developer.microsoft.com/store/register)**。
2. **[Windows 10 周年更新](https://microsoft.com/windows)** 或更高版本
3. **[Unity](https://store.unity.com/)** 版本**5.5.4p5** （或更高版本）， **2017.1p5** （或更高版本），或者**2017.2.0f3** （或更高版本） 与**[Microsoft Visual Studio Tools for Unity](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity)** 并**脚本编写后端的 Windows 应用商店.NET**。
4. **[Visual Studio 2015](https://www.visualstudio.com/)** 或**[Visual Studio 2017 15.3.3](https://www.visualstudio.com/)** （或更高版本） 与**通用 Windows 应用开发工具**。
5. **[Xbox Live 平台扩展 SDK](https://aka.ms/xblextsdk)**。


> [!NOTE]
> 如果你想要使用 Xbox Live 使用 IL2CPP 脚本编写后端，则需要 Unity 2017.2.0p2 或更高版本和 Xbox Live Unity 插件版本"1802年预览版本"或更高版本。


## <a name="import-the-unity-plugin"></a>导入 Unity 插件

要将插件导入到新的或现有的 Unity 项目中，请遵循以下步骤：

1. 导航到 Xbox Live Unity 插件版本选项卡上的[ https://github.com/Microsoft/xbox-live-unity-plugin/releases ](https://github.com/Microsoft/xbox-live-unity-plugin/releases)。
2. 下载**XboxLive.unitypackage**。
3. 在 Unity 中，单击**资产** > **导入包** > **自定义包**并导航到**XboxLive.unitypackage**.

![成功导入](../images/unity/get-started-with-creators/importXBL_Small.gif)

### <a name="optional-configure-the-plugin-to-work-in-the-unity-editor-net-46-or-il2cpp-only"></a>（可选）配置插件以使用在 Unity 编辑器中 （.NET 4.6 或仅 IL2CPP）

> [!NOTE]
> 支持更改 Unity 中的脚本运行时版本需要的 Xbox Live Unity 插件版本"1711年发行版"或更高版本的.NET 4.6 和版本"1802年预览版本"或更高的 IL2CPP。

有三个可以在 Unity 来定义如何编译你的代码中配置的设置：

1. **脚本后端**是编译器使用。 Unity 为通用 Windows 平台支持两个不同的脚本编写后端：.NET 和 IL2CPP。
2. **脚本运行时版本**是运行 Unity 编辑器脚本运行时的版本。
3. **API 兼容性级别**是 API 外围应用，你将生成针对您的游戏。

下表显示 Xbox Live Unity 插件的当前脚本的支持矩阵：

| 脚本编写后端     | 脚本运行时版本 | 支持     | 所需的最小的 Unity 版本 |
|-------------------    |-------------------        |-----------    |------------------------------- |
| IL2CPP                | .NET 3.5 等效       | 否            | 不适用                            |
| Il2CPP                | .NET 4.6 等效       | 是           | 2017.2.0p2                     |
| .NET                  | .NET 3.5 等效       | 是           | 相同为系统必备组件          |
| .NET                  | .NET 4.6 等效       | 是           | 相同为系统必备组件          |

我们为 Xbox Live Unity 插件，从"1711年发行版"版本添加了其他脚本的运行时支持。 默认情况下，该插件配置为使用.NET 后端脚本和脚本运行时版本的.NET 3.5 在 Unity 编辑器中运行。 如果你的项目使用.NET 4.6 的脚本运行时版本，需要配置插件以在编辑器中正常工作：

1. 在 Unity 项目资源管理器，导航到**Xbox Live\Libs\UnityEditor\NET46**和选择文件夹中的所有 Dll。
2. 在检查器窗口中，检查**编辑器**下**包括平台**。
3. 在 Unity 项目资源管理器，导航到**Xbox Live\Libs\UnityEditor\NET35**和选择文件夹中的所有 Dll。
4. 在检查器窗口中，取消选中**编辑器**下**包括平台**。

![更改脚本运行时](../images/unity/get-started-with-creators/changeScriptingRuntime.gif)

> [!IMPORTANT]
> 如果在项目中的脚本的运行时版本更改回 3.5 相反将需要这些步骤。

## <a name="set-visual-studio-as-the-ide-in-unity"></a>设置为在 IDE 中 Unity 的 Visual Studio

生成所需的 visual Studio[通用 Windows 平台 (UWP)](https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp)游戏。 可以通过转到 Visual Studio 的 Unity 中设置你的 IDE**编辑** > **首选项** > **外部工具**并设置**外部脚本编辑器**到 Visual Studio。

![设置 VS 外部工具](../images/unity/get-started-with-creators/setVSExternalTool_Small.gif)

## <a name="unity-plugin-file-structure"></a>Unity 插件文件结构

Unity 插件的文件结构分为以下几个部分：

* __Xbox Live__ 包含包括在发布的 **.unitypackage** 中的实际插件资源。
    * __编辑器__包含提供基本 Unity 配置 UI 的脚本，并在生成期间处理项目。
    * __示例__包含一组简单的场景文件，可显示如何使用各种 prefabs 并将其连接在一起。
    * __图像__是用于 prefabs 的一小组图像
    * __Libs__是 Xbox Live 库的存储位置。
    * __Prefabs__ 包含多个实现 Xbox Live 功能的 [Unity prefab](https://docs.unity3d.com/Manual/Prefabs.html) 对象。
    * __脚本__包含通过调用 Xbox Live Api 的所有代码文件。 这是查找有关如何正确调用 Xbox Live API 的示例的绝佳位置。
    * __Tools\AssociationWizard__包含 Xbox Live 关联向导，用于从应用程序配置下拉[合作伙伴中心](https://developer.microsoft.com/windows)Unity 中使用。

## <a name="enable-xbox-live"></a>启用 Xbox Live

为您的标题以与 Xbox Live 进行交互，您将需要设置 Xbox Live 的初始配置。 你可以轻松地在 Unity 的内部使用 Xbox Live 关联向导：

1. 在 **Xbox Live** 菜单中，选择**配置**。
2. 在 **Xbox Live** 窗口中，选择**运行 Xbox Live 关联向导**。
3. 在中**将在标题上与 Windows 应用商店相关联**对话框中，单击**下一步**，然后使用你的合作伙伴中心帐户登录。
4. 选择你要与此项目关联的应用，然后单击**选择**。 如果没有看到要关联的应用，尝试单击**刷新**。 或者，你可以通过预留一个名称，然后单击**预留**来创建新应用。
5. 系统将提示您启用 Xbox Live 如果还没有。 单击**启用**若要启用你的标题中的 Xbox Live。
6. 单击**完成**保存配置。

若要调用 Xbox Live 服务，您的桌面必须为开发人员模式下，因为你的标题是 Xbox Live 配置中设置为相同的沙盒。 验证可以通过查看这两**Xbox Live 配置**Unity 中的窗口：

1. **开发人员模式下配置**认为**已启用**。 如果显示**已禁用**，请单击**切换到开发人员模式**。
2. **标题配置** > **沙盒**应具有相同的 ID**开发人员模式下配置** > **开发人员模式**。

![已启用 XBL](../images/unity/unity-xbl-enabled.png)

请参阅[Xbox Live 沙盒](../xbox-live-sandboxes.md)沙盒有关的信息。

## <a name="build-and-test-the-project"></a>生成并测试该项目

在编辑器中运行你的主题作品时，如果尝试使用 Xbox Live 功能，你将会看到虚假数据。 例如，如果您[功能中添加登录](unity-prefabs-and-sign-in.md)到您的场景和尝试登录，你将看到**Fake 用户**显示为带有占位符图标的个人资料名称。 若要使用实际的配置文件登录并测试你的标题中的 Xbox Live 功能，将需要生成 UWP 解决方案并在 Visual Studio 中运行它。  可以通过执行以下步骤生成 Unity 中的 UWP 项目：

1. 打开**生成设置**通过选择窗口**文件** > **生成设置**。
2. 添加所有你想要在下，在生成中包括在后台**生成中的场景**部分。
3. 切换到**通用 Windows 平台**通过选择**通用 Windows 平台**下**平台**，然后单击**切换平台**.
4. 设置**SDK**到**10.0.15063.0**或更高版本。
5. 若要启用脚本调试检查**UnityC#项目**。
6. 单击**生成**并指定项目的位置。

![生成设置](../images/unity/build_settings.JPG)

完成生成后，Unity 会生成新的 UWP 解决方案文件将会需要在 Visual Studio 中运行：

1. 在您指定的文件夹，打开 **&lt;ProjectName&gt;.sln** Visual Studio 中。
2. 在顶部工具栏中，选择**x64**并将其部署到**本地计算机**。

如果启用了**脚本调试**从 Unity 生成 UWP 解决方案，然后你的脚本将位于下面**程序集 c# (通用 Windows)** 项目。

![虚设用户：123456789](../images/unity/get-started-with-creators/visualStudio.PNG)

> [!NOTE]
> 在使用之前在 Visual Studio 生成测试您的游戏使用实际数据，请按照[此清单](test-visual-studio-build.md)以帮助确保你的标题将能访问 Xbox Live 服务。

> [!IMPORTANT]
> 在可能的 2018 现已要求对 package.appxmanifest.xml 文件才能正确地在 Visual Studio 中测试 UWP 标题进行更新。 要实现此目的，请执行以下操作：
>
> 1. 搜索 package.appxmanifest.xml 文件在解决方案资源管理器
> 2. 右键单击该文件，并选择查看代码。  
    如果查看代码选项不可用或 package.appxmanifest 文件没有扩展名。 你将需要打开为 xml 文件，并继续执行剩余步骤。
> 3. 下`<Properties></Properties>`部分中，添加以下行： `<uap:SupportedUsers>multiple</uap:SupportedUsers>`。
> 4. 通过从 Visual Studio 中启动远程调试生成将部署到您的 Xbox 游戏。 您可以找到指令，以设置你的标题在 Xbox 上[设置在 Xbox 开发环境在 UWP](../../xbox-apps/development-environment-setup.md)一文。
>
> 配置已更改的一部分可能看起来它启用多玩家，但仍有必要在单用户方案中运行您的游戏。

## <a name="try-out-the-examples"></a>试用示例

你已经准备好开始使用 Unity 项目中的 Xbox Live 了！ 请尝试打开 **Xbox Live/示例**文件夹中的场景，以查看操作中的插件以及如何自行使用功能的示例。 在编辑器中运行的示例将为您提供模拟数据，但是，如果生成 Visual Studio 中的项目和[将您的 Xbox Live 帐户关联与沙盒配合使用](authorize-xbox-live-accounts.md)，可以使用你的玩家代号登录。

尝试使用 **SignInAndProfile** 场景登录你的 Microsoft 帐户，使用**排行榜**场景创建排行榜，并使用 **UpdateStat** 场景显示和更新统计数据。

## <a name="see-also"></a>另请参阅

* [在 Unity 中登录到 Xbox Live](unity-prefabs-and-sign-in.md)
* [为 Xbox Live 帐户授权](authorize-xbox-live-accounts.md)
