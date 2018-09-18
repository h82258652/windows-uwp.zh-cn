---
author: JnHs
Description: Learn how to send notifications from Windows Dev Center to your app to encourage groups of customers to take an action, such as rating an app or buying an add-on.
title: 将目标推送通知发送到应用客户
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 目标通知, 推送通知, toast, 磁贴
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.localizationpriority: medium
ms.openlocfilehash: 9d62f46ad1b55fbad3ab7c21a593625a2538b68f
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "4019685"
---
# <a name="send-notifications-to-your-apps-customers"></a>向应用客户发送通知

在正确的时间，通过正确的消息吸引客户是作为应用开发人员取得成功的关键。 通知可以鼓励客户采取行动，例如为应用评分、购买加载项、试用新功能或下载其他应用（或许可通过你提供的[促销代码](generate-promotional-codes.md)免费获取）。

Windows 开发人员中心提供以数据驱动的客户参与平台，可用于将通知发送到所有应用客户，或仅面向符合[客户类别](create-customer-segments.md)中所定义条件的应用的 Windows10 客户子集发送。 <!-- You can also send a single notification to all of the customers for multiple apps. -->

> [!IMPORTANT]
> 此类通知仅可与 UWP 应用一起使用。

当考虑通知的内容时，请记住：
- 通知内容必须符合应用商店[内容策略](https://docs.microsoft.com/legal/windows/agreements/store-policies#content_policies)。
- 通知内容不应包含机密信息或潜在敏感信息。
- 虽然我们将尽力按计划提供你的通知，但有时可能存在影响交付的延迟问题。
- 请确保不要太频繁发送通知。 频率高于每 30 分钟一次似乎会产生干扰（在很多情况下，最好不要超过该频率）。
- 请注意，如果使用你的应用（并且在确定类别成员身份时使用 Microsoft 帐户登录）的客户稍后会将其设备给其他人使用，则其他用户可能会看到面向原始客户的通知。 有关详细信息，请参阅[针对定向推送通知配置应用](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers)。
- 如果你向多个应用的客户发送相同的通知，你不能针对某个段；将向你选择的应用的所有客户发送通知。


## <a name="getting-started-with-notifications"></a>通知入门

在高级别上，需要完成三件事才能使用通知与客户接洽。

1. **注册应用以接收推送通知。** 在应用中添加 Microsoft Store Services SDK 引用，然后添加几行在开发人员中心和应用之间注册通知通道的代码，可完成此操作。 我们将使用该通道向你的客户传递通知。 有关详细信息，请参阅[针对定向推送通知配置应用](../monetize/configure-your-app-to-receive-dev-center-notifications.md)。
2. **确定目标客户。** 你可以向所有应用客户，或（对于为单个应用创建的通知）称为*段*的客户组发送通知，具体可以根据人口统计或收入条件来定义。 有关详细信息，请参阅[创建客户类别](create-customer-segments.md)。
3. **创建通知内容并发送。** 例如，你可以创建鼓励新客户为应用评分的通知，或者发送促进购买加载项的特别优惠的通知。


## <a name="to-create-and-send-a-notification"></a>创建并发送通知

请按照以下步骤在仪表板中创建通知，并将其发送给特定的客户类别。

> [!NOTE]
> 必须先在应用中调用 [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) 方法来注册应用以接收通知，应用才可从开发人员中心接收通知。 此方法在 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 中可用。 有关如何调用此方法的详细信息以及代码示例，请参阅[针对定向推送通知配置应用](../monetize/configure-your-app-to-receive-dev-center-notifications.md)。

1. 展开 [Windows 开发人员中心仪表板](https://partner.microsoft.com/dashboard/)中的**参与**部分，然后选择**通知**。
2. 在**通知**页上，选择**新通知**。
3. 在**选择模板**部分中，选择你想要发送，然后单击**确定**[通知类型](#notification-template-types)。
4. 在下一页中，使用下拉菜单选择要为其生成通知的**单个应用**或**多个应用**。 你只能选择已[配置为使用 Microsoft Store Services SDK 接收通知](../monetize/configure-your-app-to-receive-dev-center-notifications.md)的应用。
5. 在**通知设置**部分，选择通知**名称**，并选择通知发送目标的**客户组**（如果适用）。 （发送至多个应用的通知只能发送至这些应用的所有客户。）如果要使用你尚未创建的段，请选择**创建新客户组**。 请注意，新段 24 小时之后才可用作通知。 有关详细信息，请参阅[创建客户类别](create-customer-segments.md)。
6. 如果你想要指定何时发送通知，清除**立即发送通知**复选框并选择特定的日期和时间（除非指定为使用每个客户的当地时区，否则所有客户均使用 UTC 时间）。
7. 如果希望通知在某个时间点过期，请清除**通知永远不会过期**复选框，然后选择特定过期日期和时间（UTC 时间）。
8. **面向单个应用的通知：** 如果要筛选收件人，以便通知仅发送给使用特定语言或位于特定时区的人员，请勾选**使用筛选器**复选框。 然后你可以指定想要使用的语言和/或时区选项。
8. **面向多个应用的通知：** 指定仅将通知发送给每台设备上的最后一个活动应用（每个客户）还是发送给每台设备上的所有应用。
10. 在**通知内容**部分的**语言**菜单中，选择显示通知的语言。 有关详细信息，请参阅[翻译通知](#translate-your-notifications)。
11. 在**选项**部分中，输入文本，然后配置其他任何要配置的选项。 如果首先使用的是模板，有些模板已默认提供，但可进行任何所需更改。

    可用选项各不相同，具体取决于所使用的通知类型。 一些选项为：

    * **激活类型**（交互式 Toast 类型）。 可选择**前台**、**后台**或**协议**。
    * **启动**（交互式 Toast 类型）。 可选择使通知打开某个应用或网站。
    * **跟踪应用启动率**（交互式 Toast 类型）。 如果想要测量通过每条通知与客户的交流程度，请选择此复选框。 有关详细信息，请参阅[测量通知性能](#measure-notification-performance)。
    * **持续时间**（交互式 Toast 类型）。 可选择**短**或**长**。
    * **方案**（交互式 Toast 类型）。 可选择**默认**、**警报**、**提醒**或**来电**。
    * **基本 URI**（交互式 Toast 类型）。 有关详细信息，请参阅 [BaseUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.baseuri#Windows_UI_Xaml_FrameworkElement_BaseUri)。
    * **添加图像查询**（交互式 Toast 类型）。 有关详细想信息，请参阅 [addImageQuery](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual#attributes-and-elements)。
    * **可视**。 图像、视频或声音。 有关详细信息，请参阅[视觉](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-visual)。
    * **输入**/**操作**/**选择**（交互式 Toast 类型）。 允许你让用户通过通知交互。 有关详细信息，请参阅[自适应和交互式 Toast 通知](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)。
    * **绑定**（交互式磁贴类型）。 Toast 模板。 有关详细信息，请参阅[绑定](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/element-binding)。

    > [!TIP]
    > 请尝试使用[通知可视化工具](https://www.microsoft.com/store/apps/9nblggh5xsl1)应用设计并测试自适应磁贴和交互式 Toast 通知。

12. 选择**保存为草稿**，稍后继续处理该通知，或者在全部操作完成时选择**发送**。


## <a name="notification-template-types"></a>通知模板类型

可从大量通知模板中进行选择。

-   **空白 (Toast)。** 首先从可自定义的空白 Toast 通知开始。 Toast 通知是显示在屏幕上的弹出 UI，当客户在另一个应用中、在“开始”屏幕上或在桌面上时，通过 Toast 通知应用可与客户进行通信。
-   **空白（磁贴）。** 首先从可自定义的空白磁贴通知开始。 磁贴是应用在“开始”屏幕上的表示形式。 磁贴可以是“实时的”，这意味着它们显示的内容可以为了响应通知而进行更改。
-   **请求评分 (Toast)。** 请求客户为应用评分的 Toast 通知。 当客户选择此通知时，将显示应用的应用商店评分页面。
-   **请求反馈 (Toast)。** 请求客户为应用提供反馈的 Toast 通知。 当客户选择此通知时，将显示应用的反馈中心页面。
    > [!NOTE]
    > 如果选择此模板类型，则在**启动**框中，请记住使用应用实际的包系列名称 (PFN) 替换 {PACKAGE_FAMILY_NAME} 占位符值。 可在[应用标识](view-app-identity-details.md)页中找到应用的 PFN（**应用管理** > **应用标识**）。

    ![反馈 Toast 启动框](images/push-notifications-feedback-toast-launch-box.png)

-   **交叉推广 (Toast)。** 推广所选其他应用的 Toast 通知。 当客户选择此通知时，将显示其他应用的应用商店一览。
    > [!NOTE]
    > 如果选择此模板类型，则在**启动**框中，请记住使用要交叉推广的商品的实际应用商店 ID 替换 **{要在此处推广的 ProductId}** 占位符值。 可在[应用标识](view-app-identity-details.md)页查找应用商店 ID（**应用管理** > **应用标识**）。

    ![交叉推广 Toast 启动框](images/push-notifications-promote-toast-launch-box.png)

-   **推广促销 (Toast)。** 用于宣布达成应用交易的 Toast 通知。 当客户选择此通知时，将显示应用的应用商店一览。
-   **更新提示 (Toast)。** 鼓励运行旧版应用的客户安装最新版本的 Toast 通知。 当客户选择此通知时，Microsoft Store 应用将启动，并显示**下载和更新**列表。 请注意，此模板仅用于单个应用，你无法针对特定的客户段或定义发送时间；我们始终会安排在 24 小时内发送此通知，并会尽最大努力覆盖尚未运行应用最新版本的所有用户。


## <a name="measure-notification-performance"></a>测量通知性能

可测量通过每条通知与客户的交流程度。


### <a name="to-measure-notification-performance"></a>测量通知性能

1.  创建通知时，请在**通知内容**部分中选中**跟踪应用启动率**复选框。
2.  在应用中，为响应针对性通知，请调用 [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) 方法通知开发人员中心应用已启动。 此方法由 Microsoft Store Services SDK 提供。 有关如何调用此方法的详细信息，请参阅[配置应用以接收开发人员中心通知](../monetize/configure-your-app-to-receive-dev-center-notifications.md)。


### <a name="to-view-notification-performance"></a>查看通知性能

如上所述配置通知和应用，使之可测量通知性能后，可使用仪表板查看通知执行的程度。

若要查看每个通知的详细的数据：

1.  展开 Windows 开发人员中心仪表板中的**参与**部分，然后选择**通知**。
2.  在现有通知的表中，选择**正在进行**或**已完成**，，然后查看**传送速率**和**应用启动率**列，以查看每个通知的高级性能。
3.  若要查看更详细的性能细节，请选择通知名称。 在**交货统计**部分，可以查看以下通知**状态**类型的**计数**和**百分比**信息：
    * **失败**：由于某些原因，通知未发送。 这可能会在 Windows 通知服务发生问题时出现。
    * **通道过期失败**：通知由于应用和开发人员中心之间的通道过期而无法发送。 这可能会在客户长时间没有打开应用的情况下发生。
    * **正在发送**：通知排在待发送队列中。
    * **已发送**：通知已发送。
    * **启动数**：通知已发送、客户已点击该通知，最后应用也打开。 请注意，这仅跟踪应用启动数。 此状态不包含邀请客户执行其他操作的通知，例如启动应用商店以留下评分。
    * **未知**：我们无法确定此通知的状态。

若要分析你的所有通知的用户活动数据：

1.  展开 Windows 开发人员中心仪表板中的**参与**部分，然后选择**通知**。
2.  在**通知**页面上，单击**分析**选项卡。此选项卡上显示以下数据：
    * 为你的 toast 和操作中心通知的各种用户操作状态的图形视图。
    * 为你的 toast 和操作通过率单击的世界地图视图中心通知。
3. 在页面顶部附近，可以选择希望显示数据的时间段。 默认选择为 30D（30 天），但你可以选择要显示 3、6 或 12 个月的数据或指定的自定义数据范围的数据。 你还可以展开**筛选器**筛选的所有数据的应用和市场。

## <a name="translate-your-notifications"></a>翻译通知

若要最大化发挥通知的影响力，请考虑将它们翻译为客户首选的语言。 开发人员中心利用 [Microsoft Translator](https://www.microsoft.com/translator/home.aspx) 服务的力量，轻松为你自动翻译通知。

1.  使用默认语言编写通知后，选择**添加语言**（在**通知内容**部分的**语言**菜单下）。
2.  在**添加语言**窗口下，选择用以显示通知的其他语言，然后选择**更新**。
通知将自动译为在**添加语言**中选择的语言，并且这些语言将添加到**语言**菜单。
3.  若要查看通知译文，请在**语言**菜单中选择刚才添加的语言。

对于翻译需要记住的有关事项：
 - 在该语言的**内容**框中输入其他内容将覆盖自动译文。
 - 如果在覆盖自动译文后向通知的英文版添加其他文本框，新文本框将不会添加到翻译后的通知。 在这种情况下，需要将新文本框手动添加到每个翻译后的通知。
 - 如果在通知翻译后更改英文文本，我们将自动更新翻译的通知以匹配更改。 但是，如果之前已选择覆盖初始译文，这种情况将不会发生。

## <a name="related-topics"></a>相关主题
- [适用于 UWP 应用的磁贴](../design/shell/tiles-and-notifications/creating-tiles.md)
- [Windows 推送通知服务 (WNS) 概述](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
- [通知可视化工具应用](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager.RegisterNotificationChannelAsync() | registerNotificationChannelAsync() 方法](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)
