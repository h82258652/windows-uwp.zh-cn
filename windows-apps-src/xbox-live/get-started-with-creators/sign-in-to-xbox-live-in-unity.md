---
title: "在 Unity 中登录 Xbox Live"
author: KevinAsgari
description: "了解如何在 Unity 游戏中使用 Xbox Live 插件 prefab 登录到 Xbox Live 帐户。"
ms.assetid: f5402d4c-894e-4879-969a-7e68699546c5
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, 创意者, 登录"
ms.openlocfilehash: 8cfb9c7693c87fbd6df1766f9e16a1049f2c9320
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2017
---
# <a name="sign-in-to-xbox-live-in-unity"></a>在 Unity 中登录 Xbox Live

> **注意：**只建议 [Xbox Live 创意者计划](../developer-program-overview.md)成员使用 Xbox Live Unity 插件，因为当前不支持成就或多人游戏。

使用 Xbox Live Unity 插件，你可以在 Unity 项目中轻松登录到 Xbox Live。 你可以使用包含的 prefab，也可以将包含的脚本附加到你的自定义对象中。

> **注意：**此主题假设你已在 Unity 项目中设置了 Xbox Live 插件。 有关如何执行此操作的信息，请参阅[在 Unity 中配置 Xbox Live](configure-xbox-live-in-unity.md)。

## <a name="using-the-prefab"></a>使用 prefab

**UserProfile** prefab 是最重要的 Xbox Live prefab，位于 **Xbox Live\Prefabs** 中。 此 prefab 允许用户登录到 Xbox Live，并且在用户登录后，它将显示其玩家代号、玩家图片和玩家分数。 通常你将在初始菜单屏幕上显示此 prefab，或者在游戏启动时自动触发它。 为了使用任何其他 Xbox Live prefab，你必须包含一个 UserProfile prefab 或手动调用登录 API。 有关如何执行此操作的详细信息，请参阅 **UserProfile.cs** 脚本和以下部分。

只需将 prefab 拖动到场景中，然后所有内容会设置完毕。

![&lt;玩家代号&gt;&lt;玩家分数&gt;](../images/unity/unity-userprofile-prefab.PNG)

若要使用任何 Xbox Live prefab，你至少需要将一个 **XboxLiveServices** prefab 的实例拖动到初始场景中。 在该 prefab 中，你可以启用和关闭来自 unity 程序包的各个 prefab 的日志，以便进行调试。

如果你进入播放模式，则该按钮将更改为显示**登录**。

![登录](../images/unity/unity-sign-in.PNG)

如果你单击该按钮，它将显示用户的玩家代号、玩家图片，以及玩家分数。 在编辑器中，这将是占位符数据。

![虚构用户 123456789](../images/unity/unity-game-fake-data.PNG)

你需要生成项目并在 Visual Studio 中运行它以使用真正的 Xbox Live 帐户登录。 有关详细信息，请参阅[在 Unity 中配置 Xbox Live](configure-xbox-live-in-unity.md)。

## <a name="using-the-scripts"></a>使用脚本

Prefab 用于登录到 Xbox Live 的脚本是 **Xbox Live\Scripts\UserProfile.cs**。 这里要注意的主要方法是 `SignInAsync`，它将调用 `XboxLive.Instance.SignInAsync`。

Unity 中的大多数 Xbox Live 功能由 `XboxLive` 脚本 (**Xbox Live\Scripts\XboxLive.cs**) 管理。  当你使用任何 Xbox Live 功能时，会在场景中自动实例化此对象，并将其标记为 `DontDestroyOnLoad`，以便其在游戏的整个持续时间内生效。

任何需要调用 Xbox Live API 的脚本都应使用 `XboxLive.Instance` 的各种属性。

* `Context`  提供了众多 Xbox Live 服务的主要入口点，并在用户使用 `SignInAsync` 进行身份验证后进行初始化。  有关详细信息，请参阅 [Xbox Live API 文档](http://github.com/Microsoft/xbox-live-api-csharp)。

* `User`  提供对当前通过身份验证的用户的引用，可以在调用各种服务时使用它。

用户登录后，你便可获取他们的相关信息。 你可以查看 `UserProfile` 的 `LoadProfileInfo` 方法了解脚本是如何获取用户的 ID、玩家图片和其他信息的。

## <a name="see-also"></a>另请参阅

* [在 Unity 中配置 Xbox Live](configure-xbox-live-in-unity.md)
