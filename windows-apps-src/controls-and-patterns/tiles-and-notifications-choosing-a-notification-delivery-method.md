---
Description: 本文介绍了用于传递磁贴和锁屏提醒更新以及 Toast 通知内容的四个通知选项：本地、计划、定期和推送。
title: 选择通知传递方法
ms.assetid: FDB43EDE-C5F2-493F-952C-55401EC5172B
label: 选择通知传递方法
template: detail.hbs
---

# 选择通知传递方法


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文介绍了用于传递磁贴和锁屏提醒更新以及 Toast 通知内容的四个通知选项：本地、计划、定期和推送。 当用户未直接与你的应用交互时，磁贴或 Toast 通知可以将信息传达给用户。 应用特性和内容以及希望传递的信息有助于确定最适合你的方案的一个或多个通知方法。

## <span id="Notification_delivery_methods__overview"> </span> <span id="notification_delivery_methods__overview"> </span> <span id="NOTIFICATION_DELIVERY_METHODS__OVERVIEW"> </span>通知传递方法概述


应用可使用 4 种机制传递通知：

-   **本地**
-   **计划**
-   **定期**
-   **推送**

下表总结了通知传递类型。

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">传递方法</th>
<th align="left">使用场景</th>
<th align="left">说明</th>
<th align="left">示例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">本地</td>
<td align="left">磁贴、锁屏提醒、Toast</td>
<td align="left">一组在应用运行时发送通知的 API 调用，直接更新磁贴或锁屏提醒，或者发送 Toast 通知。</td>
<td align="left"><ul>
<li>音乐应用更新其磁贴以显示“正在播放”的内容。</li>
<li>用户离开游戏时，游戏应用使用用户的最高分更新其磁贴。</li>
<li>启动应用时，将清除字形指示应用中有新信息的锁屏提醒。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">计划</td>
<td align="left">磁贴、Toast</td>
<td align="left">一组 API 调用，用于计划一个通知在你指定的时间更新。</td>
<td align="left"><ul>
<li>日历应用可为即将开展的会议设置 Toast 通知提醒。</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">定期</td>
<td align="left">磁贴、锁屏提醒</td>
<td align="left">以固定的时间间隔，通过轮询云服务查找新内容来定期更新磁贴和锁屏提醒。</td>
<td align="left"><ul>
<li>天气应用以 30 分钟的间隔更新它的磁贴，可显示天气预报。</li>
<li>“每日交易”站点每天早上更新它的每日交易。</li>
<li>用于显示直到某个事件每天半夜更新所显示的倒计时为止的天数的磁贴。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">推送</td>
<td align="left">磁贴、锁屏提醒、Toast、原始</td>
<td align="left">从一个云服务器发送的通知，即使应用未运行也是如此。</td>
<td align="left"><ul>
<li>购物应用发送一个 Toast 通知，让用户了解它们查看的商品的销量。</li>
<li>新闻应用在爆炸新闻发生时使用它更新其磁贴。</li>
<li>体育应用在一场比赛进行期间不断更新它的磁贴。</li>
<li>通信应用提供有关传入的消息或电话呼叫的提醒。</li>
</ul></td>
</tr>
</tbody>
</table>

 

## <span id="Local_notifications"> </span> <span id="local_notifications"> </span> <span id="LOCAL_NOTIFICATIONS"> </span>本地通知


在应用正在运行时更新应用磁贴或锁屏提醒或者发出一个 Toast 通知是最简单的通知传递机制；它只需要本地 API 调用。 每个应用都具有要显示在磁贴上的有用或有趣的信息，即使该内容仅在用户启动应用并与其交互后更改也是如此。 即使你还使用了其他三种通知机制中的一种，本地通知也是保持应用磁贴最新的好方法。 例如，照片应用磁贴可以显示来自最近添加的相册的照片。

建议你的应用在第一次启动时本地更新其磁贴，或至少在用户进行你的应用会在磁贴上正常反映的更改后立即更新其磁贴。 直到用户离开应用才能看到此更新，但是通过在使用该应用时做出这一更改可确保磁贴在用户离开时已是最新状态。

尽管这些 API 调用是本地的，但通知可引用 Web 图像。 如果 Web 图像不可供下载、已损坏或不符合图像规范，磁贴和 Toast 的反应会有所不同：

-   磁贴：不会显示更新
-   Toast：显示通知，但带有占位符图像

虽然本地通知不会过期，但最佳做法是设置明确的过期时间。

有关详细信息，请参阅以下主题：

-   [发送本地磁贴通知](tiles-and-notifications-sending-a-local-tile-notification.md)
-   [通用 Windows 平台 (UWP) 通知代码示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <span id="Scheduled_notifications"> </span> <span id="scheduled_notifications"> </span> <span id="SCHEDULED_NOTIFICATIONS"> </span>计划通知


计划通知是本地通知的子集，可以指定应更新磁贴或显示 Toast 通知的准确时间。 计划通知是已提前知道更新内容等情况的理想选择，例如会议邀请。 如果不能提前知道通知内容，应该使用推送或定期通知。

默认情况下，计划通知在从推送时刻算起的三天后过期。 如果需要，你可以使用明确的过期时间替代此默认设置。

有关详细信息，请参阅以下主题：

-   [计划通知指南](https://msdn.microsoft.com/library/windows/apps/hh761464)
-   [通用 Windows 平台 (UWP) 通知代码示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <span id="Periodic_notifications"> </span> <span id="periodic_notifications"> </span> <span id="PERIODIC_NOTIFICATIONS"> </span>定期通知


定期通知只需极少的云服务和客户端投资即可提供动态磁贴更新。 它们也是向大量访问群体分发相同内容的出色方法。 你的客户端代码指定 Windows 将轮询磁贴或锁屏提醒更新的云位置 URL，以及应该轮询该位置的频率。 在每个轮询间隔，Windows 访问该 URL 以下载指定的 XML 内容并在磁贴上显示它。

定期通知需要应用托管一个云服务，此服务将以指定的间隔由所有安装了该应用的用户轮询。 请注意，定期更新不能用于 Toast 通知，Toast 通知最好使用计划或推送通知。

默认情况下，定期通知在从轮询发生时刻算起的三天后过期。 如果需要，你可以使用明确的过期时间替代此默认设置。

有关详细信息，请参阅以下主题：

-   [定期通知概述](tiles-and-notifications-periodic-notification-overview.md)
-   [通用 Windows 平台 (UWP) 通知代码示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <span id="Push_notifications"> </span> <span id="push_notifications"> </span> <span id="PUSH_NOTIFICATIONS"> </span>推送通知


当与实时数据或用户个性化的数据通信时，推送通知是理想之选。 推送通知用于在无法预测的时间生成内容，例如爆炸性新闻、社交网络更新或即时消息。 推送通知也可用于数据具有时效性且不适合使用定期通知的情况，例如游戏期间的比赛计分。

推送通知需要一个云服务，该服务将管理推送通知通道并选择何时以及向谁发送通知。

默认情况下，推送通知在从 Windows 推送通知服务 (WNS) 收到推送通知那一刻算起的三天后过期。 如果需要，你可以使用明确的过期时间替代此默认设置。

有关详细信息，请参阅：

-   [Windows 推送通知服务 (WNS) 概述](tiles-and-notifications-windows-push-notification-services--wns--overview.md)
-   [推送通知指南](https://msdn.microsoft.com/library/windows/apps/hh761462)
-   [通用 Windows 平台 (UWP) 通知代码示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

**注意**  
本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## <span id="related_topics"> </span>相关主题


* [发送本地磁贴通知](tiles-and-notifications-sending-a-local-tile-notification.md)
* [推送通知指南](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [计划通知指南](https://msdn.microsoft.com/library/windows/apps/hh761464)
* [Toast 通知指南](https://msdn.microsoft.com/library/windows/apps/hh465391)
* [定期通知概述](tiles-and-notifications-periodic-notification-overview.md)
* [Windows 推送通知服务 (WNS) 概述](tiles-and-notifications-windows-push-notification-services--wns--overview.md)
* [GitHub 上的通用 Windows 平台 (UWP) 通知代码示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)
 

 






<!--HONumber=Mar16_HO1-->


