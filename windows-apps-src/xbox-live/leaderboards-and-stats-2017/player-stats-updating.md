---
title: 更新 Stats 2017
description: 了解如何通过使用 Stats 2017 更新 Xbox Live 玩家统计数据。
ms.assetid: 019723e9-4c36-4059-9377-4a191c8b8775
ms.date: 08/24/2018
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 玩家统计数据, stats 2017
ms.localizationpriority: medium
ms.openlocfilehash: d5b37c008e6aa719b641321c5e5a1c3360b20786
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8794189"
---
# <a name="updating-stats-2017"></a>更新 Stats 2017

通过为 Xbox Live 服务发送最新值，你可以使用下面将要讨论的 `StatsManager` API 更新统计数据。

由你的作品来跟踪玩家统计数据，你可以调用 `StatsManager` 适当地更新这些统计数据。  `StatsManager` 将缓冲所做的任何更改，并将这些更改定期刷新到该服务。  你的作品还可以手动刷新。

> [!NOTE]
> 不要太频繁刷新统计数据。  否则，你的作品将受到速率限制。  最佳做法是最多每 5 分钟刷新一次。

### <a name="multiple-devices"></a>多台设备

玩家可在多台设备上玩你的作品。  在这种情况下，你需要做些努力，让内容保持同步。

例如，如果玩家在家里的 Xbox 上获得了 15 个头像。  后来，他们又在朋友家的 Xbox 上获得了 10 多个头像。  你需要将统计数据值 25 发送到第二台设备上。  但是，你无法得知这一情况，除非以某种方式同步此信息。

为此，你可以采用下面几种方法：

1. 使用[连接存储](../storage-platform/connected-storage/connected-storage-technical-overview.md)存储它们。  通常，你可以使用连接存储来存储每个用户的保存数据。  对于给定用户，此数据将在不同设备之间保持同步。
2. 如果你已经使用 Web 服务为你的作品执行辅助任务，请使用你自己的 Web 服务使统计数据保持同步。

### <a name="offline"></a>脱机

如上所述，你的作品负责跟踪玩家统计数据，因此负责支持脱机场景。 

### <a name="examples"></a>示例

我们将演示一个示例，将这些概念关联在一起。

赛车游戏中的常见统计数据是单圈时间。  通常，对这些统计数据，越短越好。因此，你应创建统计数据和关联的排行榜，其中单圈时间越短越好。  换言之，此排行榜应按降序排序。

你的作品应在用户的游戏会话期间跟踪其单圈时间。  仅当他们的单圈时间少于其以前最快时间时，你才需要更新统计数据管理器。

你可以采用以下方式之一跟踪以前最佳时间：
1. 使用连接存储从保存文件中执行此操作。
2. 使用你自己的 Web 服务。

无论如何，该服务将替换统计数据值。  因此，即使你要使用大于以前最快时间的单圈时间更新，也将覆盖其以前最快时间。

因此，请确保在你的作品中，你仅根据你的游戏玩法方案发送正确的统计数据值。  在某些情况下，值越低可能就越好；在另一些情况下，值越高可能就越好，或完全是其他情况。

## <a name="programming-guide"></a>编程指南

通常，统计数据的使用流程如下：

1. 通过传入本地用户初始化 `StatsManager` API。
1. 当用户在你的作品中玩游戏时，请使用 `set_stat` 函数更新统计数据值。
1. 这些统计数据更新将定期刷新并写入到 Xbox Live。  你还可以手动执行此操作。

### <a name="initialization"></a>初始化

你可以本地用户的身份调用 `StatsManager`，以使用必需的信息初始化 API。

有关示例，请参阅下文

```cpp
std::shared_ptr<stats_manager> statsManager = stats_manager::get_singleton_instance();
statsManager->add_local_user(user);
statsManager->do_work();  // returns stat_event_type::local_user_added
```

```csharp
Microsoft.Xbox.Services.Statistics.Manager.StatisticManager statManager = StatisticManager.SingletonInstance;
statManager.AddLocalUser(user);
statEvent = statManager.DoWork();
```

### <a name="writing-stats"></a>写入统计数据

你可以使用 `stats_manager::set_stat` 系列函数写入统计数据。  对于每种数据类型，此函数有三种变体：

* `set_stat_number` 浮点。
* `set_stat_integer` 整数。
* `set_stat_string` 字符串。

在调用这些函数时，统计数据更新将在设备上本地缓存。  这些更新将定期刷新到 Xbox Live。

你可以选择通过 `stats_manager::request_flush_to_service` API 手动刷新统计数据。  请注意，如果你太频繁地调用此函数，则你将受到速率限制。  这并不意味着将不更新统计数据。  这只意味着超时到期时将会进行更新。

```cpp
statsManager->set_stat_integer(user, L"numHeadshots", 20);
statsManager->request_flush_to_service(user); // requests flush to service, performs a do_work
statsManager->do_work();  // applies the stat changes, returns stat_update_complete after flush to service
```

```csharp
statManager.SetStatisticIntegerData(user, statName, (long)statValue);
statManager.RequestFlushToService(user);
statManager.DoWork();
```

#### <a name="example"></a>示例

假设你拥有第一人称射击游戏。  在匹配过程中，你可以累积以下统计数据：

| 统计数据名称 | 格式 |
|-----------|--------|
| 每局的最佳击杀数 | 整数 |
| 总击杀数 | 整数 |
| 总死亡数 | 整数 |
| 总击杀/死亡比 | 数字 |

在玩家进行比赛时，你要在本地增加*每局的击杀数*、*总击杀数*和*总死亡数*。

在比赛结束时，你要执行以下操作：
1. 将他们在本局中获得的击杀数与之前的最佳成绩做以比较。  如果数字更大，则更新 `StatsManager`。
2. 使用新值更新他们的“总击杀数”和“总死亡数”，并更新 `StatsManager`。
3. 计算击杀/死亡比并更新 `StatsManager`

请注意，对于 1 和 2 这两种情况，你需要了解其以前的统计数据值。  有关检索这些值的最佳实践，请参阅上面部分。

其中的任意统计数据可能对应于排行榜，而排行榜将在下一篇文章中进行探讨。

### <a name="flushing-stats"></a>刷新统计数据

你可以使用 `stats_manager::request_flush_to_service` 手动刷新统计数据。  如果你要显示排行榜，则可能需要执行此操作。

例如，如果你拥有上例中的`Lifetime Kills`的排行榜，你需要确保，与此统计数据对应的统计数据更新已刷新到服务器，然后再显示排行榜。  这样一来，排行榜便可反映玩家的最新进度。

### <a name="cleanup"></a>清除
在作品关闭后，从统计数据管理器中删除用户。 此时会将最新的统计数据值刷新到该服务。

```cpp
statsManager->remove_local_user(user);
statsManager->do_work();  // applies the stat changes, returns local_user_removed after flush to service
```

```csharp
statManager.RemoveLocalUser(user);
statManager.DoWork();
```
