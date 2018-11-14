---
title: Xbox Live SDK 的新增功能 - 2016 年 8 月
author: KevinAsgari
description: Xbox Live SDK 的新增功能 - 2016 年 8 月
ms.assetid: fa52e7bd-2c2c-4c25-94ab-761036a7ca79
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fd78e2ff061f8cf1268a340ad63d954993f563fc
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6254031"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2016"></a>Xbox Live SDK 的新增功能 - 2016 年 8 月

请参阅[新增功能 - 2016 年 6 月](1606-whats-new.md)文章了解 2016 年 6 月版本中增加了哪些功能。

## <a name="os-and-tool-support"></a>操作系统和工具支持
Xbox Live SDK 支持 Windows 10 RTM [版本 10.0.10240] 和 Visual Studio 2015 RTM [版本 14.0.23107.0]。

## <a name="documentation"></a>文档
- 如果你在编写 UWP 应用程序，并且要实现向游戏邀请用户的功能，需要了解一下[为多人游戏配置 AppXManifest](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md) 中必须进行的 ```.appxmanifest``` 更改的相关说明。  这之前在[论坛](https://forums.xboxlive.com)以及[将 xbox live 代码从 era 移植到 uwp](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) 一文中进行了讨论
- [社交管理器简介](../social-platform/intro-to-social-manager.md)一文已经更新，以反映最新的 API 更改，并提供了一些函数的返回代码的更多信息。

## <a name="unity-samples"></a>Unity 示例
针对编写 UWP 应用程序的 Unity 开发人员添加了一些新示例。
- 现在有了社交示例的 Unity 版本，你可以在“示例/社交/Unity”目录下找到此示例。
- 另外还提供了介绍如何使用连接存储的示例。  请参见“示例/GameSave/Unity”了解该示例。
在“示例/AchievementsLeaderboard/Unity”下提供了 AchievementsLeaderboard 的 Unity 版本

## <a name="social-manager"></a>社交管理器
除了上述提到的文档更新外，还添加了一些新 API。  这些内容在下方进行了介绍，你可以在 social_manager.h 上看到更多详细信息

- 新增的允许更新（无需重新创建）社交组的新 API：

```cpp
    _XSAPIIMP xbox_live_result<void> update_social_user_group(
        _In_ const std::shared_ptr<xbox_social_user_group>& group,
        _In_ const std::vector<string_t>& users
        );
```
- 完成的社交组更新由 ```social_user_group_updated``` 事件指示


## <a name="multiplayer"></a>多人游戏
改进会话浏览现已推出，你可以使用新的多人游戏 API 来利用它。

使用新 API，你可以对标记、字符串和其他丰富的数据进行筛选，以便用户可以更轻松地找到他们想要播放的会话。

我们将在接下来的几个月内发布更全面的文档，不过你现在可以暂时使用 ```set_search_handle``` 来将“搜索句柄”与 MPSD 会话关联，然后用户便可以通过标题调用使用可靠的筛选机制搜索会话 ```get_search_handles```

新 API 已在下面列出。  请试用一下，如果你遇到任何问题，请在[论坛](https://forums.xboxlive.com)中发布支持线程或联系你的 DAM。  我们很快将提供如何使用这些 API 的示例。

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> set_search_handle(
    _In_ multiplayer_search_handle_request searchHandleRequest
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<std::vector<multiplayer_search_handle_details>>> get_search_handles(
    _In_ const string_t& serviceConfigurationId,
    _In_ const string_t& sessionTemplateName,
    _In_ const string_t& orderBy,
    _In_ bool orderAscending,
    _In_ const string_t& searchQuery
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> clear_search_handle(_In_ const string_t& handleId);
```

### <a name="xbox-integrated-multiplayer"></a>Xbox 集成多人游戏

我们已包括了 Xbox 集成多人游戏 (XIM) API 的文档。  API 本身将在 Xbox Live SDK 的后续版本中提供，但文档和标头即将提供预览。

XIM 是一个自包含界面，可通过 Xbox Live 服务功能向你的游戏轻松添加多人游戏实时网络和聊天通信。

我们在此处共享 API 文档预览是为了鼓励客户提供反馈、询问问题。 我们谈到之前在 Xfest 2016 中，此 API，你可以看到存档的[托管的合作伙伴开发人员站点上的演示材料](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2016.aspx)从"打开密钥多人游戏网络和聊天"交谈。 请注意，此预览文档仅用于 C++ API。 C# 和其他语言的 WinRT 等效项将在今年晚些时候发布。

如果你对 XIM 的功能感兴趣，或希望提供有关此项目的反馈或有其他相关问题，请随时在 [Xbox 开发人员论坛](https://forums.xboxlive.com/)上发帖子，或通过你的开发者帐户管理器联系我们。

你可以在 Xbox Live SDK 的文档目录下的 xbox_integrated_multiplayer.chm 中看到这个新文档。  包含的文件在 \include\xim\XboxIntegratedMultiplayer.h 中以预览形式提供。  
