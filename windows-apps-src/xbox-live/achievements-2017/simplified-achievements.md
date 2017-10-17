---
title: Achievements 2017
author: KevinAsgari
description: Achievements 2017
ms.assetid: d424db04-328d-470c-81d3-5d4b82cb792f
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, games, uwp, windows 10, xbox one
ms.openlocfilehash: dca2603dacd827127cb4bba7f485d277ea682560
ms.sourcegitcommit: b73a57142b9847b09ebb00e81396f2655bbc26ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2017
---
# <a name="achievements-2017"></a>Achievements 2017

The Achievements 2017 system enables game developers to use a direct calling model to unlock achievements for new Xbox Live games on Xbox One, Windows 10, Windows 10 Phone, Android, and iOS.

## <a name="introduction"></a>Introduction

With Xbox One, we introduced a new Cloud-Powered Achievements system that empowers game developers to drive the data for their Xbox Live capabilities, such as user stats, achievements, rich presence, and multiplayer, by simply sending in-game telemetry events. This has opened up a multitude of new benefits – a single event can update data for multiple Xbox Live features; Xbox Live configuration lives on the server instead of in the client; and much more.

In the years following the Xbox One launch, we have listened closely to game developer feedback, and developers have consistently shared the following:

1.  **Desire to unlock achievements via a direct calling pattern.** Many developers build games for various platforms, including previous versions of Xbox, and the achievement-like systems on those platforms use a direct calling method. Supporting direct unlock calls on Xbox One and other current-gen Xbox platforms would ease their cross-platform game development needs and development time costs.

2.  **Minimize configuration complexity.** With the Cloud-Powered Achievements system, an achievement’s unlock logic must be configured in Xbox Live so that the services know how to interpret the title’s stats data and when to unlock the achievement for a user. This was done via a new Achievement Rules section of an achievement’s configuration that did not previously exist. While having unlock logic in the cloud can be quite powerful, this additional configuration requirement adds complexity into the design & creation of a title’s achievements.

3.  **Difficult to troubleshoot.** While the Cloud-Powered Achievements system introduces a variety of helpful capabilities, it can also be more difficult for game developers to validate and troubleshoot issues with their achievements since achievement unlocks are triggered indirectly by rules that live on the service rather than directly controlled by the game itself.

It is worth noting that game developers have also repeatedly shared feedback that they appreciate and value certain features that were introduced along with the Cloud-Powered Achievements system:

1.  New user experience features such as achievement progression, real-time updates, concept art rewards, and posting unlocks into activity feed.

2.  Configuration improvements such as a service config instead of a local config that must be included in the game package (i.e. gameconfig, XLAST, SPA, etc.) and the ability to easily edit achievement strings & images after the game has shipped.

With Achievements 2017, we are building a replacement of the existing Cloud-Powered Achievements system for future titles to use that makes it even easier for Xbox game developers to configure achievements, integrate achievement unlocks & updates into the game code, and validate that the achievements are working as expected.

## <a name="whats-different-with-achievements-2017"></a>What’s Different with Achievements 2017

|                          | Achievements 2017 system        | Cloud-Powered Achievements system      |
|--------------------------|---------------------------------------|----------------------------------------|
| Unlock Trigger           | Directly via API call                 | Indirectly via telemetry events        |
| Unlock Owner             | Title                                 | Xbox Live                              |
| Configuration            | Strings, images, rewards              | Strings, images, rewards, unlock rules  \[+ stats, +events\]                    |
| Progression              | Supported <br>*directly via API call*                | Supported <br> *indirectly via telemetry events*       |
| Real-Time Activity (RTA) | Supported                             | Supported                              |
| Challenges               | Not Supported   | Supported                      |

## <a name="title-requirements"></a>游戏要求

The following are the requirements of any title that will use the Achievements 2017 system.

1.  **Must be a new (unreleased) title.** Titles that have already been released and are using the Cloud-Powered Achievements system are ineligible. For more, see [Why can’t existing titles “migrate” onto the new Achievements 2017 system?](#_Why_can’t_existing)

2.  **必须使用 2016 年 8 月的 XDK 或更高版本。** Update_Achievement API 已在 2016 年 8 月的 XDK 中发布。

3.  **必须为 XDK 或 UWP 游戏。** Achievements 2017 系统不可用于传统平台，包括 Xbox 360、Windows 8.x 或更早版本或者 Windows Phone 8 或更早版本。

## <a name="updateachievement-api"></a>Update_Achievement API

通过 [XDP](achievements-in-xdp.md) 或 [UDC](achievements-in-udc.md) 配置完成就并将其发布到开发沙盒后，你的游戏即可通过调用 Update_Achievement API 解锁成就。

此 API 在 XDK 和 Xbox Live SDK 中均有提供。

### <a name="api-signature"></a>API Signature

The API signature is as follows:

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

`xbox::services::xbox_live_result<T>` is the return call for all C++ Xbox Live Services API calls.

For more information, check out the Xfest 2015 talk, “XSAPI: C++, No Exceptions!”<br>
[视频](http://go.microsoft.com/?linkid=9888207) |  [幻灯片](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest_2015/Xbox_Live_Track/XSAPI_Cpp_No_Exceptions.pptx)

### <a name="unlocking-via-updateachievement-api"></a>通过 Update_Achievement API 解锁

若要解锁成就，请将 *percentComplete* 设为 100。

If the user is online, the request will be immediately sent to the Xbox Live Achievements service and will trigger the following user experiences:

-   The user will receive an Achievement Unlocked notification;

-   The specified achievement will appear as Unlocked in the user’s achievement list for the title;

-   The unlocked achievement will be added to the user’s activity feed.

> *Note: There will be no visible difference in user experiences for achievements that use the Achievements 2017 system and the Cloud-Powered Achievements.*

If the user is offline, the unlock request will be queued locally on the user’s device. When the user’s device has reestablished network connectivity, the request will automatically be sent to the Achievements service – note: no action is required from the game to trigger this – and the above user experiences will occur as described.

### <a name="updating-completion-progress-via-updateachievement-api"></a>通过 Update_Achievement API 更新完成进度

若要更新用户距离解锁成就的进度，请将 *percentComplete* 设为 1-100 之间的相应整数。

An achievement’s progress can only increase. If *percentComplete* is set to a number less than the achievement’s last *percentComplete* value, the update will be ignored. For example, if the achievement’s *percentComplete* had previously been set to 75, sending an update with a value of 25 will be ignored and the achievement will still be displayed as 75% complete.

If *percentComplete* is set to 100, the achievement will unlock.

如果将 *percentComplete* 设为大于 100 的数字，则 API 将按照设为 100 一样运行。

## <a name="frequently-asked-questions"></a>常见问题

### <a name="span-idwhyarechallenges-classanchorspancan-i-ship-my-title-using-the-achievements-2017-system-yet"></a><span id="_Why_are_Challenges" class="anchor"></span>我还可以使用 Achievements 2017 系统交付游戏吗？

没问题！ 欢迎并鼓励所有新游戏使用 Achievements 2017 系统来代替支持云的成就系统。

### <a name="why-are-challenges-not-supported-in-the-achievements-2017-system"></a>为什么在 Achievements 2017 系统中不支持挑战？

Usage data across Xbox games has shown that the current implementation and presentation of challenges does not fulfill a need for most game developers. We will continue gathering developer input and feedback in this space and endeavor to deliver future features that are more on point with developer needs. The newly released Xbox Arena is an example of a feature that introduces new competitive capabilities for Xbox games a new, but similar, direction.

### <a name="can-i-still-add-new-achievements-every-calendar-quarter-if-my-title-is-using-the-achievements-2017-system"></a>Can I still add new achievements every calendar quarter if my title is using the Achievements 2017 system?

Yes. The Achievements policy is unchanged.

### <a name="span-idwhycantexisting-classanchorspanwhy-cant-existing-titles-migrate-onto-the-new-achievements-2017-system"></a><span id="_Why_can’t_existing" class="anchor"></span>Why can’t existing titles “migrate” onto the new Achievements 2017 system?

For the vast majority of existing titles, a ‘migration’ to the Achievements 2017 system would not be limited to simply updating their service configurations and swapping out event writes for achievement unlock calls – although these changes alone would be very costly and would carry significant risk of error and unintended behavior that could result in the achievements being irreparably broken. Rather, most existing titles also have users with existing data. Attempting to convert a live game that is already using the Cloud-Powered Achievements system would not only be a very costly effort, for both the developer and Xbox, but would significantly jeopardize existing users’ profiles and/or game experiences.

### <a name="if-my-title-was-released-using-the-cloud-powered-achievements-system-can-any-future-dlc-for-the-title-switch-to-achievements-2017"></a>If my title was released using the Cloud-Powered Achievements system, can any future DLC for the title switch to Achievements 2017?

All achievements for a title must use the same Achievements system. Whichever Achievements system is used by the base game’s achievements is the system that must be used for all future achievements for the title.

### <a name="while-testing-achievements-in-my-dev-sandbox-can-i-mix-and-match-between-using-the-achievements-2017-system-and-the-cloud-powered-achievements-system"></a>While testing achievements in my dev sandbox, can I mix-and-match between using the Achievements 2017 system and the Cloud-Powered Achievements system?

No. 游戏的所有成就必须使用相同的成就系统。

### <a name="does-achievements-2017-also-include-offline-unlocks"></a>Achievements 2017 是否也包括脱机解锁？

如果游戏在设备处于脱机状态时解锁成就，则 Update_Achievement API 会将脱机解锁请求自动排队并在设备重新建立网络连接时自动同步至 Xbox Live，类似于当前支持云的成就系统的脱机体验。 用户处于脱机状态时不会发生成就解锁。

### <a name="i-see-a-new-achievementupdate-event-in-xdp-if-my-title-uses-that-event-does-that-mean-it-has-achievements-2017"></a>I see a new “AchievementUpdate” event in XDP. If my title uses that event, does that mean it has Achievements 2017?

The *AchievementUpdate* base event is required by Xbox Live for backend purposes. You can safely ignore it. If your title configures an event using this base event type, those event writes will be ignored by Xbox Live. Titles that are built on the Cloud-Powered Achievements system should continue to configure their events by using the other base event types. 在 Achievements 2017 系统上构建的游戏无需为成就配置*任何*事件。
