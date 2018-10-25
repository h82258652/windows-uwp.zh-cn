---
title: 向 Unity 游戏添加多用户支持
author: KevinAsgari
description: 使用 Xbox Live Unity 插件向 Unity 游戏添加多用户支持
ms.assetid: ''
ms.author: heba
ms.date: 07/14/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, unity, 多用户
ms.localizationpriority: medium
ms.openlocfilehash: cc59d014bae8553cbbc9a4aca1db954872d28029
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5472525"
---
# <a name="add-multi-user-support-to-your-unity-game"></a>向 Unity 游戏添加多用户支持
> [!IMPORTANT]
> Xbox Live Unity 插件不支持成就或多人在线游戏，只建议 [Xbox Live 创意者计划](../developer-program-overview.md)成员使用。

对于涉及多个本地玩家的场景，Xbox Live Unity 插件现在支持多用户支持。 每个玩家都可以使用自己的 Xbox Live 帐户进行统计数据和登录。 你的游戏还可以为没有 Xbox Live 帐户的玩家启用来宾模式。 仅在 Xbox 主机上支持此功能。

此教程将介绍如何向不同的 Xbox Live Prefabs 添加多用户支持。

> [!IMPORTANT]
> 仅在 Xbox 主机上支持多用户场景。 此功能在电脑上不起作用。

## <a name="prerequisites"></a>先决条件
你需要遵循[入门](configure-xbox-live-in-unity.md)教程才能启用和配置 Xbox Live。

## <a name="adding-and-signing-in-multiple-xbox-live-users"></a>添加和登录多个 Xbox Live 用户

1. 从**资源** > **Xbox Live** > **Prefabs** 文件夹中，按玩家数量将 **XboxLiveUser** prefab 实例拖动到场景中。 对于此教程而言，因为有两个玩家，故应向场景中添加两个 **XboxLiveUser** 实例。 为方便起见，我们将其称为 **XboxLiveUser** 和 **XboxLiveUser2**。

2. 要正确添加用户并登录，请向场景添加两个 **UserProfile** prefab 实例，每个 **XboxLiveUser** 添加一个。 确保在场景中有 `XboxLiveServices` prefab 实例。 另外，确保将两个 **UserProfile** 对象分开，以便区分。 由于这些 prefab 使用 Unity Eventsystem，请确保你在场景中有 `EventSystem` 实例。

    ![Xbox Live Unity 插件教程项目中多用户支持的层次结构](../images/unity/MUA-Tutorial-Hierarchy.png)

    ![Xbox Live Unity 插件教程项目中多用户支持的游戏场景](../images/unity/MUA-Tutorial-GameScene.png)

3. 向每个 **UserProfile** 对象分配一个场景上拥有的 **XboxLiveUser** prefab 的实例。

    ![多用户支持的 UserProfile prefab](../images/unity/user-profile-for-mua.png)

4. 由于多用户应用程序仅在 Xbox One 设备上受支持，需要向 **UserProfile** 对象添加控制器支持。 在每个 **UserProfile** 对象上，有称为 `InputControllerButton` 的字段，你可以在其中指定每个 **UserProfile** 应收听的游戏杆和按钮数。

对于此教程，我们将对向其分配玩家 1 的 **UserProfile** 使用 `joystick 1 button 0`，对玩家 2 和第二个 **UserProfile** 游戏对象使用 `joystick 2 button 0`。 这会分配 `A` 按钮以使两个控制器与 **UserProfile** 进行交互。

> [!Note]
> 有关 Xbox Live Unity 插件中的 Xbox 控制器支持的详细信息，请查看：[向 Xbox Live Prefab 添加控制器支持](add-controller-support-to-xbox-live-prefabs.md)

5. 在编辑器中运行场景并在每个按钮上点击“运行”，确保正确配置了所有内容。

    ![在 Unity 编辑器中测试多用户支持](../images/unity/run-example-mua.png)

## <a name="building-and-testing-the-uwp"></a>生成和测试 UWP

1. 遵循[使用 Unity 开发创意者主题作品](configure-xbox-live-in-unity.md)教程中概述的步骤后，在 Visual Studio 中打开导出的 UWP 解决方案。

2. 在游戏的 UWP 项目中，右键单击 `package.appxmanifest.xml` 文件并选择**查看代码**。

3. 在 `<Properties></Properties>` 部分下，添加以下内容可启用应用的多用户输入：`<uap:SupportedUsers>multiple</uap:SupportedUsers>`

4. 要在 Xbox 上测试，请遵循[在 Xbox 开发环境上设置你的 UWP](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/development-environment-setup) 教程中的文档。

## <a name="using-the-other-xbox-live-prefabs-with-multiple-users"></a>通过多用户使用其他 Xbox Live Prefab

在插件的**示例**文件夹中有 **MultiUserExample** 场景，可演示不同的 prefab 如何使用 **XboxLiveUser** 实例显示每个用户的特定信息。
