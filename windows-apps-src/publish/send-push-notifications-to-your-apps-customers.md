---
author: shawjohn
Description: "了解如何将目标推送通知从 Windows 开发人员中心发送到应用，鼓励客户采取行动，例如为应用评分或购买加载项。"
title: "将目标推送通知发送到应用客户"
translationtype: Human Translation
ms.sourcegitcommit: 3e3c9737784c81f5eb882296a82a4dcd879363e1
ms.openlocfilehash: 817043579cfd068267c54f2eab9210ef303ca364

---

# 将目标推送通知发送到应用客户

在正确的时间，通过正确的消息吸引客户是作为应用开发人员取得成功的关键。 Windows 开发人员中心提供以数据驱动的客户参与平台，可用于将推送通知发送到所有客户或仅发送到符合在[客户类别](create-customer-segments.md)中定义的条件的 Windows10 客户子集。

可使用目标推送通知鼓励客户采取行动，例如为应用评分、购买加载项、试用新功能或下载其他应用。

> 
            **重要提示** 目标推送通知仅可与 UWP 应用一起使用。

## 推送通知入门

在高级别上，需要做三件事才能使用推送通知吸引客户。
1. **注册应用以接收推送通知。** 在应用中添加 Microsoft Store Services SDK 引用，然后添加几行在开发人员中心和应用之间注册通知通道的代码，可完成此操作。 我们将使用该通道向你的客户传递推送通知。 有关详细信息，请参阅[注册通知通道](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.engagementclient.registernotificationchannelasync.aspx)。
2. **创建一个或多个希望面向的客户类别。** 可根据人口统计或收益条件将客户分组到各个类别中。 有关详细信息，请参阅[创建客户类别](create-customer-segments.md)。
3. **创建推送通知，并将其发送到特定的客户类别中。** 例如，发送鼓励新客户为应用评分的通知，或者发送带有购买加载项的特殊交易的通知。

## 创建和发送目标推送通知

1. 如果尚未创建和发送目标推送通知，请在应用中安装 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)，并在启动代码中调用 [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) 方法，注册应用以接收通知。 有关如何调用此方法的详细信息，请参阅[配置应用以接收开发人员中心通知](../monetize/configure-your-app-to-receive-dev-center-notifications.md)。
2.  在 [Windows 开发人员中心仪表板](https://developer.microsoft.com/dashboard/overview)中，选择应用。
3.  在左侧导航菜单中，展开“服务”，然后选择“推送通知”。
4.  在“目标推送通知”页上，选择“新通知”。
5.  在“选择模板”部分，选择希望发送的通知类型。 有关详细信息，请参阅[通知模板类型](#notification-template-types)。
  ![通知模板](images/push-notifications-template.png)
6.  在“通知设置”区域，选择通知“名称”，并选择通知发送目标的“客户组”。
如果尚未创建类别，请选择“创建新客户组”。 请注意，新类别 24 小时之后才可用作通知。 有关详细信息，请参阅[创建客户类别](create-customer-segments.md)。
7.  如果希望指定发送通知的时间，请清除“立即发送通知”复选框，然后选择特定日期和时间。
8.  如果希望通知在某个时间点过期，请清除“通知永远不会过期”复选框，然后选择特定过期日期和时间。
9.  在“通知内容”部分的“语言”菜单中，选择显示通知的语言。 有关详细信息，请参阅[翻译通知](#translate-your-notifications)。
10. 在“选项”部分中，输入文本，然后配置其他任何要配置的选项。 如果首先使用的是模板，有些模板已默认提供，但可进行任何所需更改。
   可用选项各不相同，具体取决于所使用的通知类型。 一些选项为：
   - 
            **激活类型**（交互式 Toast 类型）。 可选择“前台”、“后台”或“协议”。
   - 
            **启动**（交互式 Toast 类型）。 可选择使通知打开某个应用或网站。
   - 
            **跟踪应用启动率**（交互式 Toast 类型）。 如果想要测量通过每条通知与客户的交流程度，请选择此复选框。 有关详细信息，请参阅[测量通知性能](#measure-notification-performance)。
   - 
            **持续时间**（交互式 Toast 类型）。 可选择“短”或“长”。
   - 
            **方案**（交互式 Toast 类型）。 可选择“默认”、“警报”、“提醒”或“来电”。
   - 
            **基本 URI**（交互式 Toast 类型）。 有关详细信息，请参阅 [BaseUri](https://msdn.microsoft.com/library/windows/apps/br208712)。
   - 
            **添加图像查询**（交互式 Toast 类型）。 有关详细想信息，请参阅 [addImageQuery](https://msdn.microsoft.com/library/windows/apps/br230847)。
   - 
            **视觉**。 图像、视频或声音。 有关详细信息，请参阅[视觉](https://msdn.microsoft.com/library/windows/apps/br230847)。
   - 
            **输入**
            /
            **操作**
            /
            **选择**（交互式 Toast 类型）。 允许你让用户通过通知交互。 有关详细信息，请参阅[自适应和交互式 Toast 通知](../controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts.md#actions)。
   - 
            **绑定**（交互式磁贴类型）。 Toast 模板。 有关详细信息，请参阅[绑定](https://msdn.microsoft.com/en-us/library/windows/apps/br230843)。

   > 
            **使用技巧** 尝试使用[通知可视化工具](https://www.microsoft.com/store/apps/9nblggh5xsl1)应用设计并测试自适应磁贴和交互式 Toast 通知。

11. 选择“保存为草稿”，稍后继续处理该通知，或者在全部操作完成时选择“发送”。

> 
            **注意** 通知内容必须符合应用商店[内容策略](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#content_policies)。

## 通知模板类型

可从大量通知模板中进行选择。

-   **空白 (Toast)。** 首先从可自定义的空白 Toast 通知开始。 Toast 通知是显示在屏幕上的弹出 UI，当客户在另一个应用中、在“开始”屏幕上或在桌面上时，通过 Toast 通知应用可与客户进行通信。
-   **空白（磁贴）。** 首先从可自定义的空白磁贴通知开始。 磁贴是应用在“开始”屏幕上的表示形式。 磁贴可以是“实时的”，这意味着它们显示的内容可以为了响应通知而进行更改。
-   **请求评分 (Toast)。** 请求客户为应用评分的 Toast 通知。 当客户选择此通知时，将显示应用的应用商店评分页面。
-   **请求反馈 (Toast)。** 请求客户为应用提供反馈的 Toast 通知。 当客户选择此通知时，将显示应用的反馈中心页面。
   > 
            **注意** 如果选择此模板类型，则在“启动”框中，请记住使用应用实际的程序包系列名称 (PFN) 替换 {PACKAGE_FAMILY_NAME} 占位符值。 可在[应用标识](view-app-identity-details.md)页中找到应用的 PFN（“应用管理” > “应用标识”）。

   ![反馈 Toast 启动框](images/push-notifications-feedback-toast-launch-box.png)
-   **交叉推广 (Toast)。** 推广所选其他应用的 Toast 通知。 当客户选择此通知时，将显示其他应用的应用商店一览。
  > 
            **注意** 如果选择此模板类型，则在“启动”框中，请记住使用要交叉推广的商品的实际应用商店 ID 替换 **{要在此处推广的 ProductId}** 占位符值。 可在 [应用标识](view-app-identity-details.md)页查找应用商店 ID（“应用管理” > “应用标识”）。

  ![交叉推广 Toast 启动框](images/push-notifications-promote-toast-launch-box.png)
-   **推广促销 (Toast)。** 用于宣布达成应用交易的 Toast 通知。 当客户选择此通知时，将显示应用的应用商店一览。
- **更新提示 (Toast)。** 鼓励运行旧版应用的客户安装最新版本的 Toast 通知。 当客户选择此通知时，将显示“应用商店”应用中的“下载和更新”列表。 请注意，使用此模板无需创建客户类别。 我们计划在 24 小时内发送此通知，并尽最大努力面向所有未运行最新版应用的用户。

## 测量通知性能

可测量通过每条通知与客户的交流程度。

###测量通知性能

1.  创建通知时，请在“通知内容”部分中选中“跟踪应用启动率”复选框。
2.  在应用中，为响应目标通知，请调用 [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) 方法通知开发人员中心应用已启动。 此方法由 Microsoft Store SDK 提供。 有关如何调用此方法的详细信息，请参阅[配置应用以接收开发人员中心通知](../monetize/configure-your-app-to-receive-dev-center-notifications.md)。

###查看通知性能

如上所述配置通知和应用，使之可[测量通知性能](#to-measure-notification-performance)后，可使用仪表板查看通知执行的程度。

1.  在仪表板上选择一个应用。
2.  在左侧菜单中展开“服务”部分，然后选择“推送通知”，查看与该应用关联的通知。
3.  在“目标推送通知”页面上，选择“进行中”或“已完成”，然后查看“传送速率”和“应用启动率”列，了解每个通知的高级性能。
4.  若要查看更详细的性能细节，请选择通知名称。 显示“交货统计”部分，并显示以下通知“状态”类型的“计数”和“百分比”信息：
 - 
            **失败**：由于某些原因，通知未发送。 这可能会在 Windows 通知服务发生问题时出现。
 - 
            **通道过期失败**：通知由于应用和开发人员中心之间的通道过期而无法发送。 这可能会在客户长时间没有打开应用的情况下发生。
 - 
            **正在发送**：通知排在待发送队列中。
 - 
            **已发送**：通知已发送。
 - 
            **启动数**：通知已发送、客户已点击该通知，最后应用也打开。 请注意，这仅跟踪应用启动数。 此状态不包含邀请客户执行其他操作的通知，例如启动应用商店以留下评分。
 - 
            **未知**：我们无法确定此通知的状态。

## 翻译通知

若要最大化发挥通知的影响力，请考虑将它们翻译为客户首选的语言。 开发人员中心利用 [Microsoft Translator](https://msdn.microsoft.com/library/dd576287.aspx) 服务的力量，轻松为你自动翻译通知。

1.  使用默认语言编写通知后，选择“添加语言”（在“通知内容”部分的“语言”菜单下）。
2.  在“添加语言”窗口下，选择用以显示通知的其他语言，然后选择“更新”。
通知将自动译为在“添加语言”中选择的语言，并且这些语言将添加到“语言”菜单。
3.  若要查看通知译文，请在“语言”菜单中选择刚才添加的语言。

对于翻译需要记住的有关事项：
 - 在该语言的“内容”框中输入其他内容将覆盖自动译文。
 - 如果在覆盖自动译文后向通知的英文版添加其他文本框，新文本框将不会添加到翻译后的通知。 在这种情况下，需要将新文本框手动添加到每个翻译后的通知。
 - 如果在通知翻译后更改英文文本，我们将自动更新翻译的通知以匹配更改。 但是，如果之前已选择覆盖初始译文，这种情况将不会发生。

## 相关主题
- [适用于 UWP 应用的磁贴、锁屏提醒和通知](../controls-and-patterns/tiles-badges-notifications.md)
- [Windows 推送通知服务 (WNS) 概述](../controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview.md)
- [Windows 推送通知服务 (WNS) 概述（Windows 运行时应用）](https://msdn.microsoft.com/en-us/library/windows/apps/hh913756.aspx)
- [通知可视化工具应用](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager.RegisterNotificationChannelAsync() | registerNotificationChannelAsync() 方法](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx)
- [客户细分市场和推送通知：新 Windows 开发人员中心会员计划功能（博客文章）](https://blogs.windows.com/buildingapps/2016/08/17/customer-segmentation-and-push-notifications-a-new-windows-dev-center-insider-program-feature/#XTuCqrG8G5IMgWew.97)



<!--HONumber=Nov16_HO1-->


