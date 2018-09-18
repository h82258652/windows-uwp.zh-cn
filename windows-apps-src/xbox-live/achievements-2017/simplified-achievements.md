---
title: Achievements 2017
author: KevinAsgari
description: Achievements 2017
ms.assetid: d424db04-328d-470c-81d3-5d4b82cb792f
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 04d2fab9aa836d36a0dba202b2292c311b6d4979
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "4018004"
---
# <a name="achievements-2017"></a>Achievements 2017

Achievements 2017 系统支持游戏开发人员使用直接调用模型来解锁 Xbox One、Windows 10、Windows 10 Phone、Android 和 iOS 上的新 Xbox Live 游戏成就。

## <a name="introduction"></a>简介

在 Xbox One 上，我们引入了基于云的新成就系统，它使游戏开发人员只需发送游戏内遥测事件即可驱动其 Xbox Live 功能数据，如用户统计数据、成就、完整状态和多人游戏。 这带来了大量新的好处 – 通过单个事件即可更新多个 Xbox Live 功能的数据；Xbox Live 配置依赖于服务器而不是客户端，等等。

自 Xbox One 发布以来，我们密切听取了游戏开发人员的反馈，开发人员一致反应以下情况：

1.  **希望能够通过直接调用模式解锁成就。** 许多开发人员在不同的平台上构建游戏，包括以前版本的 Xbox，这些平台上的成就类系统使用的是直接调用方法。 在 Xbox One 和其他最新一代 Xbox 平台上支持直接解锁调用可以缓解跨平台游戏开发需求并降低开发时间成本。

2.  **最小化配置复杂性。** 在支持云的成就系统上，必须在 Xbox Live 中配置成就的解锁逻辑，使服务知道如何解释游戏的统计数据以及何时为用户解锁成就。 这通过成就配置中的成就规则部分（先前没有）完成。 尽管在云中拥有解锁逻辑非常强大，但这一额外配置要求为游戏成就的设计和开发增加了复杂性。

3.  **难以进行疑难解答。** 尽管支持云的成就系统进入了许多有用功能，但游戏开发人员也难以验证和解决与成就相关的问题，因为成就解锁是由服务上的规则间接触发，而不是由游戏本身直接控制。

值得注意的是，游戏开发人员也多次反应了他们欣赏和喜欢支持云的成就系统所带来的以下功能：

1.  新的用户体验功能，如成就进度、实时更新、概念艺术作品奖励以及将解锁插入活动源中。

2.  配置改进，例如服务配置而不是必须包括在游戏包中的本地配置（即 gameconfig、XLAST、SPA 等），以及在游戏发行后可以轻松编辑成就字符串和图像的功能。

在 Achievements 2017 中，我们开发了现有支持云的成就系统的替代系统，供未来的游戏使用，这使得 Xbox 游戏开发人员可以更轻松地配置成就、将成就解锁和更新集成到游戏代码中以及验证成就是否按照预期工作。

## <a name="whats-different-with-achievements-2017"></a>与 Achievements 2017 的不同之处

|                          | Achievements 2017 系统        | 支持云的成就系统      |
|--------------------------|---------------------------------------|----------------------------------------|
| 解锁触发           | 通过 API 调用直接触发                 | 通过遥测事件间接触发        |
| 解锁所有者             | 游戏                                 | Xbox Live                              |
| 配置            | 字符串、图像、奖励              | 字符串、图像、奖励、解锁规则 \[+ 统计数据，+ 事件\]                    |
| 进度              | 受支持 <br>*通过 API 调用直接支持*                | 受支持 <br> *通过遥测事件间接支持*       |
| 实时活动 (RTA) | 受支持                             | 受支持                              |
| 挑战               | 不支持   | 受支持                      |

## <a name="title-requirements"></a>游戏要求

以下是对使用 Achievements 2017 系统的任何游戏的要求。

1.  **必须为新（未发布的）游戏。** 已发布并且使用支持云的成就系统的游戏不符合条件。 有关详细信息，请参阅[为什么不能将现有游戏“迁移”至新的 Achievements 2017 系统？](#_Why_can’t_existing)

2.  **必须使用 2016 年 8 月的 XDK 或更高版本。** Update_Achievement API 已在 2016 年 8 月的 XDK 中发布。

3.  **必须为 XDK 或 UWP 游戏。** Achievements 2017 系统不可用于传统平台，包括 Xbox 360、Windows 8.x 或更早版本或者 Windows Phone 8 或更早版本。

## <a name="updateachievement-api"></a>Update_Achievement API

通过 XDP 或 [UDC](../configure-xbl/dev-center/achievements-in-udc.md) 配置完成就并将其发布到开发沙盒后，你的游戏即可通过调用 Update_Achievement API 解锁成就。

此 API 在 XDK 和 Xbox Live SDK 中均有提供。

### <a name="api-signature"></a>API 签名

API 签名如下所示：

```c++
/// <summary>
    /// Allow achievement progress to be updated and achievements to be unlocked.  
    /// This API will work even when offline. On PC and Xbox One, updates will be posted by the system when connection is re-established even if the title isn't running
    /// </summary>
    /// <param name="xboxUserId">The Xbox User ID of the player.</param>
    /// <param name="titleId">The title ID.</param>
    /// <param name="serviceConfigurationId">The service configuration ID (SCID) for the title.</param>
    /// <param name="achievementId">The achievement ID as defined by XDP or Dev Center.</param>
    /// <param name="percentComplete">The completion percentage of the achievement to indicate progress.
    /// Valid values are from 1 to 100. Set to 100 to unlock the achievement.  
    /// Progress will be set by the server to the highest value sent</param>
    /// <remarks>
    /// Returns a task<T> object that represents the state of the asynchronous operation.
    ///
    /// This method calls V2 GET /users/xuid({xuid})/achievements/{scid}/update
    /// </remarks>
    _XSAPIIMP pplx::task<xbox::services::xbox_live_result<void>> update_achievement(
        _In_ const string_t& xboxUserId,
        _In_ uint32_t titleId,
        _In_ const string_t& serviceConfigurationId,
        _In_ const string_t& achievementId,
        _In_ uint32_t percentComplete
        );
```

`xbox::services::xbox_live_result<T>` 为所有 C++ Xbox Live 服务 API 调用的返回调用。

有关详细信息，请参阅 Xfest 2015 访谈，“XSAPI：C++，无异常！”<br>
[视频](http://go.microsoft.com/?linkid=9888207) |  [幻灯片](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest_2015/Xbox_Live_Track/XSAPI_Cpp_No_Exceptions.pptx)

### <a name="unlocking-via-updateachievement-api"></a>通过 Update_Achievement API 解锁

若要解锁成就，请将 *percentComplete* 设为 100。

如果用户处于联机状态，则请求将会立即发送至 Xbox Live 成就服务并触发以下用户体验：

-   用户将会收到一则成就解锁通知；

-   指定的成就将在用户的游戏成就列表中显示为已解锁；

-   解锁的成就将添加到用户的活动源中。

> *注意：使用 Achievements 2017 系统和使用支持云的成就系统的用户体验没有明显差别。*

如果用户处于脱机状态，则解锁请求将在用户设备上进行本地排队。 在用户设备重新建立网络连接后，请求将会自动发送至成就服务 – 注意：游戏无需任何操作即可触发此事件 – 并且将会按照所述发生以上用户体验。

### <a name="updating-completion-progress-via-updateachievement-api"></a>通过 Update_Achievement API 更新完成进度

若要更新用户距离解锁成就的进度，请将 *percentComplete* 设为 1-100 之间的相应整数。

成就进度只能增加。 如果将 *percentComplete* 设为小于成就的最后 *percentComplete* 值的数字，则更新将被忽略。 例如，如果先前已将成就的 *percentComplete* 设为 75，则发送值为 25 的更新将被忽略并且成就仍将显示为 75% 完整。

如果将 *percentComplete* 设为 100，则成就将解锁。

如果将 *percentComplete* 设为大于 100 的数字，则 API 将按照设为 100 一样运行。

## <a name="frequently-asked-questions"></a>常见问题

### <a name="span-idwhyarechallenges-classanchorspancan-i-ship-my-title-using-the-achievements-2017-system-yet"></a><span id="_Why_are_Challenges" class="anchor"></span>我还可以使用 Achievements 2017 系统交付游戏吗？

没问题！ 欢迎并鼓励所有新游戏使用 Achievements 2017 系统来代替支持云的成就系统。

### <a name="why-are-challenges-not-supported-in-the-achievements-2017-system"></a>为什么在 Achievements 2017 系统中不支持挑战？

Xbox 游戏中的使用数据已显示当前的挑战实施和演示不符合大部分游戏开发人员的需求。 我们将继续收集开发人员在此空间的输入和反馈，并尝试在未来提供更符合开发人员需求的功能。 新发布的 Xbox Arena 是一项示例功能，用于以全新、简单的方向为 Xbox 游戏引入一些具有竞争力的新功能。

### <a name="can-i-still-add-new-achievements-every-calendar-quarter-if-my-title-is-using-the-achievements-2017-system"></a>如果我的游戏使用的是 Achievements 2017 系统，那么我是否仍然可以在每个日历季度添加新的成就？

可以。 成就策略并未改变。

### <a name="span-idwhycantexisting-classanchorspanwhy-cant-existing-titles-migrate-onto-the-new-achievements-2017-system"></a><span id="_Why_can’t_existing" class="anchor"></span>为什么不能将现有游戏“迁移”至新的 Achievements 2017 系统？

对于绝大多数现有游戏，“迁移”至 Achievements 2017 系统并不限于简单地更新其服务配置并将每次写入操作交换为成就解锁调用，但这些更改非常昂贵并且将会带来重大错误风险和异常行为，可能为成就带来无法修复的破坏。 更确切地说，大部分现有游戏也让用户拥有现有数据。 尝试转换已使用支持云的成就系统的实时游戏不仅对于开发人员和 Xbox 来说代价昂贵，而且还会严重危及现有用户的个人资料和/或游戏体验。

### <a name="if-my-title-was-released-using-the-cloud-powered-achievements-system-can-any-future-dlc-for-the-title-switch-to-achievements-2017"></a>如果发布的游戏使用的是支持云的成就系统，该游戏的任何未来 DLC 是否能够切换至 Achievements 2017？

游戏的所有成就必须使用相同的成就系统。 基本游戏成就使用的成就系统即为该游戏的所有未来成就必须使用的系统。

### <a name="while-testing-achievements-in-my-dev-sandbox-can-i-mix-and-match-between-using-the-achievements-2017-system-and-the-cloud-powered-achievements-system"></a>在我的开发人员沙盒中测试成就时，是否可以混合搭配使用 Achievements 2017 系统和支持云的成就系统？

不可以。 游戏的所有成就必须使用相同的成就系统。

### <a name="does-achievements-2017-also-include-offline-unlocks"></a>Achievements 2017 是否也包括脱机解锁？

如果游戏在设备处于脱机状态时解锁成就，则 Update_Achievement API 会将脱机解锁请求自动排队并在设备重新建立网络连接时自动同步至 Xbox Live，类似于当前支持云的成就系统的脱机体验。 用户处于脱机状态时不会发生成就解锁。

### <a name="i-see-a-new-achievementupdate-event-in-xdp-if-my-title-uses-that-event-does-that-mean-it-has-achievements-2017"></a>我在 XDP 中看到新的“AchievementUpdate”事件。 如果我的游戏使用此事件，这是否意味着它采用了 Achievements 2017？

出于后端目的，Xbox Live 需要使用 *AchievementUpdate* 基本事件。 你可以安全地忽略它。 如果你的游戏使用此基本事件类型配置事件，则 Xbox Live 将会忽略这些事件写入操作。 在支持云的成就系统上构建的游戏应该会继续使用其他基本事件类型配置其事件。 在 Achievements 2017 系统上构建的游戏无需为成就配置*任何*事件。
