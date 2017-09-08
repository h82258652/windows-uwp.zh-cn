---
title: "在 Unity 中配置 Xbox Live"
author: KevinAsgari
description: "了解如何使用 Xbox Live Unity 插件在 Unity 游戏中配置 Xbox Live。"
ms.assetid: 55147c41-cc49-47f3-829b-fa7e1a46b2dd
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, Unity, 配置"
ms.openlocfilehash: eecf41f579e16bca072e74d1024c355d6d156566
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="configure-xbox-live-in-unity"></a>在 Unity 中配置 Xbox Live

> **注意：**只建议 [Xbox Live 创意者计划](../developer-program-overview.md)成员使用 Xbox Live Unity 插件，因为当前不支持成就或多人游戏。

使用 Xbox Live Unity 插件可以轻松地向 Unity 游戏添加 Xbox Live 支持，让你有更多时间专注于以最适合主题作品的方式使用 Xbox Live。

本主题将介绍在 Unity 中设置 Xbox Live 的过程。

## <a name="prerequisites"></a>先决条件

你需要以下先决条件才能在 Unity 中配置 Xbox Live：

* [Xbox Live 帐户](https://support.xbox.com/browse/my-account/manage-account/Create%20account)
* [Windows 10 周年更新](https://microsoft.com/windows)或更高版本
* [Unity 5.5](https://store.unity.com/) 或更高版本
  * 安装时，务必选择 **Windows 应用商店 .NET 脚本后端**。
* [Visual Studio 2015](https://www.visualstudio.com/) 或更高版本
  * 任意版本的 Visual Studio 应该都适用，包括社区版。
  * 安装时，务必选择**通用 Windows 应用开发工具**下的所有内容。  你也可以修改安装，以为现有安装包括这些功能。

## <a name="import-the-unity-plugin"></a>导入 Unity 插件

要将插件导入到新的或现有的 Unity 项目中，请遵循以下步骤：

1. 导航到 [https://github.com/Microsoft/xbox-live-unity-plugin/releases](https://github.com/Microsoft/xbox-live-unity-plugin/releases) 上的 Xbox Live Unity 插件。
2. 下载 XboxLive.unitypackage
3. 双击 XboxLive.unitypackage 包，或者在 Unity 中单击“资源”|“导入包”|“自定义包”下的菜单，将其导入到 Unity 项目中。

![导入 Unity 程序包](../images/unity/unity-import.png)

## <a name="unity-plugin-file-structure"></a>Unity 插件文件结构

Unity 插件的文件结构分为以下几个部分：

* __Xbox Live__ 包含包括在发布的 **.unitypackage** 中的实际插件资源。
    * __编辑器__包含提供基本 Unity 配置 UI 的脚本，并在生成期间处理项目。
    * __示例__包含一组简单的场景文件，可显示如何使用各种 prefabs 并将其连接在一起。
    * __图像__是用于 prefabs 的一小组图像
    * __库__是将存储 Xbox Live 库的位置。
    * __Prefabs__ 包含多个实现 Xbox Live 功能的 [Unity prefab](https://docs.unity3d.com/Manual/Prefabs.html) 对象。
    * __脚本__包含所有实际上从 prefabs 调用 Xbox Live API 的代码文件。  这是查找有关如何正确调用 Xbox Live API 的示例的绝佳位置。
    * __Tools\AssociationWizard__ 包含 Xbox Live 关联向导，该向导用于从 [Windows 开发人员中心](https://developer.microsoft.com/windows)中下拉应用程序配置以用于 Unity.

## <a name="enable-xbox-live"></a>启用 Xbox Live

要在 Unity 项目中实际启用 Xbox Live，需要遵循以下步骤：

1. 在 **Xbox Live** 菜单中，选择**配置**。

    ![Xbox Live：配置](../images/unity/xbox-live-configuration.PNG)

2. 在 **Xbox Live** 窗口中，选择**运行 Xbox Live 关联向导**。

    ![Xbox Live：启用 Xbox Live](../images/unity/enable-xbox-live.PNG)

    > **注意：**你的设备必须处于开发人员模式才能调用 Xbox Live 服务。 启用 Xbox Live 后，你可以选择**切换到开发人员模式**，以切换为开发人员模式。

3. 在**将你的游戏与 Windows 应用商店关联**对话框中，单击**下一步**，然后使用开发人员中心帐户登录。

    ![Xbox Live：启用 Xbox Live](../images/unity/associate-game-with-store.png)

4. 选择你要与此项目关联的应用，然后单击**选择**。 如果没有看到要关联的应用，尝试单击**刷新**。 或者，你可以通过预留一个名称，然后单击**预留**来创建新应用。

5. 单击**启用**以启用游戏中的 Xbox Live。

    ![启用游戏的 Xbox Live 功能](../images/unity/associate-your-game-with-the-windows-store.PNG)

6. 单击**完成**保存配置。

7. 返回 Unity 中的 **Xbox Live** 窗口，确保**开发人员模式配置**下显示**已启用**。 如果显示**已禁用**，请单击**切换到开发人员模式**。

8. 确保你的主题作品与你的设备当前位于同一个沙盒。 你可以在**主题作品配置**下**沙盒**旁边查看主题作品所在的沙盒；你的设备所在的沙盒位于**开发人员模式配置**下**开发人员模式**旁边（括号中）。 请参阅 [Xbox Live 沙盒](../xbox-live-sandboxes.md)获取有关沙盒和如何切换设备上的沙盒的信息。 插件应该会自动更换设备上的沙盒。

    ![Xbox Live 已启用：匹配沙盒](../images/unity/unity-xbl-enabled.PNG)

9. 你现在已在 Unity 项目中成功启用 Xbox Live！

## <a name="build-and-test-the-project"></a>生成并测试该项目

在编辑器中运行你的主题作品时，如果尝试使用 Xbox Live 功能，你将会看到虚假数据。 例如，在 **SignInAndProfile** 示例场景中，如果你在编辑器中运行标题并尝试登录，你将看到**虚假用户 123456789** 显示为带有占位符图标的个人资料名称。

![虚假用户：123456789](../images/unity/unity-game-fake-data.PNG)

要使用真实的个人资料登录并测试主题作品中的 Xbox Live 功能，你需要生成主题作品，然后在 Visual Studio 中运行。

1. 在 Unity 中，通过选择**文件-> 生成设置**或按 **Ctrl + Shift + B** 打开**生成设置**窗口。

2. 确保在**生成中的场景**下的生成中包括了要测试的场景。 如果场景未列出，请打开场景，并选择**添加打开的场景**。

3. 通过选择**平台**下的 **Windows 应用商店**并单击**切换平台**，切换到 **Windows 应用商店**平台。

4. 在 **SDK** 旁边选择**通用 10**。

5. 在**调试**下，选中 **Unity C# 项目**。

6. 单击**生成**。

    ![生成设置](../images/unity/unity-build-settings.PNG)

7. 选择一个文件夹，在其中放入文件资源管理中的生成。

8. 在指定的文件夹中，依次打开 Visual Studio 中的 **&lt;ProjectName&gt;\\&lt;ProjectName&gt;.csproj**。

9. 在工具栏顶部选择 **x64** 或 **x86**（具体取决于设备支持的位数），并部署到**本地计算机**。

    ![调试; x64; 本地计算机](../images/unity/vs-debug-local-machine.PNG)

10. 如果你在项目中添加了登录到 Xbox Live 的方法，请使用拥有沙盒访问权限的帐户登录。 你现在已经将 Xbox Live 与主题作品连接！

## <a name="try-out-the-examples"></a>试用示例

你已经准备好开始使用 Unity 项目中的 Xbox Live 了！ 请尝试打开 **Xbox Live/示例**文件夹中的场景，以查看操作中的插件以及如何自行使用功能的示例。 在编辑器中运行示例会为你提供虚拟数据，但是，如果在 Visual Studio 中生成项目，并将 Microsoft 帐户与沙盒关联，你可以使用玩家代号登录。

尝试使用 **SignInAndProfile** 场景登录你的 Microsoft 帐户，使用**排行榜**场景创建排行榜，并使用 **UpdateStat** 场景显示和更新统计数据。

## <a name="see-also"></a>另请参阅

* [在 Unity 中登录 Xbox Live](sign-in-to-xbox-live-in-unity.md)
