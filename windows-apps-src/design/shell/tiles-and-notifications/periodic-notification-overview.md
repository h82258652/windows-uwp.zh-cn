---
Description: Periodic notifications, which are also called polled notifications, update tiles and badges at a fixed interval by downloading content from a cloud service.
title: 定期通知概述
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f7a5054fde1a1a24945b193f578b8389519dc2d5
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7971800"
---
# <a name="periodic-notification-overview"></a>定期通知概述
 


定期通知（也称为轮询通知）通过从云服务下载内容，以固定间隔更新磁贴和锁屏提醒。 若要使用定期通知，客户端应用代码需要提供两部分信息：

-   Windows 为你的应用轮询磁贴或锁屏提醒更新的 Web 位置的统一资源标识符 (URI)
-   URI 的轮询频率

定期通知让你的应用只需极少的云服务和客户端投资即可提供动态磁贴更新。 定期通知是向广泛受众分配相同内容的出色传递方法。

**注意**你可以了解有关 Windows8.1 下载[推送和定期通知示例](http://go.microsoft.com/fwlink/p/?linkid=231476)和 windows 10 应用中重复使用其源代码。

 

## <a name="how-it-works"></a>工作原理


定期通知要求应用托管一个云服务。 该服务将由所有安装了该应用的用户定期进行轮询。 Windows 在每个轮询间隔（例如一小时一次）向 URI 发送 HTTP GET 请求，下载响应请求而提供的磁贴或锁屏提醒内容（如 XML），并在应用磁贴上显示此内容。

请注意，定期更新不能与 Toast 通知配合使用。 Toast 最好通过[计划](https://msdn.microsoft.com/library/windows/apps/hh465417)或[推送](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)通知来传递。

## <a name="uri-location-and-xml-content"></a>URI 位置和 XML 内容


任何有效的 HTTP 或 HTTPS 网址均可用作要轮询 URI。

云服务器的响应包括已下载的内容。 从 URI 返回的内容必须符合[磁贴](adaptive-tiles-schema.md)或[锁屏提醒](https://msdn.microsoft.com/library/windows/apps/br212851) XML 架构规格，并且必须为 UTF-8 编码。 可使用已定义的 HTTP 标头来指定通知的[到期时间](#expiration-of-tile-and-badge-notifications)或标记。

## <a name="polling-behavior"></a>轮询行为


可调用以下方法之一开始轮询：

-   [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_)（磁贴）
-   [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.BadgeUpdater#Windows_UI_Notifications_BadgeUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_)（锁屏提醒）
-   [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_)（磁贴）

调用一个方法时，URI 将立即进行轮询，且磁贴或锁屏提醒将根据所接收的内容进行更新。 初次轮询后，Windows 继续按照所要求的间隔提供更新。 轮询不断进行，直到（使用 [**TileUpdater.StopPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater.StopPeriodicUpdate)）将其显式阻止、卸载应用或删除磁贴（适用于辅助磁贴的情况）。 否则，即使应用从此再不启动，Windows 也将继续轮询磁贴或锁屏提醒更新。

### <a name="the-recurrence-interval"></a>重复出现的间隔

指定重复出现的间隔，作为上面所列方法的一个参数。 请注意，虽然 Windows 尽量根据请求进行轮询，但此间隔不是很准确。 所请求的轮询间隔最长可能延迟 15 分钟，具体取决于 Windows。

### <a name="the-start-time"></a>开始时间

你可以选择指定一天中的某个特定时间开始轮询。 考虑应用一天仅更改一次磁贴内容。 在此情况下，我们建议你将轮询开始时间设置为接近更新云服务的时间。 例如，如果一家日常购物网站每天早上 8 点发布当日的商品与服务，则在早上 8 点不久之后轮询新磁贴内容。

如果提供开始时间，则首次调用该方法时即会立即轮询内容。 然后，在指定开始时间的 15 分钟之内开始定期轮询。

### <a name="automatic-retry-behavior"></a>自动重试行为

只能在设备联机时轮询 URI。 如果网络可用，但因某种原因无法联系 URI，则轮询间隔此次迭代将被跳过，并将在下个间隔再次轮询 URI。 如果达到轮询间隔时，该设备处于关闭、睡眠或休眠状态，则设备从关闭或睡眠状态返回时，将轮询 URI。

### <a name="handling-app-updates"></a>处理应用更新

如果发布更改轮询 URI 的应用更新，你应该添加使用新 URI 调用 StartPeriodicUpdate 的每日[时间触发器后台任务](../../../launch-resume/run-a-background-task-on-a-timer-.md)，以确保磁贴使用新的 URI。 否则，如果用户收到应用更新但未启动应用，他们的磁贴仍将使用旧 URI，如果 URI 已无效或者返回负载引用了本地不再存在的图像，磁贴可能会无法显示。

## <a name="expiration-of-tile-and-badge-notifications"></a>磁贴和锁屏提醒通知到期时间


默认情况下，定期磁贴和锁屏提醒通知在从下载时刻算起的三天后过期。 通知到期时，此内容将从锁屏提醒、磁贴或队列中删除，且不再向用户显示。 最佳做法是，使用一个对于你的应用或通知合理的时间，在所有定期磁贴和锁屏提醒通知上设置显式过期时间，以确保该内容不会在它不相关时仍存在。 对于具有已定义的使用寿命的内容来说，一个显式过期时间是必需的。 它还可确保在无法连接云服务或用户长时间断开网络时删除过时的内容。

你的云服务通过在响应负载中包括 X-WNS-Expires HTTP 标头来为通知设置过期日期和时间。 X-WNS-Expires HTTP 标头符合 [HTTP-date 格式](http://go.microsoft.com/fwlink/p/?linkid=253706)。 有关详细信息，请参阅 [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) 或 [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_)。

例如，股票市场活跃交易日期间，你可将股票价格更新到期时间设置为轮询间隔的两倍（例如，如果是每半小时轮询一次，则将股票价格更新到期时间设置为一小时）。 另一个示例是，新闻应用可确定每日新闻磁贴更新的适当到期时间为一天。

## <a name="periodic-notifications-in-the-notification-queue"></a>通知队列中的定期通知


你可将定期磁贴更新与[通知循环](https://msdn.microsoft.com/library/windows/apps/hh781199)结合使用。 默认情况下，“开始”屏幕中的磁贴会显示单条通知的内容，直到有新的通知替换当前的通知。 启用循环之后，队列中最多会保留 5 条通知，磁贴会循环显示这些通知。

如果队列达到了 5 条通知的容量上限，则下一条新通知将替换队列中最早的通知。 但是，通过在通知上设置标记，你可以影响队列的替换策略。 标记是特定于应用的字符串，区分大小写，最多包含 16 个字母数字字符，可在响应负载的 [X-WNS-Tag](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_tag) HTTP 标头中指定。 Windows 将传入通知的标记与队列中已存在的所有通知的标记进行比较。 如果发现匹配，则新通知将替换队列中标记相同的通知。 如果发现不匹配，则应用默认替换规则，新通知将替换队列中最早的通知。

你可使用通知队列和标记来实现各种丰富的通知方案。 例如，股票应用可发送 5 条通知，每条通知关注一支不同的股票并以相应股票的名称作为标记。 这可以防止队列包含两条有关同一股票的通知，其中较早的通知为过时通知。

有关详细信息，请参阅[使用通知队列](https://msdn.microsoft.com/library/windows/apps/hh781199)。

### <a name="enabling-the-notification-queue"></a>启用通知队列

若要实现通知队列，请首先启用磁贴队列（请参阅[如何使用具有本地通知的通知队列](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/01/05/quickstart-how-to-use-the-tile-notification-queue-with-local-notifications/)）。 在应用的生命周期内，调用启用队列必须一次性完成，但在每次启动应用时均调用一次也无妨。

### <a name="polling-for-more-than-one-notification-at-a-time"></a>你可以一次轮询多条通知。

你必须为希望 Windows 为磁贴下载的每条通知提供唯一的 URI。 你可使用 [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_) 方法一次最多提供五个 URI 与通知队列配合使用。 将在相同时间或接近该时间轮询每个 URI 以获得单个通知负载。 每个轮询的 URL 还可以返回各自的到期时间和标记值。

## <a name="related-topics"></a>相关主题


* [定期通知指南](https://msdn.microsoft.com/library/windows/apps/hh761461)
* [如何为锁屏提醒设置定期通知](https://msdn.microsoft.com/library/windows/apps/hh761476)
* [如何为磁贴设置定期通知](https://msdn.microsoft.com/library/windows/apps/hh761476)
 
