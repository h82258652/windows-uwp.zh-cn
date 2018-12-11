---
title: Xbox Live SDK 的新增功能 - 2016 年 11 月
description: Xbox Live SDK 的新增功能 - 2016 年 11 月
ms.assetid: 5cf9ba9d-5a15-4e62-bc1f-45ff8b8bf3b0
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: de716ea88e5225789f93a7ff770d7b8dd2f8bd94
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8892436"
---
# <a name="whats-new-for-the-xbox-live-sdk---november-2016"></a>Xbox Live SDK 的新增功能 - 2016 年 11 月

请参阅[新增功能 - 2016 年 8 月](1608-whats-new.md)文章了解 2016 年 8 月版本中增加了哪些功能。

## <a name="xbox-services-api"></a>Xbox 服务 API

### <a name="simplified-achievements"></a>简化成就

* [简化成就](../achievements-2017/simplified-achievements.md)是一个配置和使用成就的更简单的新方法。  你不再需要发送事件，让 Xbox Live 服务来决定是否解锁成就。  你的标题现在负责这个决定。
* 如果你已加入了“简化成就”的早期试用，我们还增加了脱机支持。
* 名为 SimplifiedAchievements 的新示例展示了它有多易于使用。

### <a name="multiplayer"></a>多人游戏

* [会话浏览](../multiplayer/session-browse.md)是为你的用户提供的查找多人游戏的新方法。  “会话浏览”可让玩家搜索满足指定条件的一组开放的多人游戏会话。
* [多人游戏管理器](../multiplayer/multiplayer-manager.md)现在具有自动填充功能。  如果启用，“多人游戏管理器”将在游戏期间通过比赛查找成员来填补空位。
* [XIM（Xbox 集成多人游戏）](../multiplayer/xbox-integrated-multiplayer.md)的预生产版本现已可用于 XDK 开发。  XIM 是一个自包含界面，可通过 Xbox Live 服务功能向你的游戏轻松添加多人游戏实时网络和聊天通信。

### <a name="social-manager"></a>社交管理器

* 大量[社交管理器](../social-platform/intro-to-social-manager.md) API 更改：
    * 社交管理器已经离开了实验命名空间。 特别感谢那些成为早期采用者并提供反馈的用户！
    * `xbox_social_user`
        * `string_t` 对象已更改为 `char*` 对象
        * 每个自定义状态记录有六个 `social_manager_presence_title_record` 限制 `social_manager_presence_record`
    * `social_event`
        * 改为返回 `std::vector<xbox_user_id_container>` `std::vector<xbox_social_user>`
        * 此矢量可以传入新 API， `xbox_social_user_group::get_users_from_xbox_user_ids()`
    * `xbox_social_user_group`
        * `users()` API 现在返回 `std::vector<xbox_social_user*>`。 这些指针在下一次调用中失效， `social_manager::do_work()`
        * `get_copy_of_users` 以在 social_user_group 中将 `std::vector<xbox_social_user>` 作为当前用户的副本返回给呼叫方。 调用此函数可能会影响 `do_work` 的完成时间。
* “社交管理器”现在永远不会在初始化后失败。 “社交管理器”将在断开连接时自动重试 RTA。 `error` 和 `rta_disconnect_error` 事件已弃用并删除
* 标题可以指定自定义内存分配器。 请参阅新文档：[社交管理器内存和性能](../social-platform/social-manager-memory-and-performance-overview.md)

### <a name="xbox-one-uwp"></a>Xbox One UWP
* TCUI API 为 Xbox One UWP 应用增加了多用户支持。  XSAPI C++ 增加了用于指定用户的新 Windows::System::User^ 参数，XSAPI WinRT API 增加了 ForUserAsync API。
* 更新了 UWP 示例以在 Xbox 上支持多个用户

### <a name="other"></a>其他

* 增加了 C++/WinRT 支持。   更多详细信息可以在[此处](../introduction-to-xbox-live-apis.md)找到
* 用于添加和删除你自己的日志记录回调的新功能。  诊断级别将被传递给你的回调，以便你可以微调行为。  请参见 `microsoft::xbox::services::system::xbox_live_services_settings` 命名空间中的 `add_logging_handler` 和 `remove_logging_handler`

## <a name="documentation"></a>文档
* [多人游戏管理器](../multiplayer/multiplayer-manager.md)、[XIM](../multiplayer/xbox-integrated-multiplayer.md) 和 Xbox Live 的[多人游戏概念](../multiplayer/multiplayer-concepts.md)提供了新文档。
* [Xbox Live 简介](../get-started-with-partner/get-started-with-xbox-live-partner.md)部分已经重写。  如果你要创建启用了 Xbox Live 的新游戏，或好奇如何在你的游戏中集成其他 Xbox Live 功能，你可以参阅[此处](../get-started-with-partner/get-started-with-xbox-live-partner.md)的新文档。

## <a name="tools"></a>工具
* Xbox Live 跟踪分析器，你可以找到在[http://aka.ms/XboxLiveTools](http://aka.ms/XboxLiveTools)现在具有 CERT 模式。  
* 同样，Xbox Live 跟踪分析器具有多主机支持。  如果你传入包含多个主机的 HTTP 请求的 Fiddler 跟踪，将为每个请求生成单独的报告。
