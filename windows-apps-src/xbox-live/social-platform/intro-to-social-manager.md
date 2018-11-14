---
title: 社交管理器简介
author: aablackm
description: 了解跟踪在线好友的 Xbox Live 社交管理器 API。
ms.assetid: d4c6d5aa-e18c-4d59-91f8-63077116eda3
ms.author: aablackm
ms.date: 03/26/2018
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e4a9f244b3ce699e097b8de34556a21e423f8d5e
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6260520"
---
# <a name="introduction-to-social-manager"></a>社交管理器简介

## <a name="description"></a>描述

Xbox Live 提供了作品可用于各种场景的丰富社交图片。

在 Xbox Live API (XSAPI) 中使用社交 API 获取和维护社交图片的信息很复杂，让这些信息始终保持最新也可能很复杂。  如果未能正确处理，则可能由于过多地调用不必要的 Xbox Live 社交服务而导致性能问题、过时数据或受到限制。

社交管理器通过以下方法解决这一问题：

* 创建简单的 API 调用
* 在后台使用实时活动服务创建最新信息
* 开发人员可以同步调用社交管理器 API，不会为服务带来任何额外负担

社交管理器避免了处理多个 RTA 订阅和刷新用户数据的复杂性，让开发人员可以轻松获取他们创建有趣场景所需的最新图片。

若要一睹社交管理器内存和性能特征，请看一下[社交管理器内存和性能概述](social-manager-memory-and-performance-overview.md)

## <a name="features"></a>功能

社交管理器提供以下功能：

* 简化的社交 API
* 最新的社交图片
* 控制显示信息的详细级别
* 减少调用 Xbox Live 服务的次数
  * 这与数据获取的总体延迟减少直接相关
* 安全线程
* 有效保持最新数据

## <a name="core-concepts"></a>核心概念

**社交图片**：*社交图片*为设备上的本地用户创建。 这将创建一个让用户好友的所有信息保持最新的结构。

> [!NOTE]
> 在 Windows 上只能有一个本地用户

**Xbox 社交用户**：*Xbox 社交用户*是与来自组的用户关联的完整社交数据集

**Xbox 社交用户组**：组是用于诸如填充 UI 这样的操作的用户集合。 组有两种类型

* **筛选组**：筛选组获取本地（调用）用户的*社交图片*，并始终根据指定的筛选参数返回最新用户集
* **用户组**：用户组获取用户列表，并返回这些用户的视图（始终为最新）。 这些用户可能不在用户好友列表中。

为了使*社交用户组*保持最新，必须在每个帧上调用 `social_manager::do_work()` 函数。

## <a name="api-overview"></a>API 概述

你将最频繁地使用以下密钥类：

### <a name="social-manager"></a>社交管理器

* C++ API 类名称：social_manager
* WinRT(C#) API 类名称：[SocialManager](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.socialmanager?view=xboxlive-dotnet-2017.11.20171204.01)

这是可用于获取 **Xbox 社交用户组**（上述视图）的单一实例类。

社交管理器会将 Xbox 社交用户组保持最新，并可以按状态或用户关系筛选用户组。  例如，可能会创建包含所有在线并正在玩当前作品的用户好友的 Xbox 社交用户组。  这会在好友开始或停止玩作品时保持最新。

### <a name="xbox-social-user-group"></a>Xbox 社交用户组

* C++ API 类名称：xbox_social_user_group
* WinRT(C#) API 类名称：[XboxSocialUserGroup](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager.xboxsocialusergroup?view=xboxlive-dotnet-2017.11.20171204.01)

满足某些条件的用户组，如上所述。 Xbox 社交用户组显示它们是哪类组，在跟踪哪些用户或它们的筛选器设置，以及组所属的本地用户。

可以在 [Xbox Live API 参考](https://aka.ms/xboxliveuwpdocs)中找到完整的社交管理器 API 说明。
还可以在 [Microsoft.Xbox.Services.Social.Manager.Namespace 文档](https://docs.microsoft.com/en-us/dotnet/api/microsoft.xbox.services.social.manager?view=xboxlive-dotnet-2017.11.20171204.01)中找到 WinRT API

## <a name="usage"></a>使用

### <a name="creating-a-social-user-group-from-filters"></a>从筛选器创建社交用户组

在此场景中，你想从筛选器获取一个用户列表，如该用户关注或标记为常用的所有用户。

#### <a name="source-example-using-the-c-api"></a>使用 C++ API 的源示例

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>使用 C# API 的源示例

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}

```

#### <a name="events-returned"></a>返回的事件

`local_user_added`(C++) | `LocalUserAdded`(C#) - 在完成用户社交图片的加载时触发。 指示初始化期间是否出现任何错误

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#) - 在创建了社交用户组时触发

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#) - 在用户载入时触发

#### <a name="additional-details"></a>其他详细信息

上方的示例显示了如何为用户初始化社交管理器、为该用户创建社交用户组，并使其保持最新。

筛选选项可以在 `presence_filter` 和 `relationship_filter` 枚举中看到

在游戏循环中，`do_work` 函数使用该组中用户的最新快照更新所有创建的视图。

视图中的用户可以通过调用 `xbox_social_user_group::get_users()` 函数（返回 `xbox_social_user` 对象列表）获取。  `xbox_social_user` 包含社交信息，如玩家代号、玩家图片 uri 等。

### <a name="create-and-update-a-social-user-group-from-list"></a>从列表创建和更新社交用户组

在此场景中，你需要一组用户（如多人游戏会话中的用户）的社交信息。

#### <a name="source-example-using-the-c-api"></a>使用 C++ API 的源示例

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();

socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::preferred_color_level | social_manager_extra_detail_level::title_history_level
    );

auto socialUserGroup = socialManager->create_social_user_group_from_list(
    xboxLiveContext->user(),
    userList  // this is a std::vector<string_t> (list of xuids)
    );

while(true)
{
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
}
```

#### <a name="source-example-using-the-c-api"></a>使用 C# API 的源示例

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromList(
     xboxLiveContext.User,
     userList //this is a IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
    );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>返回的事件

`local_user_added`(C++) | `LocalUserAdded`(C#) - 在完成用户社交图片的加载时触发。 指示初始化期间是否出现任何错误

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#) - 在创建了社交用户组时触发

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#) - 在用户载入时触发

### <a name="updating-social-user-group-from-list"></a>从列表更新社交用户组

你还可以通过调用 update_social_user_group() 更改社交用户组中被跟踪用户的列表

#### <a name="source-example-using-the-c-api"></a>使用 C++ API 的源示例

```cpp
//#include "Social.h"

socialManager->update_social_user_group(
    xboxSocialUserGroup,
    newUserList    // std::vector<string_t> (list of xuids)
    );

    while(true)
    {
    // some update loop in the game
    socialManager->do_work();
    // TODO: render the friends list using game UI, passing in socialUserGroup->users()
    }
```

#### <a name="source-example-using-the-c-api"></a>使用 C# API 的源示例

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.UpdateSocialUserGroup(
     xboxSocialUserGroup,
     newUserList //IReadOnlyList<string> (list of xbox user ids a.k.a. xuids)
     );

while(true)
{
     // some update loop in the game
     socialManager.DoWork();
     // // TODO: render the friends list using game UI, passing in socialUserGroup.Users
}
```

#### <a name="events-returned"></a>返回的事件

`social_user_group_updated`(C++) | `SocialUserGroupUpdated`(C#) - 社交用户组更新完成时触发。

`users_added_to_social_graph` | `UsersAddedToSocialGraph`(C#) - 在用户载入时触发。 如果用户通过图片中已有的列表添加，将不会触发此事件

### <a name="using-social-manager-events"></a>使用社交管理器事件

社交管理器还会以事件形式告诉你所发生的事情。  你可以使用这些事件来更新你的 UI 或执行其他逻辑。

#### <a name="source-example-using-the-c-api"></a>使用 C++ API 的源示例

```cpp
//#include "Social.h"

auto socialManager = social_manager::get_singleton_instance();
socialManger->add_local_user(
    xboxLiveContext->user(),
    social_manager_extra_detail_level::no_extra_detail
    );

auto socialUserGroup = socialManager->create_social_user_group_from_filters(
    xboxLiveContext->user(),
    presence_filter::all,
    relationship_filter::friends
    );

socialManager->do_work();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    auto events = socialManager->do_work();
    for(auto evt : events)
    {
        auto affectedUsersFromGraph = socialUserGroup->get_users_from_xbox_user_ids(evt.users_affected());
        // TODO: render the changes to the friends list using game UI, passing in affectedUsersFromGraph
    }
}
```

##### <a name="source-example-using-the-c-api"></a>使用 C# API 的源示例

```csharp
// using Microsoft.Xbox.Services;
// using Microsoft.Xbox.Services.System;
// using Microsoft.Xbox.Services.Social.Manager;

socialManager = SocialManager.SingletonInstance;

socialManager.AddLocalUser(
     xboxLiveContext.User,
     SocialManagerExtraDetailLevel.PreferredColorLevel | SocialManagerExtraDetailLevel.TitleHistoryLevel
     );

socialUserGroup = socialManager.CreateSocialUserGroupFromFilters(
     xboxLiveContext.User,
     PresenceFilter.All,
     RelationshipFilter.Friends
     );

socialManager.DoWork();
// TODO: initialize the game UI containing the friends list using game UI, socialUserGroup->users()

while(true)
{
    // some update loop in the game
    IReadOnlyList<SocialEvent> Events = socialManager.DoWork();
    IReadOnlyList<XboxSocialUser> affectedUsersFromGraph;
    foreach(SocialEvent managerEvent in Events)
    {
        affectedUsersFromGraph = socialUserGroup.GetUsersFromXboxUserIds(managerEvent.UsersAffected);
    }
}

```

#### <a name="events-returned"></a>返回的事件

`local_user_added`(C++) | `LocalUserAdded`(C#) - 在完成用户社交图片的加载时触发。 指示初始化期间是否出现任何错误

`social_user_group_loaded`(C++) | `SocialUserGroupLoaded`(C#) - 在创建了社交用户组时触发

`users_added_to_social_graph`(C++) | `UsersAddedToSocialGraph`(C#) - 在用户载入时触发

#### <a name="additional-details"></a>其他详细信息

此示例显示了一些提供的其他控件。  与依靠社交用户组筛选器在游戏循环中提供新用户列表不同，社交图片在游戏循环外初始化。  然后标题依靠 `socialManager->do_work()` 函数返回的 `events`。  `events` 是一个 `social_event` 列表，每个 `social_event` 包含对最后一帧中出现的社交图片的更改。  例如，`profiles_changed`、`users_added` 等。详细信息可以在 `social_event` API 文档中找到。

### <a name="cleanup"></a>清除

#### <a name="cleaning-up-social-user-groups"></a>清除社交用户组

```cpp
//#include "Social.h"

socialManger->destroy_social_user_group(
    socialUserGroup
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.DestroySocialUserGroup(
     socialUserGroup
     );
```

清除创建的社交用户组。 调用方还应删除创建社交用户组时必须进行的任何引用，因为它现在包含过时数据。

#### <a name="cleaning-up-local-users"></a>清除本地用户

```cpp
//#include "Social.h"

socialManger->remove_local_user(
    xboxLiveContext->user()
    );
```

```csharp
// using Microsoft.Xbox.Services.Social.Manager;

socialManager.RemoveLocalUser(
     xboxLiveContext.User
     );
```

删除本地用户将删除加载的用户社交图片，以及使用该用户所创建的任何社交用户组。

#### <a name="events-returned"></a>返回的事件

`local_user_removed`(C++) | `LocalUserRemoved`(C#) - 成功删除本地用户时触发